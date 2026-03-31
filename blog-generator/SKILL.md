---
name: blog-generator
description: Generate deep Turkish psychology blog posts with topic-gap analysis, strict structure validation, optional featured image generation, and editor-reviewed draft-first DB save; before save, pass the draft through blog-humanizer.
---

# Blog Generator

Use this skill when the user wants a new Turkish psychology blog post, a topic recommendation, a rewrite into the repo's house style, or an end-to-end production workflow.

## Default behavior

- Treat "generate a blog" style requests as end-to-end by default.
- End-to-end means: choose a coverage-aware topic, write the JSON, validate it, humanize the draft, re-validate if needed, generate a featured image if credentials exist, save to MongoDB as a draft by default, and report the saved slug or blocking error.
- Only publish immediately if the user explicitly asks for it and the content passes editorial quality checks; otherwise save as draft for review.
- Only stop before image generation or DB save if the user explicitly asks for draft-only output, required credentials are missing, or the save step fails.

## Required flow

1. Check topic coverage and duplication risk.
   - `npm run topic-stats`
   - `npm run existing-titles`
2. Write the Turkish article in consistent `sen` voice.
3. Produce post JSON.
4. Validate with `npx tsx src/scripts/validate-blog-post.ts <file>`.
5. Apply `blog-humanizer` before any DB save.
6. Re-validate if the humanizer changed structure-sensitive parts.
7. Generate a featured image if credentials exist.
8. Save with `npm run save-post -- <file>`.

## Execution notes

- Prefer topic gaps with high score and low duplication risk.
- Let `dotenv`-based scripts load `.env` themselves.
- If image generation succeeds, write `featuredImageUrl` back into the JSON before saving.
- Do not let the humanizer remove required sections, disclaimer, or expert-note blocks.
- If `tsx` commands fail in sandbox due to IPC or pipe permissions, rerun them with escalation instead of abandoning the workflow.

## Required credentials

- Image generation requires `POLLINATIONS_API_KEY` and `BLOB_READ_WRITE_TOKEN`.
- DB save requires `MONGODB_URI`.
- If credentials are missing, report exactly which variable is missing and continue as far as the environment allows.

## Content standard

- Do not reuse the same article skeleton across consecutive posts; vary the body architecture while keeping safety requirements intact.
- Choose one primary outline family per article and let the section flow reflect it:
  - Belirti -> Mekanizma -> Mudahale
  - Iliski Dongusu -> Duygusal Ihtiyac -> Onarim
  - Vaka/Sahne -> Analiz -> Genelleme
  - Mitler/Hatalar -> Gercek Cerceve -> Uygulama
  - Gelisimsel Surec -> Risk Noktalari -> Destek Plani
- Required sections may remain, but their placement and the body progression should vary by topic.
- Do not write shallow self-help filler.
- Do not invent first-hand clinical experience, years of practice, or personal client anecdotes.
- Treat the output as an expert-reviewed draft, not a substitute for editorial review.
- Explain mechanisms, not only advice.
- Include at least 2 of these frameworks with real detail:
  - BDT
  - EFT
  - Sema Terapi
  - ACT
- Use clinically plausible examples and, when useful, mention body-level processes such as threat response, amygdala, prefrontal regulation, or vagal tone.
- Prefer concrete relational or internal scenarios over generic openings.

## Required output shape

- `title`: 50-80 chars
- `summary`: 150-160 chars
- `content`: 1000-1600 words
- No H1 inside `content`
- Exactly 2 expert-note blockquotes in this format:
  - `> 💡 **Uzman Notu:** ...`
- Required H2 sections:
- `## Kendini Değerlendir`
- `## Sıkça Sorulan Sorular`
- `## Son Söz`
- `Kendini Değerlendir`: 3-5 numbered questions
- FAQ: 3-5 `###` questions
- If there is a case vignette, include `Bu bileşik bir kurgusal senaryodur`
- Never imply that the writer personally treated the case or witnessed the events.
- If medication or supplements are mentioned, include a doctor warning
- Keep the legal disclaimer at the end:

`*Bu yazı bilgilendirme amaçlıdır ve profesyonel psikolojik değerlendirme ya da tedavinin yerini almaz. Belirtileriniz günlük yaşamınızı etkiliyorsa, bir klinik psikolog veya psikiyatristle görüşmenizi öneririz.*`

## Humanizer handoff

Before DB save, apply `blog-humanizer` to reduce template-like phrasing, repetitive openings, and sterile flow while preserving structure and clinical accuracy.

- The humanizer should refine, not rewrite from scratch.
- It must preserve required sections, expert notes, disclaimer, and overall argument.
- Re-validate after the humanizer pass if needed.

## Image generation

Never pass the blog title directly into the image script. Write an English scene description that captures the article's emotional tone, metaphor, and setting. Avoid visible text-bearing objects.

## Repo commands

```bash
npm run topic-stats
npm run existing-titles
npx tsx src/scripts/validate-blog-post.ts tmp/post.json
npx tsx src/scripts/generate-image.ts "english scene description" post-slug
npm run save-post -- tmp/post.json
```
