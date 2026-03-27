# Contributing

Contributions are welcome! This document explains how to contribute to this project.

## How to contribute

1. Fork this repository
2. Create a feature branch (`git checkout -b feat-your-feature`)
3. Commit your changes with [Conventional Commits](https://www.conventionalcommits.org/) format
4. Push to the branch (`git push origin feat-your-feature`)
5. Open a Pull Request

## Adding a new skill

1. Create `skills/<skill-name>/SKILL.md`
2. Follow the [Agent Skills Specification](https://agentskills.io/specification)
3. Required frontmatter fields:
   - `name`: kebab-case, must match directory name
   - `description`: What the skill does AND when to use it (max 1024 chars)
   - `license`: MIT
4. Keep SKILL.md under 500 lines
5. Update README.md with the new skill entry

## Improving existing skills

- Test changes with realistic prompts before submitting
- Explain **why** the change improves the skill, not just what changed
- If modifying behavior, describe before/after examples

## Style guide

- Skill instructions are written in Japanese (this project's primary language)
- Use imperative form in instructions
- Prefer explaining "why" over rigid "MUST" rules
- Keep instructions concise; move large reference material to `references/` subdirectory

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
