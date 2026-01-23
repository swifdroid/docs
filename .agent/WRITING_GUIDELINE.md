# Droid App Docs Writing Guideline

Published-page style and quality policy for `docs/docs/app/**`.

Read repository `AGENTS.md` and `.agent/architecture/DOCUMENTATION_SYSTEM.md` first. The local architecture owner governs truth, publication, source verification, and tracking semantics; this file governs user-facing voice, structure, and examples. Parent SwifDroid governance is not required for ordinary docs work.

If a page violates these rules, rewrite it.

## Hard bans

Do not publish any page with:

- framework meta filler ("DSL", "wrapper", "native behavior" repeated everywhere);
- section names like "API mapping", "source mapping", "related docs";
- type/parent/source path dumps;
- obvious bullets such as "you need this in a screen";
- examples that only place a widget in a random container without teaching behavior;
- generic conclusions such as "simple but powerful", "fully native", "easy to use", or other marketing filler that does not teach a concrete behavior;
- padded prose written only to make a thin API look substantial;
- repeated boilerplate explaining that every component is a wrapper around Android when that fact adds no decision-making value.

## Editorial quality bar

A component page should read like a carefully authored README for that component, not like generated API filler.

- Every paragraph must help the reader make a concrete choice, understand runtime behavior, use an exposed API correctly, or avoid a real pitfall.
- Start with the practical role of the component and the situations where it is a good fit. Do not begin with inheritance trivia or implementation metadata.
- Prefer precise, compact prose over exhaustive padding. A genuinely thin component may have a short excellent page.
- Explain defaults, state, interaction, layout behavior, lifecycle implications, and limitations only when they are relevant and verified in source.
- Distinguish what Droid actually exposes from what Android supports upstream. Never imply that an upstream API is available merely because the Android class has it.
- Reuse parent-page links for inherited behavior instead of copying large inherited method lists into every child page.
- Call out deprecation, abstract/base-class status, timing requirements, instance requirements, and surprising defaults when they affect normal use.
- Examples must feel like code a developer could adapt directly: realistic names, coherent layout, visible outcome, and no placeholder comments standing in for the important part.
- Do not add a Methods section merely for visual completeness. Document methods that are actually exposed and useful, grouped by intent.
- Avoid repetitive page endings. Finish when the reader has the information needed to use the component confidently.

## Required structure for component pages

Use this order:

1. `# Title`
2. Friendly description (2-6 short paragraphs)
3. `## Use it when…` (paragraphs only, no bullets)
4. `## How it behaves`
5. `## Examples`
6. `## Initializers` (only if there are meaningful variations)
7. `## Layout params` (only when this component has meaningful params)
8. `## Methods` (required when methods are exposed)
9. `## Notes` (optional, only for real component-specific notes)

## Writing rules

- Explain behavior in UI terms first.
- Keep sentences concrete: what the user sees, what developers control.
- Mention Droid once near the beginning if needed, then focus on the component.
- Avoid cross-framework comparison sections unless absolutely necessary.
- Internal links are allowed only inside meaningful sentences.

## Examples rules

- Layout containers: at least 2 examples, usually 3.
- Input/control widgets: at least 2 examples.
- Object-style APIs (menu, toast, snackbar): examples must show real flow, not placeholders.
- Every example must have a visible outcome.

## Methods section rules

For each documented method:

1. Add a short human explanation (what it changes and when to use it).
2. Add a small snippet that calls the method.
3. Group methods by intent when the list is long.

Do not paste signatures as documentation.

## Accuracy rules

- Document only what exists in `droid/Sources/Droid`.
- Do not invent method names, behavior, or parameters.
- If a type is thin, explain its practical role and rely on inherited behavior naturally.
- If Android deprecates a component, say it clearly and suggest practical alternatives.

## Final checklist

Before marking a page done, verify:

- No banned meta/filler sections.
- `Use it when…` is concrete and not bullet fluff.
- `How it behaves` explains actual runtime behavior.
- Examples teach realistic usage.
- Methods have explanation + snippet (when methods exist).
- No source path, type dump, or parent metadata appears in the page.
