---
title: Claude Prompting Best Practices — Reference
version: 1.0.0
last_updated: 2026-04-17
maintainer: Kris (updated automatically via Cowork schedule)
primary_source: https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices
secondary_sources:
  - https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview
  - https://claude.com/blog/best-practices-for-prompt-engineering
  - https://platform.claude.com/docs/en/about-claude/models/overview
applies_to_models:
  - claude-opus-4-7
  - claude-opus-4-6
  - claude-sonnet-4-6
  - claude-haiku-4-5
---

# Claude Prompting Best Practices

A living reference of current prompting guidance for the latest Claude models. This file is consumed by the Prompt Engineer project and refreshed on a schedule by a Cowork automation.

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: GOLDEN RULES  (stable — rarely changes)
     ═══════════════════════════════════════════════════════════════════ -->

## Golden Rules

The principles below are model-agnostic and have held across Claude generations. Start here for any prompt.

1. **Treat Claude as a brilliant new hire.** Assume no context about your norms, workflows, audience, or constraints. Spell it out.
2. **The colleague test.** Show your prompt to someone with no context on the task. If they'd be confused, Claude will be too.
3. **Say what you want, not what you don't want.** "Write in flowing prose" beats "don't use bullet points."
4. **Tell Claude why.** Explaining the reason behind an instruction lets Claude generalise correctly to edge cases.
5. **Specificity beats length.** A tight, concrete prompt outperforms a long vague one. The best prompt is the shortest one that reliably gets the right output.
6. **Examples are the highest-leverage lever.** One or two well-chosen examples will move output quality more than paragraphs of instructions.
7. **Give Claude permission to say "I don't know."** Reduces hallucinations materially. Example: *"If the data is insufficient, say so rather than speculating."*

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: CURRENT MODELS  (auto-update: model list + capabilities)
     ═══════════════════════════════════════════════════════════════════ -->

## Current Models (as of April 2026)

| Model | String | Best for | Default effort |
|---|---|---|---|
| Claude Opus 4.7 | `claude-opus-4-7` | Most capable; long-horizon agentic work, knowledge work, vision, memory | Use `high` or `xhigh` |
| Claude Opus 4.6 | `claude-opus-4-6` | Prior flagship; still strong for complex tasks | — |
| Claude Sonnet 4.6 | `claude-sonnet-4-6` | Balance of speed and intelligence; default to `medium` effort | `high` (default) |
| Claude Haiku 4.5 | `claude-haiku-4-5-20251001` | Fastest, cheapest; lightweight tasks | — |

**Default recommendation:** Claude Opus 4.7 unless cost or latency are binding constraints.

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: CORE TECHNIQUES  (stable — foundational)
     ═══════════════════════════════════════════════════════════════════ -->

## Core Techniques

### 1. Be clear and direct

State the task, the audience, the format, the length, and any constraints. Use numbered lists for sequential steps when order matters.

### 2. Provide context and motivation

Instead of "format this cleanly," say "format this for a technical team that values clarity over decoration." Claude generalises from the explanation.

### 3. Use examples (few-shot prompting)

Wrap examples in `<example>` tags (or `<examples>` when multiple). Good examples are:
- **Relevant** — close to the actual task
- **Diverse** — cover edge cases without signalling unintended patterns
- **Structured** — clearly delineated from instructions

Start with one example. Add more only if output still doesn't match. Three to five is typically the sweet spot.

### 4. Structure prompts with XML tags

Claude is specifically trained to parse XML-like tags. Use them to separate instructions, context, input data, and examples. Common tags:

```
<instructions>...</instructions>
<context>...</context>
<input>...</input>
<example>...</example>
<output_format>...</output_format>
```

Nest tags when content has natural hierarchy (e.g. `<documents><document index="1">...</document></documents>`).

### 5. Assign a role

A single sentence in the system prompt focuses tone and expertise. *"You are a senior financial analyst specialising in ecommerce reporting"* will shift the entire response.

### 6. Long-context prompting (20k+ tokens)

- Place long documents **near the top**, above your instructions and question. Queries at the end can lift response quality by up to ~30% on complex multi-document tasks.
- Wrap each document in `<document>` tags with `<source>` and `<document_content>` sub-tags.
- For Q&A tasks, ask Claude to **quote relevant passages first** before answering.

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: MODEL-SPECIFIC TUNING  (auto-update: shifts with each release)
     ═══════════════════════════════════════════════════════════════════ -->

## Model-Specific Tuning Notes

### Claude Opus 4.7 — current flagship behaviours

