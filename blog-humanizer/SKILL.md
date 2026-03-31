---
name: blog-humanizer
description: Refine an AI-written Turkish blog draft before save so it feels more natural, fluid, and human while preserving meaning, structure, and clinical accuracy. Use for blog-generator handoff, JSON blog-post refinement, final narrative cleanup, and removal of obvious AI-writing patterns.
---

# Blog Humanizer

Use this skill after the blog draft is complete and before any DB save. Its job is not to replace the article; it should preserve the core meaning and structure while removing template-like phrasing and making the writing feel more human.

## When to use

- The draft reads too sterile, too smooth, or too obviously AI-generated
- The article structure is correct, but the narration needs a final pass
- You want a stronger sense of natural rhythm without losing clarity

## Preserve at all costs

- Required H2 sections:
  - `## Kendini Değerlendir`
  - `## Sıkça Sorulan Sorular`
  - `## Son Söz`
- Exactly 2 expert-note blocks:
  - `> 💡 **Uzman Notu:** ...`
- The legal disclaimer
- Clinical accuracy, theory framing, and core argument
- JSON shape and non-content fields unless there is a strong reason to touch them

## What to improve

- Reduce repetitive sentence openings
- Soften mechanical transitions
- Add natural variation to paragraph rhythm
- Reduce generic advice-bot language
- Where useful, add small concrete human details such as inner dialogue, body sensations, or believable daily-life texture
- Actively detect and remove these AI-leaning patterns when they appear:
  - significance inflation such as `pivotal`, `transformative`, `vital role`
  - empty abstractions and inflated framing
  - formulaic contrasts such as `not just X, but Y`
  - needless rule-of-three lists
  - synonym cycling for the same concept
  - promotional adjectives and polished marketing language
  - vague attributions with no real source
  - generic optimistic conclusions that say little
- Compress filler and hedging:
  - trim phrases that add length without meaning
  - reduce stacked hedges such as repeated `may`, `can`, `sometimes`, `in some cases`
- Prefer direct verbs over inflated constructions where clarity improves
- Let paragraph lengths vary; do not make every paragraph feel identically shaped

## Final audit pass

- After the first refinement, run one short audit pass with this question: `Does this still sound like a polished LLM draft?`
- If yes, make a second micro-pass only to remove leftover AI signals:
  - repeated paragraph openings
  - overly safe abstract bridge sentences
  - stock transitions such as `moreover`, `at this point`, `in other words` when unnecessary
  - prose that feels too frictionless and uniformly polished
- Keep this second pass narrow. Refine; do not rewrite.

## What not to do

- Do not invent new diagnoses, treatment claims, or frameworks
- Do not turn the article into poetic or overly literary prose
- Do not remove or rename required sections
- Do not merely swap words with synonyms while leaving the robotic flow intact
- Do not add unsupported statistics, studies, or authority claims
- Do not overcorrect into slang, jokes, or social-media tone

## Output expectation

- Most changes should happen inside `content`
- `summary` may be lightly refined only if it sounds mechanical; keep length constraints intact
- The result should still be ready for validation and save
- The output should reflect both a main refinement pass and a brief final audit pass while preserving the JSON shape and required blocks

## Required order

1. Draft the article
2. Place it in JSON
3. Validate
4. Apply `blog-humanizer`
5. Re-validate if needed
6. Save
