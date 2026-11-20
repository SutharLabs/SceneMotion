# Local configuration and commands

The plugin bundles its Node server, detached worker, fixed recipe-preview renderer and original starter recipe data. It does not bundle the MotionGraphics content tree, creative runtimes, font files or model weights. Use Node 22 or later.

Pack listing, starter installation, inspection, comparison, local export and recipe drafting work without MotionGraphics. Set `SCENE_MOTION_PACKS` for the store (default `~/.scene-motion/packs`) and `SCENE_MOTION_PROJECTS` for draft output (default `~/SceneMotionProjects`). A version 1 config may contain only `packsRoot` and `projectsRoot`. Store and output directories must be separate and non-overlapping. `--packs` and `--projects` override these settings.

Source-backed catalogue and production commands additionally need `SCENE_MOTION_WORKSPACE`, `--workspace`, or config `sourceWorkspace`. A version 1 `scene-motion.config.json` can also specify `includePrivate`, `channelsRoot` and `runtime`; select it with `SCENE_MOTION_CONFIG` or `--config`. Config-relative paths resolve from that file; explicit values override it. A pinned source connection does not consume an unrelated cwd config unless one is explicitly selected. Pack/draft roots cannot overlap a configured source workspace.

## Channel profiles and named workflows

Channel profiles live in the user's own store: `~/.scene-motion/channels/<id>/channel.json` by default. Choose another store with `--channels`, `SCENE_MOTION_CHANNELS` or config `channelsRoot`, in that order of precedence (the environment variable is ignored for isolated editor connections). The store must sit outside the content workspace and not contain it. Profiles follow the source contract `channels/channel.schema.json` (schemaVersion 1, `id`, `name`, `style`, `formats.long`/`formats.short` with `design`, `export` and `fps`, plus optional workflows, defaults, rules, voice, identity and upload fields; channel-specific extra keys are kept). `channel new` starts a neutral profile from the source brand tokens and an optional workflow; `channel save --file` and `save_channel` validate and write atomically into the store only. Logo paths are recorded as given; binaries are not copied.

Channels under the content workspace's `channels/` folder are personal. They are read only when `includePrivate` is enabled, and a user-store profile with the same id replaces them. Folders starting with `_` are templates, not channels. Neither store is ever packaged into plugin, extension or content artifacts.

Named workflows come from the source `workflows/workflows.json`. A workflow is listed when it names no styles or at least one visible style. Its visible skills are readable with `get_skill`. An explicit style wins, followed by a compatible channel style, then the workflow default. A channel can supply a style for a workflow with no default. Plans record the selected channel and workflow. `docs/production-recipe.md` gives the steps for a production plan, and the selected source profile can reference its own editorial instructions. Channel grounds stay with the profile and apply to styles that use the `grounds` skill.

Run `new <name> --workflow edit-my-footage --topic <task>` or choose `source-assets` to create a workflow task without selecting a production style. These folders contain `scene-motion.workflow.json`, `docs/workflow.md`, `CREDITS.md`, and `AGENTS.md`. Open the checklist to load the source skills and perform the task. Runtime commands list and act on production plans.

`runtime` may specify installed `hyperframesPackage`, `browser`, `python`, `ffmpeg` and `ffprobe` paths. The executable adapter requires HyperFrames **0.9.39**, a local headless browser and existing Python/audio/FFmpeg resources. `probe_runtime` reports prerequisites; it does not install packages or download models. `SCENE_MOTION_HYPERFRAMES` can select the installed HyperFrames package directory.

A recipe draft contains `scene-motion.recipe.json`, editable brief/shotlist, local tokens, generated `previews/concept.svg`, credits and agent guidance. It is an illustrative authoring draft with pending approvals and no executable renderer. A source-backed production plan instead contains `scene-motion.plan.json`; runtime materialization creates `.scene-motion/project.json`. Public content clearance remains pending.

Source-backed plan creation also supplies `docs/script-review.md`. Its Humanizer-pass and final-wording fact-check sections are authoring evidence, not completed checks or an approval. Style/card-engine skill resolution remains the source bundle; shared narration guidance is reported separately, and `get_skill humanizer` reads the live source skill. Missing common source files are setup gaps, not permission to skip the pass.

