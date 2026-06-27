# Hermes Video Watch Public Packaging Research

## Goal

Package `hermes-video-watch` as a clean public Hermes Agent skill that is easy to install, inspect, and later publish through more formal distribution channels.

## Distribution option 1: GitHub repository

### How it works

Publish the folder as a normal public Git repository containing:

- `SKILL.md`
- `README.md`
- `LICENSE`
- `scripts/hermes_video_watch.py`
- `examples/`
- `references/`
- `research/`

### Pros

- Familiar to developers.
- Easy to review diffs, issues, releases, and tags.
- Works immediately with `git clone`.
- Provides a canonical public URL for attribution and documentation.
- Supports CI validation for frontmatter, script syntax, and portability scans.

### Cons

- Users still need to know where to clone the skill in their Hermes setup.
- Discoverability depends on documentation, search, and social sharing.
- Versioning discipline must be maintained manually through tags/releases.

## Distribution option 2: Hermes skills publish

### How it works

Use Hermes' publishing workflow with the skill directory path:

```bash
hermes skills publish ./hermes-video-watch
```

Current CLI help also exposes target flags:

```bash
hermes skills publish ./hermes-video-watch --to github --repo <owner>/<repo>
```

### Pros

- Best long-term user experience if Hermes supports install/search/update from a skill registry.
- Enables standardized metadata, versioning, and compatibility checks.
- Reduces manual install path errors once the skill is available by identifier.

### Cons

- Availability depends on the user's Hermes Agent version and registry support.
- Publishing requirements may evolve.
- Public review or approval may be required.

## Distribution option 3: Skill tap or direct `SKILL.md` URL

### How it works

A tap adds a GitHub repository as a skill source:

```bash
hermes skills tap add <owner>/<tap-repo>
hermes skills install hermes-video-watch
```

Hermes also supports direct HTTP(S) install from a raw `SKILL.md` URL:

```bash
hermes skills install https://raw.githubusercontent.com/<owner>/<repo>/<branch>/SKILL.md
```

For this package, the direct raw `SKILL.md` path is **not** the preferred distribution method because the skill depends on `scripts/hermes_video_watch.py`. Use raw `SKILL.md` only for a future single-file variant, or ensure the registry/tap packaging includes the full directory.

### Pros

- Easier than manual cloning when registry/tap support is configured.
- Can support curated collections of related skills.
- Direct `SKILL.md` install is simple for single-file skills.

### Cons

- A tap/index adds one more artifact to maintain.
- Direct raw `SKILL.md` install does not include bundled scripts/assets.
- Users need clear trust/update semantics for third-party taps.

## Recommendation

Start with a public GitHub repository as the source of truth. It is the most transparent and immediately usable distribution method. Keep `SKILL.md` metadata clean and semver-based so the same package can later be published through `hermes skills publish` or referenced by a skill tap/direct URL without restructuring.

Recommended release checklist:

1. Validate `SKILL.md` frontmatter.
2. Run Python syntax checks for `scripts/hermes_video_watch.py`.
3. Run a local generated-video smoke test.
4. Scan for machine-specific paths and non-portable terms.
5. Tag the first release as `v1.0.0`.
6. Add CI for the same validation checks.
