---
name: penrose-svg
description: Generate mathematically rigorous, web-ready SVG diagrams exclusively through official Penrose code and the @penrose/roger CLI. Use when Codex needs to create, validate, refine, batch-render, or integrate Penrose Domain, Substance, Style, or .trio.json files for academic diagrams, technical documentation, courseware, blog posts, or frontend SVG assets.
---

# 1. Skill Overview

Use this skill to generate technical, mathematical, and academic SVG diagrams through Penrose's declarative diagramming workflow. Produce valid Penrose Domain, Substance, Style, and Trio JSON inputs, then render SVG output exclusively with the official `@penrose/roger` CLI.

Do not write direct SVG markup as the source of truth. The source of truth must be Penrose code, and the SVG must be an artifact rendered by Penrose.

| Capability | AI + Penrose | Direct AI SVG Generation |
| --- | --- | --- |
| Logical rigor | Encodes mathematical objects, predicates, and constraints explicitly in Domain and Substance. | Often mixes visual appearance with unstated logic. |
| Code quality | Separates abstract domain, concrete instance, and visual style into reusable files. | Usually produces one-off markup with tangled geometry and styling. |
| Style consistency | Reuses Style programs across many diagrams in the same domain. | Requires repeated manual SVG edits for consistency. |
| Reproducibility | Renders from versioned Penrose source and a fixed CLI command. | Depends on generated coordinates and ad hoc markup. |
| Editability | Changes are made by editing mathematical declarations or Style rules. | Changes require editing SVG elements, paths, and coordinates directly. |

Target use cases:
- Academic publishing diagrams for papers, preprints, slides, and supplementary materials.
- Technical documentation diagrams for algorithms, data structures, systems, and mathematical concepts.
- Courseware and lecture diagrams with reusable domains and styles.
- Blog posts and static sites that need web-native SVG assets.
- Frontend products that consume Penrose-rendered SVG assets.

Non-target use cases:
- Generative art where mathematical structure is not the source of truth.
- Photorealistic imagery or raster illustration.
- Non-technical decorative SVG design.
- Any workflow where the requested output source is hand-authored SVG.

# 2. Core Competencies & Proficiency Levels

## Level 1: Foundational (Single Diagram Rapid Prototyping)

Be able to:
- Generate a runnable single-diagram `.trio.json` that references one Domain file, one Substance file, and one or more Style files.
- Validate that Domain declares all types, functions, constructors, and predicates used by Substance and Style.
- Execute `roger trio --trio <file.trio.json> --out <file.svg>` or `npx @penrose/roger trio --trio <file.trio.json> --out <file.svg>`.
- Iterate visual appearance only through Style changes.
- Preserve the rule that Domain and Substance contain no visual coordinates, colors, sizes, shape declarations, or rendering logic.

## Level 2: Advanced (Batched & Engineering Workflow Integration)

Be able to:
- Create a custom Domain schema for specialized mathematical or technical domains.
- Generate many `.trio.json` configurations for bulk SVG production with shared Domain and Style files.
- Integrate `@penrose/roger` into `package.json` scripts, shell automation, static site generators, and CI jobs.
- Use `roger trios --trios <files...> --out <folder>` for batch rendering.
- Keep generated SVG files reproducible by versioning Penrose source files and the package version used to render them.

## Level 3: Expert (Full-Stack Automation & Product Integration)

Be able to:
- Encapsulate the workflow in a callable wrapper that writes Penrose source files, invokes `@penrose/roger`, and returns the generated SVG path.
- Generate HTML or frontend components that embed Penrose-rendered SVG assets without replacing Penrose as the rendering source.
- Build multimodal pipelines such as OCR to math logic extraction to Penrose code to `@penrose/roger` SVG output.
- Create reusable, domain-specific Penrose packages for teams by standardizing Domain schemas, Style programs, Trio JSON templates, validation checks, and CLI scripts.

# 3. Official Penrose Syntax & Compliance Standards

