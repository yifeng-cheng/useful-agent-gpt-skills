# Implementation guide

## Recommended architecture

- `Tk` main thread: widgets, dialogs, configuration editing, coordinate overlays, and status updates.
- Worker thread: sequence execution only, using an immutable plain-Python configuration snapshot.
- `threading.Event`: stop and pause signals.
- Global hotkeys: callbacks post actions to the Tk event queue; they do not touch widgets directly.
- PyAutoGUI: keep `FAILSAFE = True`; avoid a global pause when the app already owns interruptible timing.

## Point schema

Represent each enabled step with:

```json
{
  "name": "Point 1",
  "x1": 100,
  "y1": 200,
  "x2": 140,
  "y2": 230,
  "wait_min_ms": 150,
  "wait_max_ms": 400
}
```

Normalize reversed bounds when editing or loading. A click target is sampled independently on every visit. For large regions, an optional 2–3 pixel inset avoids border pixels; do not inset tiny or zero-area regions into invalid bounds.

Backward-compatible loading:

- Legacy `x/y` becomes `x1=x2=x` and `y1=y2=y`.
- Legacy `wait_ms` becomes `wait_min_ms=0` and `wait_max_ms=wait_ms`.
- Ignore an old disabled-smooth-motion flag when the current product explicitly guarantees no jump transitions.

## Pause-correct interruptible waiting

Track a `remaining` duration, subtract elapsed active time, and reset the timing baseline after a pause. Never use a fixed wall-clock deadline without shifting it, because pause time would consume the remaining delay.

Use sleep slices around 5–20ms. Smaller slices increase wakeups without providing real-time guarantees; much larger slices make emergency stop feel sluggish.

## Interruptible curved movement

For each transition:

1. Read the current pointer position and target.
2. Compute the direction and its perpendicular unit vector.
3. Choose two ordered progress ratios, such as `0.20–0.38` and `0.62–0.84`.
4. Choose a bend side and bounded magnitude proportional to distance.
5. Evaluate a cubic Bezier curve at eased, monotonic progress values.
6. Between samples, check stop and pause events.
7. Set the exact final target coordinate if execution was not stopped.

Generate new control points for every leg. This prevents a return leg from mechanically retracing the outgoing path. Do not add overshoot unless the user explicitly needs it for a synthetic-data model; overshoot can leave safe click areas and complicate cancellation.

## Screen-region picker

Use a topmost translucent fullscreen overlay and capture absolute root coordinates on mouse press/release. Sort both axes so dragging in any direction works. A click without a drag may represent a fixed point. Always support Escape to cancel and restore the main window in a `finally`-style path.

## Thread-safety checklist

- No `StringVar.get()` or widget access from the worker thread.
- Worker-to-UI status changes are posted to the Tk queue.
- Hotkey callbacks are posted to the Tk queue.
- Shutdown marks a closing flag before unregistering hotkeys and destroying the root.
- Late callbacks catch `TclError` or are ignored after closing.

## Verification

At minimum:

```bash
python -m py_compile path/to/app.py
```

Also test these behaviors when dependencies and a desktop session are available:

- A→B and B→A use different visible paths.
- Current pointer→first point and last point→next-loop first point never jump.
- Stop and pause respond during a long movement.
- A paused wait resumes with the same remaining duration.
- Old configuration files load into the new schema.
- Reverse-direction region selection normalizes correctly.
- Invalid timing is rejected before the worker starts.
- Closing the window while hotkeys are active does not raise a Tk error.

For Windows packaging, keep the source filename, PyInstaller `--name`, displayed version, README output path, and generated executable name consistent.