- **More literal instruction following.** Won't silently generalise from one item to others; state scope explicitly ("apply this to *every* section, not just the first").
- **Calibrates response length to task complexity.** Short on simple queries, long on open-ended analysis. If you need a specific length, ask for it.
- **Direct, less validation-forward tone.** Fewer emoji, more opinionated. If you want a warmer voice, specify it.
- **Uses tools less often and reasons more** than 4.6. If you want more tool use, raise effort or instruct explicitly.
- **Spawns fewer subagents by default.** Specify when they should and shouldn't be used.
- **Stronger design defaults** (cream/serif/terracotta) that read editorial but wrong for dashboards. Override with concrete colour/type specs, or ask it to propose options before building.
- **Dial back old "anti-laziness" prompts.** Phrasing like *"CRITICAL: You MUST use this tool"* can now cause overtriggering. Use normal language: *"Use this tool when..."*

### Effort parameter (Opus 4.7 / Sonnet 4.6)

| Level | Use for |
|---|---|
| `max` | Highest intelligence; risk of overthinking; test selectively |
| `xhigh` | Best for coding and agentic work |
| `high` | Default for intelligence-sensitive tasks |
| `medium` | Cost-sensitive; good for chat and content |
| `low` | Short scoped tasks, latency-sensitive; Opus 4.7 respects this strictly (under-thinking risk on complex tasks) |

For Opus 4.7 at `max` or `xhigh`, set `max_tokens` to **64k+** so the model has room to think and use tools.

### Adaptive thinking (Opus 4.7, Sonnet 4.6)

Use `thinking: {type: "adaptive"}`. Claude decides when and how much to think based on query complexity and the effort level. This has replaced the older `budget_tokens` pattern, which is now deprecated.

If Claude is thinking more than you'd like (common with large system prompts), add:

> *"Thinking adds latency and should only be used when it meaningfully improves answer quality. When in doubt, respond directly."*

### Prefilled responses — deprecated

Starting with 4.6 models, prefilling the last assistant turn is no longer supported. Model instruction-following is strong enough that explicit prompting achieves the same result. Common migrations:

- **Forcing JSON output** → Describe the schema in the prompt and request JSON only.
- **Eliminating preambles** → *"Respond directly without any preamble or acknowledgement."*
- **Avoiding refusals** → Provide context and clear legitimate use.

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: OUTPUT & FORMATTING  (stable)
     ═══════════════════════════════════════════════════════════════════ -->

## Output & Formatting Controls

### Match prompt style to desired output

If you want prose output, write the prompt as prose. If you want minimal markdown, strip markdown from the prompt itself. Output style tracks input style.

### Control markdown and bullets

For report-style work without heavy bullets:

```
<format>
Write in clear, flowing prose using complete paragraphs. Reserve markdown
for inline code, code blocks, and simple headings. Avoid bold and italics.
Do not use bullet points or numbered lists unless presenting discrete
items or explicitly requested.
</format>
```

### Suppress LaTeX (for business writing)

Claude 4.6+ defaults to LaTeX for maths. If you want plain text:

> *"Format all math in plain text. Do not use LaTeX, MathJax, or \( \), $, or \frac{}{}. Use /, *, ^ for division, multiplication, exponents."*

### Length calibration

Simple asks → short answers. Complex asks → longer. If you need a specific length, state it: *"Aim for ~200 words"* or *"a two-paragraph summary."*

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: TOOL USE & AGENTS  (auto-update: features evolve fastest here)
     ═══════════════════════════════════════════════════════════════════ -->

## Tool Use & Agentic Patterns

### Explicit action vs suggestion

Claude 4.7 interprets literally. *"Can you suggest changes?"* may return suggestions only. To get action: *"Implement the changes directly."*

**Default-to-action system prompt:**
> *"By default, implement changes rather than suggesting them. If intent is unclear, infer the most useful likely action and proceed, using tools to discover missing details rather than guessing."*

**Default-to-conservative system prompt:**
> *"Do not implement or modify files unless clearly instructed. When intent is ambiguous, provide information, research, and recommendations rather than taking action."*

### Parallel tool calling

Claude 4.6+ is strong at running independent tool calls in parallel. To maximise:

> *"If you intend to call multiple tools with no dependencies between them, make all independent calls in parallel. Never use placeholders for missing parameters — call sequentially when a later call depends on earlier results."*

### Subagent orchestration

Claude 4.7 spawns fewer subagents than 4.6. If you want more delegation, state when subagents are warranted (parallel workstreams, isolated context, independent fan-out). If you want less, state that simple single-file edits should be done directly.

