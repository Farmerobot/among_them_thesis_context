---
trigger: always_on
---

# Among Them Thesis - Cascade Guidelines

## Project Overview

This is a LaTeX engineering thesis about the "Among Them" framework—a research platform for studying persuasion and chain-of-thought reasoning in Large Language Models through a social deduction game environment. The project involves:

- **Game Environment**: Text-based Among Us-inspired social deduction game (POMDP)
- **LLM Agents**: DeepSeek-R1 reasoning models with chain-of-thought capabilities
- **Training Pipeline**: Supervised Fine-Tuning (SFT) followed by Multi-Agent PPO (MAPPO) reinforcement learning
- **Research Focus**: Analyzing how LLMs develop persuasion strategies and whether training pressure affects reasoning faithfulness

## Writing Style Guidelines

### Priority: First-Time Reader Accessibility

**CRITICAL**: Always assume the reader knows NOTHING about the project. Do not assume prior context. Introduce concepts before using them. The thesis must be readable linearly without jumping between sections.

### Prose Over Lists

- **Avoid excessive bullet points and numbered lists**. Write flowing prose instead.
- Lists are acceptable for: game phases, action types, file structures, step-by-step algorithms, formal requirements
- Lists are NOT acceptable for: describing modules, explaining concepts, general descriptions
- Compare to published papers: they have dense paragraphs of text, not bullet-point outlines

### Paragraph and Section Hierarchy

**Terminology clarification**: In this document, "paragraph" refers to **typographic paragraphs**—blocks of text separated by blank lines in LaTeX source. This is distinct from the LaTeX `\paragraph{}` command, which is actually a sectioning heading (the lowest level, below `\subsubsection{}`).

**CRITICAL**: Maintain proper document hierarchy at all times:

1. **Multiple sentences per typographic paragraph**: A paragraph is NOT one sentence. Each blank-line-separated text block should contain 3+ sentences developing a single coherent thought.
2. **One thought per typographic paragraph**: Each text block represents one idea. Insert a blank line when shifting to a new concept.
3. **Multiple typographic paragraphs per section**: A `\section{}`, `\subsection{}`, or `\subsubsection{}` should contain multiple text blocks. If a section has only one paragraph, either expand it or merge with another section.

**Anti-pattern**: Creating a `\subsubsection{}` or `\paragraph{}` heading for every single thought—this makes the document read like a bulleted outline disguised as prose.

**When to use `\paragraph{Title:}`**: Use for **labeled inline asides** that need a visible title but are too minor for `\subsubsection{}`. Always end the title with a colon. Acceptable cases:

- `\paragraph{Design Evolution:}` — historical context distinguishing legacy from current design
- `\paragraph{Future Work Directions:}` — out-of-scope items mentioned briefly
- `\paragraph{Limitations:}` — caveats within a section
- `\paragraph{Note:}` or `\paragraph{Remark:}` — important clarifications

**Do NOT** use `\paragraph{}` as a substitute for typographic paragraphs or to label every thought. If content needs its own heading and has multiple paragraphs, use `\subsubsection{}` instead.

### When to Use `\subsubsection{}` vs Plain Paragraphs

Use the **Reference & Autonomy Decision Tree**:

1. **REFERENCEABILITY**: Would this topic be referenced elsewhere (in this doc, by readers, or in related work)?
   - YES → Candidate for header
   - NO → Go to Q2

2. **AUTONOMY**: Could this content be extracted and still make sense without surrounding paragraphs?
   - YES → Candidate for header
   - NO → Plain paragraph (part of narrative flow)

3. **LENGTH CHECK** (for candidates):
   - 3+ typographic paragraphs → `\subsubsection{}`
   - 1-2 paragraphs, labeled aside → `\paragraph{Title:}`
   - 1-2 paragraphs, main flow → plain paragraph

**Examples**:
- "Supervised Fine-Tuning" → Referenceable + Autonomous + Substantial → `\subsubsection{}`
- "History and Observation Model" → Named data structure, referenceable → `\subsubsection{}`
- "Hybrid action space" → Part of Agents narrative flow → plain paragraph
- "Design Evolution" → Labeled aside, 1 paragraph → `\paragraph{}`

**Correct structure**:
```latex
\subsection{Topic}
First paragraph with multiple sentences about aspect A...

Second paragraph with multiple sentences about aspect B...

Third paragraph with multiple sentences about aspect C...
```

**Wrong structure** (each heading has only 1 paragraph):
```latex
\subsubsection{Aspect A}
One paragraph.

\subsubsection{Aspect B}
One paragraph.

\subsubsection{Aspect C}
One paragraph.
```

### Chapter 03 vs Chapter 04 Content Separation

**Core distinction**:

| Chapter 03 (Conceptual Design) | Chapter 04 (Technical Implementation) |
|-------------------------------|--------------------------------------|
| **What** the system does | **How** the code achieves it |
| **Why** decisions were made | **Engineering challenges** encountered |
| Abstractions and interfaces | Concrete classes, functions, algorithms |
| Reader could **reimplement differently** | Reader could **understand this codebase** |

**The "Implementation Complexity" Test**:

> A topic belongs in Chapter 04 **only if there is non-trivial implementation complexity to explain**.

**Decision flowchart**:

