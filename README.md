# Useful Agent / GPT Skills

A growing collection of reusable skills for Codex and other agent workflows. Each skill lives in its own directory at the repository root and contains a required `SKILL.md` plus only the references, scripts, and assets it actually needs.

## Skills

### build-humanlike-autoclicker

Guidance for building maintainable Python desktop autoclickers for authorized UI testing, accessibility, and clearly labeled synthetic input-data generation. It covers randomized regions, bounded intervals, interruptible curved movement, Tkinter thread safety, configuration migration, and Windows packaging.

Path: [`build-humanlike-autoclicker`](build-humanlike-autoclicker)

### interactive-coding-coach

通过分阶段讲解、选择题和代码填空，引导用户理解编程题、选择算法和数据结构，并逐步完成符合平台框架的 Python 代码。默认使用中文互动教学。

Path: [`interactive-coding-coach`](interactive-coding-coach)

### travel-guide-app

将旅行资料整理成通用的手机旅游攻略 App：逐日行程、导航、美食、清单、本机保存、离线 PWA，以及可选的截图识别。

Path: [`travel-guide-app`](travel-guide-app)

## Example projects

- [`travel-guide-app/examples/kl-sg-trip`](travel-guide-app/examples/kl-sg-trip): 吉隆坡—新加坡旅行助手原始项目快照（非通用 skill），保留原有页面、图标和说明。来源：https://github.com/yifeng-cheng/kl-sg-trip ，快照提交：95359e49dc76f613620355645a0409aec59fae5a。

## Using a skill

Copy the selected skill folder into your Codex skills directory, or install it from this repository with your normal skill-installation workflow. Invoke it by name, for example:

```text
Use $build-humanlike-autoclicker to review and improve this Tkinter autoclicker.
```

## Adding more skills

Create one lowercase, hyphenated folder per skill:

```text
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