## Canonical File Definitions

- Domain Schema (`.domain`, legacy `.dsl`): Abstract type, function, constructor, predicate, and notation declarations for a mathematical domain.
- Substance Program (`.substance`, legacy `.sub`): Concrete object declarations, relationship assertions, function or constructor applications, and labels for one diagram.
- Style Program (`.style`, legacy `.sty`): Canvas declarations, visual shapes, layout constraints, objectives, layering, and styling rules.
- Trio JSON Configuration (`.trio.json`): JSON file that points `roger` to the Domain, Substance, Style, and optional variation for a diagram.

## Reserved Keywords

Treat these as Penrose language keywords and never use them as user-defined identifiers:

`forall`, `where`, `ensure`, `encourage`, `override`, `notation`, `constructor`, `type`, `function`, `predicate`

## Hard Separation Rules

- Domain declares vocabulary only: types, subtypes, functions, constructors, predicates, and notation.
- Substance declares concrete objects and logical relationships only.
- Style owns all visual rendering: canvas size, shapes, colors, sizes, constraints, objectives, text rendering, layering, and layout rules.
- Do not put graphical coordinates, pixel values, colors, shape names, or SVG logic in Domain or Substance.
- Do not generate SVG manually. Always render SVG with `@penrose/roger`.

## Domain Syntax Example

```domain
type Set

predicate Disjoint(Set s1, Set s2)
predicate Intersecting(Set s1, Set s2)
predicate Subset(Set s1, Set s2)

function Union(Set a, Set b) -> Set
constructor Singleton(Set element) -> Set
```

Validation rules:
- Every type name must be declared before use, except built-in literal types such as `String` and `Number`.
- Predicate arguments must name valid Domain types.
- Function and constructor outputs must be Domain types.
- Do not define how a set, predicate, or function looks.

## Substance Syntax Example

```substance
Set A, B, C

Subset(A, C)
Subset(B, C)
Disjoint(A, B)

AutoLabel All
```

Validation rules:
- Every object type must exist in the Domain file.
- Every predicate call must match a predicate declared in the Domain file.
- Predicate argument types must match the Domain declaration, allowing valid subtyping.
- Labels may use `AutoLabel All`, `Label object_name $tex$`, `Label object_name "text"`, or `NoLabel object_list`.

## Style Syntax Example

```style
canvas {
  width = 800
  height = 700
}

forall Set x {
  shape x.icon = Circle { }
  shape x.text = Equation {
    string : x.label
    fontSize : "32px"
  }
  ensure contains(x.icon, x.text)
  encourage norm(x.text.center - x.icon.center) == 0
  layer x.text above x.icon
}

forall Set x; Set y
where Subset(x, y) {
  ensure disjoint(y.text, x.icon, 10)
  ensure contains(y.icon, x.icon, 5)
  layer x.icon above y.icon
}
```

Validation rules:
- Declare a `canvas` block in every Style file used for final rendering.
- Use `forall` selectors to bind Domain objects.
- Use `where` selectors to bind predicate-specific visual rules.
- Use `ensure` for hard constraints and `encourage` for objectives.
- Use spaces around `+` and `-` operators in Style expressions.

## Trio JSON Example

```json
{
  "substance": "./tree.substance",
  "style": ["./euler.style"],
  "domain": "./setTheory.domain",
  "variation": "MonsoonCaterpillar95943"
}
```

Validation rules:
- `substance` must point to the Substance file.
- `style` may be a string or an array of Style files; prefer an array for consistency.
- `domain` must point to the Domain file.
- `variation` is optional; include it when reproducibility requires a fixed layout variation.

# 4. Standardized End-to-End Workflow

## Step 1. Requirement Decomposition & Definition

Instructions:
- Identify the mathematical domain, concrete objects, relationships, intended audience, output size, and required labels.
- Decide whether an existing domain pattern is enough or a custom Domain schema is needed.
- Convert user language into object types, predicates, functions, and labels.

