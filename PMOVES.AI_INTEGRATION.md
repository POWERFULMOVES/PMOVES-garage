# PMOVES.AI integration: PMOVES-garage

Fork of `deuxfleurs-org/garage` (GitHub mirror of `git.deuxfleurs.fr/Deuxfleurs/garage`).
PMOVES.AI uses it as the object store behind JuiceFS `pmoves-media`; the plan is
`pmoves/docs/architecture/JUICEFS_GARAGE_MIGRATION_PLAN.md` in PMOVES.AI.

| Branch | Role | Moves by |
|---|---|---|
| `main-v2` | Parity with upstream `main-v2` | `fork-sync.yml` in PMOVES.AI (mapping override `main-v2`) |
| `PMOVES.AI-Edition-Hardened` | Release pin, cut from tag `v2.4.1` (`268334bd2`), plus this overlay | Deliberate, tag-based promotion only. Never an automatic merge of `main-v2` |

Both branches carry the fleet's standard protection (PR required, no force-push, no deletion,
conversation resolution).

## Overlay

- `.github/workflows/pmoves-ghcr.yml`: builds the binary with upstream's own release recipe
  (`nix-build --attr releasePackages.<arch>`, `.woodpecker/release.yaml`), packages it with the
  upstream `Dockerfile` unchanged, and publishes a multi-arch (amd64, arm64) image to
  `ghcr.io/powerfulmoves/pmoves-garage/garage`.
- No Garage source is modified.

## Consuming the image

Pin the manifest-list **digest** printed in the workflow's step summary, never a tag.

## Promoting to a new upstream release

1. Push the new upstream tag to this fork.
2. Open a PR into `PMOVES.AI-Edition-Hardened` that merges the tag.
3. After the image builds, repin the digest in PMOVES.AI.