When the source supplies the production-quality guide, plans link it and include `docs/production-quality.md` for applicable visual review evidence. The tile adapter carries the selected channel's local voice settings into its runtime configuration. Review that configuration and the actual audio through the existing gates.

The source development CLI prints host-specific connection snippets with absolute paths:

```text
node packages/cli/dist/main.js connect codex --workspace <source> --projects <output>
```

Replace `codex` with `claude`, `cursor`, `copilot`, or `vscode` for the appropriate configuration shape. This prints a snippet and does not merge files. Review it before using the host's supported configuration flow. Packaged hosts run `server/server.js`, which starts the adjacent bundled worker for jobs.

The portable variant uses `${PLUGIN_ROOT}`. The Cursor variant uses `${CURSOR_PLUGIN_ROOT}`. The Claude compatibility files use `${CLAUDE_PLUGIN_ROOT}`. Installing a local process package does not make it available on a web-only host. No plugin or account installation happens when this package is built.

## CLI fallback and tool mapping

From a built Scene Motion source checkout, run `npm run scene-motion -- <command>` (or `node packages/cli/dist/main.js <command>`). Use `--packs <store>` and `--projects <output>` for independent recipe work; add `--workspace <content-root>` for source-backed workflows. `--json` returns machine-readable results. The local CLI bin name is `scene-motion`; no public npm release is implied.

| MCP tool | CLI command |
|---|---|
| `scene_motion_doctor` | `doctor` |
| `list_styles` | `styles [--engine <id>] [--format <ratio>] [--query <text>]` |
| `describe_style` | `style <id> [--engine <id>]` |
| `list_engines` | `engines` |
| `get_skill` | `skill <id>` |
| `list_workflows` | `workflows [--format <ratio>] [--style <id>]` |
| `describe_workflow` | `workflow <id>` |
| `list_channels` | `channels` |
| `describe_channel` | `channel <id>` |
| `save_channel` | `channel save --file <profile.json>` (or `channel new <id> --name <text> [--style <id>] [--workflow <id>]`) |
| `plan_project` | `plan <name> --topic <text> (--style <id> --format <ratio> \| --channel <id> \| --workflow <id>)` |
| `create_project` | `new <name> --topic <text> (--style <id> --format <ratio> \| --channel <id> \| --workflow <id>)` |
| `probe_runtime` | `runtime` |
| `scaffold_project` | `scaffold <project> [--audio-mode voiceover\|silent]` |
| `list_projects` | `projects` |
| `project_status` | `status <project>` |
| `review_project` | `review <project> --gate <gate>` |
| `run_step` | `run <project> <step> [--times 0,1,2] [--quality draft\|delivery]` |
| `list_jobs` | `jobs` |
| `job_status` | `job <uuid>` |
| `cancel_job` | `cancel <uuid>` |
| `project_outputs` | `outputs <project>` |
| `list_content_packs` | `packs` |
| `inspect_content_pack` | `pack <id> [--pack-version <version>]` |
| `install_starter_pack` | `pack-starter` |
| `compare_recipes` | `compare --pack <id> --topic <text> --format <ratio> [--message <text>] [--label <text>] [--recipes <id,id,id>]` |
| `create_recipe_draft` | `draft <name> --pack <id> --recipe <id> --topic <text> --format <ratio>` |
| `export_content_pack` | `pack-export <id> [--pack-version <version>]` |

The starter pack `scene-motion-starter` version `0.3.0` offers `clear-grid`, `signal-flow` and `type-pulse`. Compare up to three recipes with the same brief; repeat `--label` up to three times. Four formats are supported: `16:9`, `9:16`, `1:1`, `4:5`. Previews are fixed SVG composition studies drawn from validated data; render cost is explicitly unmeasured. A chosen recipe's `sourceStyle` is a workflow mapping, not a new renderer.