1. Is this about **WHAT** the system does or **WHY** a choice was made?
   → Chapter 03

2. Is this about **HOW** the code works, with non-trivial complexity?
   → Chapter 04

3. Is this a trivial implementation (delete code, change constant)?
   → Chapter 03 only (as part of design rationale)

4. Is this an engineering challenge or debugging story?
   → Chapter 04 only

**Structure mapping**: Sections do NOT need 1:1 correspondence between chapters. Chapter 03's "Design Rationale" and "Requirements" sections have no Chapter 04 counterpart. Chapter 04's debugging tools and serialization sections have no Chapter 03 counterpart.

**Examples**:

| Item | Ch 03? | Ch 04? | Rationale |
|------|--------|--------|-----------|
| WAIT action removal | ✅ (why) | ❌ | Trivial implementation |
| Turn ordering randomization | ✅ (why) | ❌ | Trivial implementation |
| Tokenizer `<think>` handling | ✅ (brief) | ✅ (detail) | Non-trivial engineering |
| Attention value head | ✅ (mention) | ✅ (detail) | Non-trivial architecture |
| TWOSOME action probabilities | ✅ (mention) | ✅ (detail) | Non-trivial algorithm |
| JSON serialization format | ❌ | ✅ | Pure implementation |
| Debugging dashboard | ❌ | ✅ | Pure implementation |

### LaTeX Conventions

- Use `\texttt{}` for code identifiers, but break out of it for arrows: `\texttt{func()} $\rightarrow$ \texttt{Result}`
- Avoid long unbreakable strings in `\texttt{}`—they cause overfull hbox warnings
- The preamble has `\tolerance=1000` and `\emergencystretch=3em` to handle minor overflow
- Use `---` for em-dashes (not `--` or `-`), use `--` for en-dashes in number ranges (e.g., "pages 10--15")
- Escape underscores in text mode: `\_`

### Design Evolution Pattern

When describing system components that evolved during development, use this pattern:

1. **First**: Describe the CURRENT design as the primary content
2. **Then**: Add a `\paragraph{Design Evolution:}` subsection explaining what changed and why

This shows engineering thought process without confusing readers about what the system actually does.

### Figure Captions

- Captions should be **long and descriptive**, not just titles
- A reader should understand the figure from its caption alone without reading the surrounding text
- Include: what is shown, key takeaways, and relevance to the discussion

## Current TODOs

The following items need attention:

1. **Add Figures from Previous Publication**: Include relevant figures from the prior Among Them paper
2. **Training Pipeline Figure**: Create a diagram illustrating the training pipeline (SFT → RL flow)

## Technical Context

### Code Repository

The `context/among_them/` directory contains the **exact repository** the thesis describes—all source code with full git history. This is the implementation being documented.

### Commit History Reference

The file `context/all_summaries_combined.txt` contains **every single commit ever made** to the Among Them repository, summarized with detailed descriptions of code changes. This provides:
- **Current state context**: What the codebase looks like now
- **Temporal context**: How and why the system evolved over time
- **Design decisions**: Rationale behind architectural changes

Use this file to understand the historical evolution of any component when writing about design decisions or explaining implementation choices.

### File Structure
- `latex_project/main.tex` - Document root with preamble
- `latex_project/chapters/01-introduction.tex` - Introduction
- `latex_project/chapters/02-background-and-theory.tex` - Background
- `latex_project/chapters/03-conceptual-design.tex` - Conceptual design
- `latex_project/chapters/04-technical-implementation.tex` - Technical implementation (good example)
- `latex_project/chapters/05-experimental-evaluation.tex` - Experiments
- `latex_project/chapters/06-discussion.tex` - Discussion
- `latex_project/chapters/todo.tex` - Internal TODO tracking
- `latex_project/appendices/A-prompts.tex` - Agent prompts (`\label{app:prompts}`)
- `latex_project/appendices/B-cot-examples.tex` - Chain-of-thought examples
- `latex_project/appendices/C-hyperparameters.tex` - Training hyperparameters (`\label{app:hyperparameters}`)
- `latex_project/bibliography.bib` - References
- `context/all_summaries_combined.txt` - Complete git commit history with summaries (temporal context)
- `context/among_them/` - Full source code repository (faithfulness branch — contains RL training implementation)

### Key Labels
- `\label{ch:implementation}` - Technical Implementation chapter
- `\label{ch:background}` - Background and Theory chapter
- `\label{app:prompts}` - Prompts appendix
- `\label{app:hyperparameters}` - Hyperparameters appendix

### Key Citations
- `\cite{twosome}` - TWOSOME methodology for action probability
- `\cite{qlora}` - QLoRA for memory-efficient fine-tuning
- `\cite{deepseek_r1}` - DeepSeek-R1 reasoning model
- `\cite{spiral_rl}` - SPIRAL turn-level MDP approach
- `\cite{agile_rl}` - AGILE trajectory masking
- `\cite{amongthem2025}` - Original Among Them framework paper

## Language

- Thesis language: **English**
- All edits, suggestions, and new content must be in English
- User may provide instructions in Polish—always respond and implement in English

## Code Style

- Do not add or delete comments unless explicitly asked
- Preserve existing formatting and indentation
- When editing LaTeX, match the surrounding style exactly