Validation checks:
- Every visual element corresponds to a mathematical or technical object or relation.
- The requested diagram is technical or mathematical enough for Penrose.
- No direct SVG generation is required.

Compliance requirements:
- Define logic before appearance.
- Refuse to make hand-authored SVG the source of truth.

## Step 2. Penrose Code Generation (Domain/Substance/Style or Trio JSON)

Instructions:
- Write `.domain`, `.substance`, `.style`, and `.trio.json` files.
- Keep the Domain minimal and abstract.
- Keep the Substance concrete and non-visual.
- Put every visual choice into Style.

Validation checks:
- Substance uses only Domain-declared vocabulary.
- Style selectors refer only to valid types and predicates.
- Trio JSON paths are correct relative to the Trio file or the CLI working directory.

Compliance requirements:
- Do not place coordinates, shape declarations, color values, or font sizes outside Style.

## Step 3. Code Validation & Logical Compliance Check

Instructions:
- Read all generated files before rendering.
- Check type declarations, predicate arity, object names, labels, and selector bindings.
- Check that each predicate used in Substance has a Style rule when it affects geometry or layout.

Validation checks:
- Domain and Substance are mathematically consistent.
- Style has no references to undeclared types, predicates, or fields.
- Style expressions use valid Penrose syntax.

Compliance requirements:
- Fix logical mismatches in Penrose source; do not patch the SVG.

## Step 4. Rendering Test & Pre-Flight Verification

Instructions:
- Run `roger trio --trio <diagram.trio.json> --out <diagram.svg>` or the `npx` equivalent.
- If the diagram uses separate files instead of Trio JSON, run `roger trio --trio <substance> <style> <domain> --out <diagram.svg>`.
- Inspect CLI output for syntax, type, optimization, or rendering errors.

Validation checks:
- CLI exits successfully.
- SVG file exists and is non-empty.
- The output is generated by `@penrose/roger`.

Compliance requirements:
- Do not use custom SVG renderers or third-party Penrose-like CLIs in the standard workflow.

## Step 5. Style Refinement & Visual Iteration

Instructions:
- Modify only Style for visual changes.
- Adjust constraints, objectives, shape properties, layering, labels, and canvas size.
- Use `roger watch` during local iterative development when working from a folder containing `.substance`, `.style`, and `.domain` files.

Validation checks:
- Logic in Domain and Substance remains unchanged unless the meaning of the diagram changes.
- Each iteration renders successfully.
- Labels are legible and shapes do not visually contradict declared predicates.

Compliance requirements:
- Avoid hard-coding layout unless the Style rule genuinely requires it.
- Preserve reusable Style rules when making diagram-specific refinements.

## Step 6. Standard SVG Output via Official CLI

Instructions:
- Render final single diagrams with `roger trio --trio <diagram.trio.json> --out <diagram.svg>`.
- Render batches with `roger trios --trios <diagram1.trio.json> <diagram2.trio.json> --out <output-folder>`.
- Use `--tex-labels` only when plain TeX strings are required in Equation shapes.
- Use `--variation <name>` when overriding or fixing a variation from the command line.

Validation checks:
- Final SVG opens in a browser or static site preview.
- File path and filename match the delivery target.
- The source `.domain`, `.substance`, `.style`, and `.trio.json` files are saved with the SVG.

Compliance requirements:
- SVG output is an artifact; Penrose source remains the editable source of truth.

## Step 7. Asset Delivery & Version Control

Instructions:
- Commit Penrose source files and generated SVGs when the project tracks built assets.
- Record the `@penrose/roger` version in `package.json`, lockfile, CI logs, or project documentation.
- Keep generated SVGs in a predictable directory such as `public/diagrams`, `static/diagrams`, or `docs/assets/diagrams`.

Validation checks:
- Paths in Trio JSON remain portable.
- Build scripts can regenerate the SVGs from a clean checkout.
- No manual SVG edits are required after rendering.

Compliance requirements:
- Version the Penrose code that produced every delivered SVG.