`pack-export` writes `<packsRoot>/exports/<id>-<version>.zip`. `pack-install <local-archive>` imports through the operator CLI; the editor offers a local picker. MCP does not accept an arbitrary archive path. Packs require the canonical STORED ZIP format produced by `buildRecipePack`; re-zipping with general tools may be rejected. Payloads are closed-schema JSON and notice/template text, with exact inventory, bounded sizes and hashes. They carry `private-development`, `UNLICENSED`, `first-party-declared` and `publishable: false`; these declarations do not prove rights. Do not treat source candidate metadata or file hashes as public-export permission.

Pass each option as its own argument and quote multi-word topics. `new` writes planning documents; `scaffold` separately materializes the tile adapter. Only tile-explainer is executable. Steps are `voiceover`, `build`, `check`, `snapshot`, `render`, `verify`. Snapshot times are at most 12 values from 0 through 600 seconds. Card command prose is never executed. Runtime command options cannot supply arbitrary shell commands or output paths.

## Exact-input reviews

The gate names are `script-shotlist`, `audio`, and `visual-delivery`. Review output shows the exact file list, hashes, metadata and digest. After inspecting those inputs, the local operator can record the explicit decision with:

```text
scene-motion approve <project> --gate <gate> --digest <displayed-sha256>
scene-motion revoke <project> --gate <gate>
```

These two commands have **no MCP equivalent**. The editor offers document/audio review followed by an explicit digest confirmation. They record local operator attribution, not authenticated human identity. Approval uses exact bytes and stable metadata; changes invalidate it even when mtimes are preserved, and downstream gate metadata includes prior digests. The adapter rechecks required gates before running. Do not manufacture approval to unblock an agent task.

Narrated projects follow research → draft → Humanizer file-mode pass → fact-check final wording → user script/shotlist approval → local voice → actual-audio review. `docs/script-review.md` is included in their script-gate input digest, so changes to that evidence invalidate approval alongside script changes. Byte fingerprints do not establish the quality or truth of the review. Explicit silent mode uses manual timing and has no synthesized narration or mandatory narration pass to approve. New plans and materialized placeholder scripts do not constitute approved content.

The authoritative instructions are `.github/skills/video-launcher/references/script-humanize.md` and `.github/skills/humanizer/SKILL.md` in MotionGraphics. The latter is the MIT-licensed synced `blader/humanizer` skill; do not edit or vendor-copy it here. When an opted-in channel is selected, use its source voice sample: `channels/<id>/voice-sample.md` (older source workspaces used `.github/skills/<id>/assets/voice-sample.md`). Do not copy personal samples into public artifacts. Preserve line tags/order, structured `caps`/`gap` data, anchor words, claims and caveats, beginner definitions, deliberate format structure, character voice and approved ending-plan lines. Long-form CTA anchor words are protected. Any later wording edit repeats the affected Humanizer/claim checks and requires fresh audio when already voiced.

Shorts follow the source Shorts guide's Endings section: payoff, a prompt cut and a live last-frame-to-frame-zero loop; no CTA/sign-off card, logo sting, blank end placeholder, freeze, black or fade. Long form retains the channel outro. Humanizer embedded mode also applies to published titles, descriptions and pinned comments. Existing projects are not rewritten automatically; inspect their current source pins/review evidence and obtain a new decision for changed inputs. No new approval gate or automatic semantic certification is added by this workflow sync.

## Detached jobs and outputs

`run_step` returns immediately with a durable UUID. Poll until `succeeded`, `failed`, `cancelled` or `interrupted`; only `succeeded` means that step completed. Logs/progress/results live under `<projectsRoot>/.scene-motion/jobs/<uuid>/` and are returned in bounded form. Snapshot proofs use `snapshots/<job-id>/`; renders use `renders/<job-id>.mp4` and a probe report. Inspect actual artifacts and their review inputs before delivery.

Every job acquires its project lock; heavy jobs also share one render lock. A competing operation does not steal a lock. Cancellation writes a request for the live worker to stop its own process; a request is not acknowledgement. A stale heartbeat reports interruption uncertainty and retains locks for manual inspection. Never delete locks automatically, restart an interrupted request, or signal a PID read from storage.

These local operations do not provide cloud images/voices, runtime installation, remote-node provisioning, assistant configuration merging or publication. Real-host and Linux validation are separate evidence milestones; refer to the source checkout's current verification report.
