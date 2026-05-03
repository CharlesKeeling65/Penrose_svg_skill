# Penrose SVG Skill

Penrose SVG Skill is a Codex skill for generating mathematically rigorous SVG diagrams through the official Penrose workflow. It helps AI coding agents write Penrose Domain, Substance, Style, and Trio JSON files, then render final SVG assets with the official `@penrose/roger` CLI.

The goal is to avoid fragile, hand-authored SVG generation. Instead, diagrams are expressed as structured mathematical logic plus reusable visual rules, and SVG output is treated as a reproducible build artifact.

## Background

[Penrose](https://penrose.cs.cmu.edu/) is a declarative diagramming system from Carnegie Mellon University for turning mathematical notation into diagrams. A Penrose diagram separates:

- **Domain**: abstract vocabulary such as types, functions, predicates, and constructors.
- **Substance**: concrete mathematical objects and relationships for one diagram.
- **Style**: visual rendering rules, layout constraints, shapes, labels, and canvas settings.
- **Trio JSON**: a small configuration file that ties Domain, Substance, and Style together for rendering.

This repository packages those workflow rules into a reusable Codex skill at:

```text
.codex/skills/penrose_svg/SKILL.md
```

## Source

The skill was created from the official Codex `skill-creator` workflow and validated against Penrose's public documentation and the official `@penrose/roger` CLI help.

Primary references:

- Official Penrose website: <https://penrose.cs.cmu.edu/>
- Official Penrose documentation: <https://penrose.cs.cmu.edu/docs/ref>
- Official Penrose GitHub repository: <https://github.com/penrose/penrose>
- Official renderer package: <https://www.npmjs.com/package/@penrose/roger>

## What This Skill Does

Use this skill when an AI agent needs to:

- Generate Penrose source files for mathematical or technical diagrams.
- Render SVG files through `@penrose/roger`.
- Build repeatable diagram workflows for academic papers, technical docs, courseware, blogs, and static sites.
- Enforce Penrose's separation of Domain, Substance, and Style.
- Avoid direct SVG source generation.

It is not intended for photorealistic imagery, generic decorative SVG art, or workflows where SVG markup is manually written as the source of truth.

## Repository Layout

```text
.
|-- README.md
`-- .codex/
    `-- skills/
        `-- penrose_svg/
            |-- SKILL.md
            `-- agents/
                `-- openai.yaml
```

## Environment Preparation

To render Penrose diagrams, install Node.js and use the official `@penrose/roger` package.

Check the CLI:

```bash
npx @penrose/roger --help
```

Optional global install:

```bash
npm i -g @penrose/roger
```

Recommended project-local install:

```bash
npm i -D @penrose/roger
```

## Using the Skill in Codex

Place this repository where Codex can discover project-local skills, then invoke the skill explicitly:

```text
Use $penrose-svg to create a Penrose diagram of two disjoint subsets A and B inside a parent set C, renderable as SVG.
```

The skill instructs the agent to produce Penrose source files and render SVG through commands such as:

```bash
npx @penrose/roger trio --trio diagram.trio.json --out diagram.svg
```

For multiple diagrams:

```bash
npx @penrose/roger trios --trios diagrams/trios/*.trio.json --out public/diagrams
```

## Workflow Summary

1. Decompose the diagram into mathematical objects and relationships.
2. Generate Penrose Domain, Substance, Style, and Trio JSON files.
3. Validate syntax and logical consistency.
4. Render with the official `@penrose/roger` CLI.
5. Refine only Penrose source files, not generated SVG.
6. Commit both source files and rendered assets when the project tracks generated diagrams.

## License

This project is released under the MIT License. See [LICENSE](LICENSE).
