---
name: blog-generator
description: Thin wrapper for the real codex-blog-generator automation. Use it to run, monitor, or inspect the external blog automation pipeline instead of reimplementing blog generation logic in the skill.
---

# Blog Generator

This skill is only a wrapper around the real automation pipeline.

## Single source of truth

Do not implement separate generation logic in the skill.
Do not maintain separate prompt rules here.
Do not humanize, validate, or save posts manually unless the user explicitly asks for a debugging step.

The real source of truth is:
- Repo: `/Users/ppp/Github/bloggenerator`
- Runner: `/Users/ppp/Github/bloggenerator/src/scripts/auto-blog-runner.ts`
- Validator: `/Users/ppp/Github/bloggenerator/src/scripts/validate-blog-post.ts`
- Image generation: `/Users/ppp/Github/bloggenerator/src/scripts/generate-image.ts`
- Scheduler entrypoint: `/Users/ppp/Github/bloggenerator/scripts/run-auto-blog.sh`

## Default behavior

When the user asks to generate or publish a blog post, use the real automation repo and prefer the production runner:

```bash
cd /Users/ppp/Github/bloggenerator
npm run auto-blog:run
```

When the user asks to inspect status:

```bash
cd /Users/ppp/Github/bloggenerator
npm run auto-blog:status
```

When the user asks to inspect the latest topics or coverage:

```bash
cd /Users/ppp/Github/bloggenerator
npm run topic-stats
npm run existing-titles
```

## Responsibilities of this skill

- Run the real automation commands.
- Read output files and summarize results.
- Help debug the automation pipeline if it fails.
- Point all rule changes back into `codex-blog-generator`.

## Non-responsibilities

- No standalone article writing workflow here.
- No separate outline system here.
- No separate quality gate rules here.
- No separate image prompt logic here.

If blog behavior must change, edit the automation repo, not this skill.