### Long-horizon / multi-window workflows

- Ask Claude to write tests and track them in a structured file (e.g. `tests.json`) before implementation.
- Set up an `init.sh` or similar so new context windows can resume without repeating setup.
- Use git as the state-tracking layer across sessions.
- For context-aware models: *"Your context window auto-compacts as it fills. Save progress and state to memory before it refreshes. Never stop early due to budget concerns."*

### Research and information-gathering

> *"Search in a structured way. Develop competing hypotheses and track confidence levels. Self-critique your approach. Maintain a research notes file for transparency."*

### Minimise hallucinations in code

> *"Never speculate about code you haven't opened. If a file is referenced, read it before answering. Give grounded, investigation-first answers."*

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: ANTI-PATTERNS  (mostly stable — updates when behaviours shift)
     ═══════════════════════════════════════════════════════════════════ -->

## Anti-Patterns (What Not to Do)

1. **Aggressive "CRITICAL/MUST/NEVER" stacking.** Current models overtrigger on this. Use normal prompting language.
2. **Prescriptive step-by-step plans when adaptive thinking is on.** *"Think thoroughly"* often beats a hand-written checklist — Claude's reasoning frequently exceeds what you'd prescribe.
3. **Relying on `budget_tokens`.** Deprecated in favour of adaptive thinking + effort parameter.
4. **Prefilling the last assistant turn.** No longer supported on 4.6+. Migrate to explicit instructions.
5. **Over-specifying formatting when the model's default is fine.** Only add format rules when you've actually seen the output drift.
6. **Blanket "use [tool] by default" instructions.** Current models trigger tools appropriately; blanket rules cause overuse.
7. **Saying "don't X" instead of "do Y."** Positive framing outperforms negative.
8. **Assuming training-era techniques still apply.** Prompting tactics for Claude 3 / 3.5 are often wrong for 4.6+.

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: PROMPT STRUCTURE TEMPLATE  (stable — occasional refinements)
     ═══════════════════════════════════════════════════════════════════ -->

## Canonical Prompt Structure

For complex tasks, assemble prompts in this order:

1. **Role / task context** — who Claude is, what the job is
2. **Tone / voice context** — style register, brand considerations
3. **Background data** — documents, reference material, prior context (wrapped in XML tags, positioned near the top for long inputs)
4. **Detailed rules / constraints** — what to include, avoid, limits
5. **Examples** — wrapped in `<example>` tags
6. **The specific request / question**
7. **Output format specification** — structure, length, tags
8. **Thinking guidance** (optional) — *"Think step-by-step before answering"*
9. **Precognitive acknowledgement** (optional) — *"If anything is unclear, ask before proceeding"*

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: CHANGE LOG  (auto-append on each update)
     ═══════════════════════════════════════════════════════════════════ -->

## Change Log

| Date | Version | Change | Source |
|---|---|---|---|
| 2026-04-17 | 1.0.0 | Initial version. Reflects Anthropic prompt engineering best practices docs (Opus 4.7 flagship, adaptive thinking, effort parameter, prefill deprecation). | Anthropic docs |

<!-- ═══════════════════════════════════════════════════════════════════════
     SECTION: AUTOMATION NOTES  (for the Cowork update schedule)
     ═══════════════════════════════════════════════════════════════════ -->

## Notes for the Update Automation

**Primary source to monitor:**
- https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/claude-prompting-best-practices

**Secondary sources:**
- https://platform.claude.com/docs/en/about-claude/models/overview (model list + strings)
- https://www.anthropic.com/news (model releases, deprecations)
- https://claude.com/blog (prompting articles)

**Sections most likely to require updates:**
- `## Current Models` — table of available models and strings
- `## Model-Specific Tuning Notes` — add new flagship section when released; move prior flagship to "legacy notes"
- `## Tool Use & Agentic Patterns` — features evolve fastest here
- `## Anti-Patterns` — add as new behaviours emerge

**Sections that rarely change:**
- `## Golden Rules`
- `## Core Techniques`
- `## Canonical Prompt Structure`

**Update protocol suggestion:**
1. Fetch the Anthropic best practices doc and compare section headings to this file.
2. For each substantive change, append a row to the Change Log with date, summary, and source URL.
3. Increment `version` in the YAML frontmatter (minor bump for additions, major bump for structural rewrites).
4. Update `last_updated`.
5. Never remove the Golden Rules or Core Techniques sections — these are the stable foundation.