# 5. Scene-Specific Workflow Presets

## 1. Rapid Prototyping Preset (1-Minute Single Diagram)

Use when the user needs one quick technical diagram.

Rules:
1. Create one minimal Domain file.
2. Create one Substance file with concrete objects and relationships.
3. Create one Style file with a canvas and one rule per object or predicate.
4. Create one `.trio.json`.
5. Run `npx @penrose/roger trio --trio diagram.trio.json --out diagram.svg`.
6. If rendering fails, fix Penrose source and rerun.

## 2. Academic Publishing Preset (Peer-Reviewed Journal/Conference Papers)

Use when the diagram must be precise, reproducible, and citation-ready.

Rules:
1. Translate claims into typed objects and predicates before writing Style.
2. Use stable labels, TeX labels where appropriate, and a fixed `variation`.
3. Keep figure-specific Substance files and reusable Domain/Style files.
4. Render with `roger trio --trio figures/<name>.trio.json --out figures/<name>.svg`.
5. Review SVG in the manuscript or paper build.
6. Commit source, SVG, and package lockfile or rendering environment metadata.

## 3. Technical Documentation Preset (Batched Courseware/Blog Post Diagrams)

Use when producing many related diagrams.

Rules:
1. Create shared `domain/` and `styles/` directories.
2. Create one Substance file and one Trio JSON file per diagram.
3. Store Trio files in `diagrams/trios/`.
4. Render with `roger trios --trios diagrams/trios/*.trio.json --out public/diagrams`.
5. Reference generated SVGs from Markdown, MDX, HTML, or the static site asset pipeline.

## 4. Interactive Web Preset (Frontend HTML+SVG Components)

Use when a web app needs Penrose-rendered assets.

Rules:
1. Generate SVGs with `@penrose/roger` before frontend bundling.
2. Store SVGs in the framework's public/static asset directory.
3. Build HTML, React, Astro, or other components that reference the generated SVG file.
4. Do not inline hand-authored SVG as a replacement for Penrose output.
5. Re-render assets in the build or prebuild step when Penrose source changes.

# 6. Official CLI Integration Specification

## Installation

Use one-shot execution:

```bash
npx @penrose/roger --help
```

Install globally when the environment permits global npm packages:

```bash
npm i -g @penrose/roger
```

## Canonical CLI Commands

Generate one SVG from a Trio JSON file:

```bash
roger trio --trio diagram.trio.json --out diagram.svg
```

Generate one SVG from three Penrose files:

```bash
roger trio --trio diagram.substance diagram.style diagram.domain --out diagram.svg
```

Use `npx` without global installation:

```bash
npx @penrose/roger trio --trio diagram.trio.json --out diagram.svg
```

Watch the current folder for local development changes to `.substance`, `.style`, and `.domain` files:

```bash
roger watch
```

Run watch mode on a specific WebSocket port:

```bash
roger watch --port 9160
```

Generate multiple SVGs from multiple `.trio.json` files:

```bash
roger trios --trios diagram-a.trio.json diagram-b.trio.json --out output
```

Use the documented positional form for batch generation:

```bash
roger trios diagram-a.trio.json diagram-b.trio.json --out output
```

## Standard Output Rules

- Use `-o` or `--out` for SVG output paths.
- For `roger trio`, `--out` is the SVG filename.
- For `roger trios`, `--out` is the output folder containing SVG files.
- Do not assume `roger trio` writes SVG markup to stdout.

## npm `package.json` Scripts

```json
{
  "scripts": {
    "diagram": "roger trio --trio diagrams/tree.trio.json --out public/diagrams/tree.svg",
    "diagrams": "roger trios --trios diagrams/trios/tree.trio.json diagrams/trios/lattice.trio.json --out public/diagrams",
    "diagrams:watch": "roger watch"
  },
  "devDependencies": {
    "@penrose/roger": "3.3.0"
  }
}
```

## Shell Automation

