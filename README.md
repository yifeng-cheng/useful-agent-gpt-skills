# Useful Agent / GPT Skills

A growing collection of reusable skills for Codex and other agent workflows. Each skill lives in its own directory under `skills/` and contains a required `SKILL.md` plus only the references, scripts, and assets it actually needs.

## Skills

### build-humanlike-autoclicker

Guidance for building maintainable Python desktop autoclickers for authorized UI testing, accessibility, and clearly labeled synthetic input-data generation. It covers randomized regions, bounded intervals, interruptible curved movement, Tkinter thread safety, configuration migration, and Windows packaging.

Path: [`skills/build-humanlike-autoclicker`](skills/build-humanlike-autoclicker)

## Using a skill

Copy the selected skill folder into your Codex skills directory, or install it from this repository with your normal skill-installation workflow. Invoke it by name, for example:

```text
Use $build-humanlike-autoclicker to review and improve this Tkinter autoclicker.
```

## Adding more skills

Create one lowercase, hyphenated folder per skill:

```text
skills/
  another-useful-skill/
    SKILL.md
    agents/openai.yaml       # optional UI metadata
    references/              # optional detailed guidance
    scripts/                 # optional reusable helpers
    assets/                  # optional output assets/templates
```

Keep `SKILL.md` concise, put conditional detail in `references/`, validate new scripts, and update the Skills list above when adding a skill.

## Responsible use

Use automation only on systems you are authorized to control. The included autoclicker skill is not intended for bypassing CAPTCHAs, access controls, anti-abuse systems, or platform rules.
