# Pixel Point Skills

Reusable skills for AI coding agents, maintained by [Pixel Point](https://github.com/pixel-point).

## Install

With Node.js 22.20 or later and Git installed, run this from the project where you want to use the skill:

```sh
npx skills add pixel-point/skills --skill compress-video
```

The CLI lets you choose your coding agent. Add `-g` to install globally instead of in the current project.

Browse all skills in this repository without installing them:

```sh
npx skills add pixel-point/skills --list
```

To choose from the collection interactively:

```sh
npx skills add pixel-point/skills
```

## Available skills

| Skill | What it does |
| --- | --- |
| [compress-video](skills/compress-video/SKILL.md) | Compresses videos locally with FFmpeg, compares codec/CRF settings, audits embedded videos, and scans and optimizes videos across a website project. |

Example requests after installation:

- "Compress `demo.mov` for my website."
- "Compare VP9 at CRF 36, 42, and 48 with H.265 at CRF 24, 30, and 36."
- "Compress `demo.mov` and also create an AV1 version."
- "Audit the videos in `./website` used on `https://example.com/blog/post`."
- "Scan and optimize all videos used by this project at `http://localhost:3000`."

The video skill needs an agent that can run terminal commands. It checks for FFmpeg and FFprobe and installs missing dependencies.

Website workflows require browser tools and access to the website's source code. Run them from its workspace or provide the project path. The agent inspects the source first, then uses a live or local URL to verify rendered video sizes and playback. If no URL is supplied, it can reuse or start the project's development server.

Commands in Codex:

```text
$compress-video demo.mov
$compress-video compare demo.mov
$compress-video audit ./website https://example.com/blog/post
$compress-video scan-and-optimize ./website http://localhost:3000
```

These are agent requests, not shell commands. Natural-language requests work too. `audit` reports on the specified page or scope. `scan-and-optimize` discovers videos across the whole project by default, checks their rendered sizes, compresses and downscales suitable videos, updates references, and verifies playback. Quality comparisons run only when requested. An explicit narrower scope limits the work.

## Repository structure

```text
skills/
├── LICENSE
├── README.md
└── skills/
    └── compress-video/
        ├── LICENSE
        ├── SKILL.md
        └── references/
            ├── compression.md
            ├── comparison.md
            ├── audit.md
            └── scan-and-optimize.md
```

Each skill lives in its own `skills/<skill-name>/` directory. The Skills CLI discovers this layout automatically. The repository root is the collection, so it does not contain a `SKILL.md`.

## Add a skill

1. Create `skills/<skill-name>/SKILL.md`.
2. Start it with YAML frontmatter containing `name` and `description`. Use a unique lowercase, hyphenated name that matches the directory.
3. Write the instructions below the frontmatter. Keep any needed `scripts/`, `references/`, or `assets/` inside that skill's directory so it can be installed independently.
4. Copy the root `LICENSE` into the skill directory so the license notice travels with standalone installations.
5. Add the skill to the table above and verify discovery from the repository root:

   ```sh
   npx skills add . --list
   ```

See the [Agent Skills specification](https://agentskills.io/specification) and the [Skills CLI discovery rules](https://github.com/vercel-labs/skills/blob/main/docs/creating-skills.md).

## Publish and appear on skills.sh

Publish the skill directories to this public GitHub repository's default branch. The installation commands above then fetch the published files directly from GitHub. This layout needs no npm package, custom discovery manifest, or build step.

Installation through the Skills CLI and appearance in the skills.sh directory are separate. According to the [skills.sh FAQ](https://skills.sh/docs/faq), skills appear automatically through installation telemetry when users install them with `npx skills add`. A Git push alone does not immediately create a leaderboard entry.

Once indexed, the directory page for this skill follows this URL:

[skills.sh/pixel-point/skills/compress-video](https://skills.sh/pixel-point/skills/compress-video)

## License

[MIT](LICENSE). Each skill includes a copy of the license for standalone installation.