```bash
#!/usr/bin/env bash
set -euo pipefail

mkdir -p public/diagrams
npx @penrose/roger trio --trio diagrams/tree.trio.json --out public/diagrams/tree.svg
npx @penrose/roger trios --trios diagrams/trios/*.trio.json --out public/diagrams
```

## Static Site Generators

Hugo:

```json
{
  "scripts": {
    "prebuild": "roger trios --trios diagrams/trios/*.trio.json --out static/diagrams",
    "build": "npm run prebuild && hugo"
  }
}
```

Astro:

```json
{
  "scripts": {
    "prebuild": "roger trios --trios diagrams/trios/*.trio.json --out public/diagrams",
    "build": "npm run prebuild && astro build"
  }
}
```

VitePress:

```json
{
  "scripts": {
    "prebuild": "roger trios --trios docs/diagrams/trios/*.trio.json --out docs/public/diagrams",
    "docs:build": "npm run prebuild && vitepress build docs"
  }
}
```

## GitHub Actions CI/CD

```yaml
name: diagrams

on:
  push:
  pull_request:

jobs:
  render-diagrams:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm
      - run: npm ci
      - run: npm run diagrams
```

# 7. Standard Prompt Templates

## 1. Basic Single Diagram Trio JSON Generation Template

```text
Use Penrose only. Do not generate SVG directly.

Create a complete runnable Penrose diagram for: [describe diagram].

Output exactly four files:
1. [name].domain
2. [name].substance
3. [name].style
4. [name].trio.json

Requirements:
- Domain declares only abstract types, predicates, functions, constructors, and notation.
- Substance declares only concrete objects, relationships, and labels.
- Style contains all visual rendering, canvas size, shapes, constraints, objectives, and layering.
- Trio JSON references the three files and includes a fixed variation if reproducibility is needed.
- Include the exact `roger trio --trio [name].trio.json --out [name].svg` command.
```

## 2. Custom Domain Schema Generation Template

```text
Use official Penrose Domain syntax only.

Design a Domain schema for: [domain].

Include:
- Type declarations and subtype relationships.
- Predicate declarations with typed arguments.
- Function and constructor declarations only when needed.
- No visual rendering, coordinates, colors, shapes, or layout constraints.

Then provide one minimal Substance example that type-checks against the Domain.
```

## 3. Style File Modification & Refinement Template

```text
Refine only the Penrose Style program below. Do not change Domain or Substance unless there is a logical bug.

Goal: [visual refinement goal].

Constraints:
- Preserve all mathematical meaning.
- Keep all visual logic in Style.
- Use valid Penrose selectors, shape declarations, constraints, objectives, and layering.
- Do not generate or patch SVG directly.

Return the full revised Style file and the exact `roger trio` command to render it.

[paste Domain]
[paste Substance]
[paste Style]
```

## 4. Batched Diagram Generation Template

```text
Create a batched Penrose diagram workflow for: [diagram set].

Output:
- One shared Domain file if the diagrams share a domain.
- One or more shared Style files.
- One Substance file per diagram.
- One `.trio.json` file per diagram.
- One `roger trios --trios ... --out ...` command.

Rules:
- Do not duplicate Domain logic unnecessarily.
- Keep diagram-specific facts in Substance.
- Keep visual consistency in shared Style.
- Use only `@penrose/roger` for SVG generation.
```

# 8. Error Handling & Troubleshooting

## Syntax Errors

Classification:
- Domain parse error: malformed `type`, `predicate`, `function`, `constructor`, or `notation`.
- Substance parse error: invalid object declaration, predicate call, assignment, or label.
- Style parse error: invalid selector, assignment, shape declaration, expression, constraint, or objective.
- Trio JSON error: invalid JSON or incorrect paths.

Resolution:
1. Read the first reported line and column.
2. Fix the earliest syntax error first.
3. Check punctuation, parentheses, braces, commas, and semicolons.
4. In Style expressions, add spaces around `+` and `-` operators.
5. Rerun `roger trio`.

