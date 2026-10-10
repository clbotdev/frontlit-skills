# FrontLit skills

Agent skills for FrontLit.

| Skill | What it does |
| --- | --- |
| [`frontlit-docs`](plugins/frontlit-docs/skills/frontlit-docs/SKILL.md) | Write FrontLit docs, or port an existing docs site (Fumadocs, Docusaurus, Mintlify, Nextra, GitBook, VitePress) to FrontLit and open a pull request |

## Install

Any agent that supports skills (Claude Code, Codex, OpenCode, Gemini CLI, Cursor, Copilot…):

```
npx skills add codelitdev/frontlit-skills --skill frontlit-docs
```

Claude Code plugin:

```
/plugin marketplace add codelitdev/frontlit-skills
/plugin install frontlit-docs@frontlit
```

## Use

Connect your repository in FrontLit (Settings → Integrations → GitHub) first. Then, in your repository:

```
Use the frontlit-docs skill to port my existing docs to FrontLit and open a pull request.
```

## Keeping in sync

`references/format.md` and `scripts/check.mjs` mirror FrontLit's Markdown importer and validator. Update them whenever those rules change in the FrontLit repository.
