---
name: build-humanlike-autoclicker
description: Build or improve a Python desktop autoclicker for legitimate UI testing, accessibility, or synthetic input-data generation, especially when it needs randomized click regions, bounded timing, interruptible curved mouse movement, Tkinter controls, configuration migration, or Windows packaging. Do not use for defeating access controls, CAPTCHAs, anti-abuse systems, or platform rules.
---

# Build Humanlike Autoclicker

Create a maintainable desktop automation utility rather than a one-off click loop. Preserve the user's existing project and configuration format when modifying an app.

## Workflow

1. Inspect the current source, README, build scripts, configuration examples, and repository status before editing.
2. Identify the UI thread, worker thread, stop/pause events, click-sequence schema, coordinate picker, and packaging entrypoint.
3. Implement requested behavior while preserving these invariants:
   - Tk widgets and variables are read or mutated on the Tk main thread. Snapshot UI settings before starting the worker.
   - Global-hotkey callbacks enqueue UI actions with `root.after`; ignore callbacks arriving during shutdown.
   - Waiting and movement remain interruptible in short steps and preserve remaining time across pauses.
   - Every configured transition, including current position to the first point and the last point to the next loop's first point, uses the selected movement policy.
   - Validate nonnegative timing, `minimum <= maximum`, positive movement duration when jumps are forbidden, and at least one enabled point.
   - Keep PyAutoGUI failsafe enabled and provide an explicit emergency-stop hotkey.
4. Maintain backward-compatible configuration loading. Convert fixed `x/y` points to zero-area regions and max-only waits to `minimum=0, maximum=old value`.
5. Update user-visible version strings, documentation, and packaging names together.
6. Run syntax compilation and focused logic checks. State clearly when real pointer/hotkey testing was unavailable.

## Movement and timing

Use a freshly generated cubic Bezier path for each transition when curved movement is requested. Randomize control-point ratios, perpendicular bend direction, bend magnitude, and duration within documented bounds. Use monotonic eased progress, finish exactly at the target, and avoid adding unbounded jitter that can leave the selected region.

Treat millisecond timing as best-effort: desktop Python and operating-system scheduling are not real-time. Explain that click-to-click time equals movement time plus post-click wait time. Label generated trajectories as synthetic when they are intended for datasets; do not claim they are indistinguishable from real user data.

For detailed implementation patterns, configuration migration, and verification checks, read [references/implementation-guide.md](references/implementation-guide.md).

## Scope and safety

Support automation only on systems and applications the user is authorized to control. If the stated goal is evading detection, bypassing verification, or defeating rate limits, do not add stealth or anti-detection behavior. Offer deterministic test automation, official APIs, or clearly labeled synthetic-data generation instead.
