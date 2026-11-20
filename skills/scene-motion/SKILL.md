---
name: scene-motion
description: Compare local recipe directions, create editable drafts, discover source-backed video styles, and run the constrained tile-explainer workflow with review gates and detached jobs. Use for motion concept selection, local packs, video planning and production status; recipe previews are illustrative studies.
---

# Scene Motion

For concept exploration, use `list_content_packs`; if none are installed, `install_starter_pack` adds three original local directions. Use `inspect_content_pack`, then `compare_recipes` with one shared topic/message/label brief and format. `create_recipe_draft` creates editable documents and an illustrative static concept. These operations do not require MotionGraphics. The plugin has 28 tools; [configuration.md](references/configuration.md) covers setup and the CLI fallback.

Recipe packs are data-only, private-development and UNLICENSED. Their hashes verify bytes, not ownership or redistribution rights. `export_content_pack` writes a local archive; it does not publish. Importing a local ZIP uses the operator CLI/editor, not an arbitrary-path MCP tool. A recipe draft is not a production project, executable adapter or approved film.

For source-backed production, use `scene_motion_doctor` when the content connection is unknown and explain missing configuration from the reference. The plugin does not install creative runtimes or download models.

Named workflows are reusable recipes from the source `workflows/workflows.json`: which styles fit, which skills load and the steps in order. `list_workflows` shows those available in the current visibility mode and `describe_workflow` returns their formats, visible styles, skills and step instructions. Workflows whose styles are all hidden are not listed.

| Workflow | Makes |
|---|---|
| `explainer-short` | A short narrated vertical explainer ending on a payoff loop |
| `story-documentary` | A long story-told documentary with question-led chapters and a callback ending |
| `mechanism-3d` | How an everyday thing works, shown with one researched 3D model |
| `data-documentary` | A what-if or trend documentary carried by globes, maps and charts |
| `talking-comedy` | Talking organs, foods or objects: a fact, its caveats and a joke |
| `reference-remake` | Study a reference video, then make an original film on its structure |
| `edit-my-footage` | Tighten the user's own recordings, reframe and normalise loudness |
| `source-assets` | Find, download and credit freely licensed photos and footage |

Channel profiles hold a channel's default style, workflow, formats (design/export/fps), voice and identity; their contract is the source `channels/channel.schema.json`. `list_channels` and `describe_channel` read the user's own store (default `~/.scene-motion/channels/<id>/channel.json`). `save_channel` validates a full profile and writes only to that store, never to the content workspace. Source-workspace channels are personal: they are visible only when private mode is explicitly enabled, and a user-store profile with the same id wins. Logo paths are recorded, not copied. Never save or reveal a personal profile without the user asking.

For "Create a <channel> video about X", call `describe_channel`, then `plan_project`/`create_project` with `channel` and the topic: the channel supplies the style, format dimensions, voice and identity, and its default workflow when visible. For "Use the <workflow> workflow", pass `workflow`; it supplies a default style when none is given and adds its steps to the plan and `docs/production-recipe.md`. An explicit style or format always wins. Workflow steps are instructions, not commands, and the review gates still apply; a workflow never approves anything.

For `edit-my-footage` or `source-assets` alone, pass the named workflow and task topic without a style. `create_project` returns a workflow task folder with `docs/workflow.md`; follow the checklist and source skill there. Choose an explicit style only for a request that also needs a production plan. For an opted-in Data Fusion source profile, follow the channel instructions referenced in the plan before making dependent editorial or media choices. For `documentary-story`, the live `grounds` skill owns background selection and installation.

Honour an explicit style/channel/workflow. Otherwise infer the topic and format, use `list_styles` to narrow the choice, then `describe_style` for the selected dependency bundle. Read only the needed entry skills with `get_skill`, plus the source always-on rules and relevant references through local file tools. Private channels require opt-in; held/excluded styles are unavailable. Candidate metadata is not asset clearance.

Use `plan_project` to inspect a recipe or `create_project` when a new project is requested. Complete its brief and shotlist from the user's topic and research. Keep design/export dimensions distinct and preserve explicit project/channel choices. Card `scaffold`/`setup` prose is reference text, never executable input.

For narrated multiscene work, detailed 3D explanations or a quality rebuild, read the live `productionQuality` guide at intake and use the relevant `renderQuality` guidance before finishing. Resolve consequential choices in the brief. Map each spoken beat to an on-screen subject, action and visible result. Inspect representative native motion and encoded text, and record applicable proofs in `docs/production-quality.md` for the existing visual review. Report technical verification and visual acceptance separately.

For every narrated or dialogue production, finish research and draft the script, then read `humanizer` with `get_skill` and the source `.github/skills/video-launcher/references/script-humanize.md`. Run the pass in file mode on the actual script, using the named channel's voice sample when present. Fact-check the final wording after that pass. Record sections reviewed, tells found per line id, changed line ids and final-wording claim checks in `docs/script-review.md`; present that final script and review evidence at the existing script/shotlist approval gate before recording a voice. Preserve line tags/order, timing/caption data, existing anchors, claims/numbers/units/caveats, beginner definitions and character voice; the source guide owns the complete house exceptions. Later wording changes repeat the affected pass and claim check, and require re-voicing when audio already exists. The plugin reads the synced Humanizer skill from its source home; it does not copy it into a content pack or automatically certify prose/facts.

Only `tile-explainer` on HyperFrames is executable. Call `probe_runtime`, then `scaffold_project` for an existing tile plan. A recipe's source-style mapping does not bypass this separate production workflow. Use actual local voiceover unless the requested production is explicitly silent; silent mode uses manual `scene-motion.timeline.json` and is not generated speech. Other styles stop at planning.

Call `review_project` to show the exact current files, digest and gate state. The operator must inspect the final script/shotlist and, for narration, its script-review evidence before production, then actual local audio before narrated scene construction. Approval happens through the local operator CLI or editor, with the exact displayed digest; MCP cannot approve or revoke. Never approve on the user's behalf without their explicit decision about the current inputs. Changed inputs require renewed review, including downstream gates. Silent projects and source-independent concept drafts do not need a narration pass merely to be explored.

Use `run_step` only for the allowed `voiceover`, `build`, `check`, `snapshot`, `render` or `verify` step. It returns a job ID, not completion. Poll `job_status`; inspect bounded failures and repair the cause before a new run. `cancel_job` requests cancellation; poll for acknowledgement. Never kill stored PIDs, steal locks or silently resume interrupted jobs. Project/heavy-job locks serialize execution.

View snapshot files from their job-specific directory and exported frames/audio before visual-delivery review. For 9:16, read the shared Shorts guide before composition and inspect movement bounds and phone typography. Shorts end on the payoff and loop toward frame zero: no CTA card, sign-off, logo sting, blank ending placeholder, freeze, black or fade-out. Long form keeps its channel outro and its anchored CTA words. Inspect the last-frame-to-frame-zero cut; place a Short's question in its pinned comment and next-video pointer in Related Video. Apply Humanizer in embedded mode to upload titles, descriptions and pinned comments. Use `project_outputs` to report actual paths. A successful process or output listing is not visual approval.

Voices/images stay local; no cloud generation. Record external sources and licence obligations in CREDITS.md and preserve local font/runtime notices. Bundled plugin artifacts contain product code, not a publicly cleared creative asset pack. Report what completed, the output paths, pending reviews and limitations; do not call planning documents a finished film.