## Constraint Conflict Errors

Resolution:
1. Identify which `ensure` constraints cannot all be satisfied.
2. Check whether the logical predicates in Substance are contradictory.
3. Convert lower-priority hard constraints from `ensure` to `encourage` when the relation is aesthetic rather than mandatory.
4. Increase canvas size or relax spacing constraints in Style.
5. Rerun with the same variation to compare changes.

## CLI Execution Failures

Resolution:
1. Run `npx @penrose/roger --help` to confirm the package is available.
2. Run `npx @penrose/roger trio --help` to confirm command syntax.
3. Confirm the Trio JSON paths resolve from the working directory.
4. Confirm `--out` points to a writable SVG path for `trio` or a writable folder for `trios`.
5. Install locally with `npm i -D @penrose/roger` when CI or package scripts require a pinned dependency.

## Rendering Artifacts

Resolution:
1. Adjust Style constraints, objectives, layering, shape properties, or canvas size.
2. Use a fixed `variation` to make iteration comparable.
3. Keep predicate-driven visual rules aligned with mathematical meaning.
4. Do not manually edit the generated SVG.

## Hard Failure Fallback Workflow

1. Reduce the diagram to the smallest Domain, Substance, and Style that should render.
2. Render the minimal case with `roger trio`.
3. Add one predicate, object group, or Style block at a time.
4. Stop at the first failing addition and fix the Penrose source.
5. If the requested diagram is not expressible as rigorous Penrose logic, explain the limitation and propose a simpler Penrose-compatible specification.

# 9. Validation & Compliance Checklist

Complete this checklist before delivery:

- [ ] Domain uses official Penrose declarations only.
- [ ] Substance contains no coordinates, colors, shape declarations, pixel values, or rendering logic.
- [ ] Style contains all visual rendering and layout logic.
- [ ] Every Substance object type exists in Domain.
- [ ] Every predicate, function, and constructor call matches the Domain declaration.
- [ ] Every Style selector references declared types and predicates.
- [ ] Trio JSON is valid JSON and points to existing files.
- [ ] `roger trio` or `roger trios` runs successfully.
- [ ] SVG output exists, is non-empty, and opens in a browser or previewer.
- [ ] The generated SVG was not manually edited.
- [ ] The diagram is mathematically/logically consistent.
- [ ] Source files and rendering command are ready for version control.
- [ ] The `@penrose/roger` version is pinned or recorded for reproducibility.

# 10. Best Practices & Anti-Patterns

## Best Practices

1. Start by modeling the domain vocabulary before thinking about visual design.
2. Keep Domain files small, abstract, and reusable.
3. Use Substance files to express only concrete facts for one diagram.
4. Put all shapes, colors, labels, layout constraints, and canvas settings in Style.
5. Prefer reusable Style rules over diagram-specific visual patches.
6. Fix CLI and parser errors in Penrose source, not in generated SVG.
7. Use fixed variations for publication and regression-sensitive workflows.
8. Use `roger trios` for documentation sets and courseware batches.
9. Pin or record the `@penrose/roger` version in automated workflows.
10. Version the Penrose source that generated every delivered SVG.

## Anti-Patterns

1. Directly writing SVG markup: violates the Penrose-only rendering mandate.
2. Adding coordinates or colors to Substance: breaks separation of concerns.
3. Adding shape declarations to Domain: Domain is abstract vocabulary only.
4. Inventing undocumented CLI flags: causes non-runnable workflows.
5. Treating generated SVG as editable source: destroys reproducibility.
6. Using `ensure` for every aesthetic preference: can create unsatisfiable constraints.
7. Duplicating domains for small diagram changes: creates drift and inconsistent semantics.
8. Encoding mathematical contradictions in Substance and hiding them in Style: undermines rigor.
9. Omitting Trio JSON for repeated workflows: makes rendering harder to automate.
10. Delivering unrendered Penrose code as complete SVG work: fails the required CLI verification step.
