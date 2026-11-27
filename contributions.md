# Contributions ledger

Pete Tanski's ([@pdtgct](https://github.com/pdtgct)) issues, RFCs and pull requests in the [vllm-project](https://github.com/vllm-project) organization, with the fork branches that wait to be opened as pull requests.

Live status, with views by RFC, is in the GitHub Project [vLLM contributions](https://github.com/users/pdtgct/projects/1). The Project shows each issue and pull request's current state; the tables below are a dated snapshot.

Last updated 2026-11-27. Status and head commits come from GitHub on that date; the lists come from `gh search` by author, which can miss items.

`vllm-project/vllm` limits contributors without write access to 6 open pull requests ([contributing guide, "Pull Request Limits and Escalation"](https://github.com/vllm-project/vllm/blob/main/docs/contributing/README.md#pull-request-limits-and-escalation)). Pete has 10 open there, 4 of them drafts and 6 ready for review, so without bypass-list access a draft can be marked ready only after another ready pull request closes or returns to draft, and new pull requests wait on fork branches. GitHub allowed undrafting at 5 non-draft pull requests and refused it at 8, which fits a limit of 6 on non-draft pull requests.

## Plan (2026-11-27)

- RFC 1: the Mamba state layer ([pdtgct/vllm#6](https://github.com/pdtgct/vllm/pull/6)) is to replace the state pool. The production implementation is on [`pstate-impl-20261122`](https://github.com/pdtgct/vllm/tree/pstate-impl-20261122) at `9503e3b878` (tag `pstate-impl-head`), on top of the re-cut #67583 and the [vllm#69139](https://github.com/vllm-project/vllm/issues/69139) fix. It passed every suite plus four upstream batch-invariance cases on an A40 on 2026-11-26 (896 cases, none failed and none skipped). The acceptance runs have not run. #67374 and #67375 stay draft until the state-layer pull request opens. Status and the token-budget findings are in the [landscape guide](https://pdtgct.github.io/pdtgct-vllm-contrib-docs/persistent-state/landscape/#status).
- #67583, the model hooks, is re-cut as a hooks-only change on `main` (approved 2026-11-21), so it no longer depends on #67374 or #67375. The re-cut is on [`recut-67583-hooks-20261124`](https://github.com/pdtgct/vllm/tree/recut-67583-hooks-20261124) at `81b7f7c8c8`, rebased on `main` on 2026-11-24, and replaces the PR's head after its GPU tests pass.
- RFC 3: move forward. vllm-omni#9849 needs only vllm#65549, and neither depends on RFC 1. If the state layer replaces the state pool, RFC 3's section 1 and its migration step 5 need rewording.

## RFCs

| RFC | Title | Opened | Status | Pull requests |
| --- | --- | --- | --- | --- |
| [vllm#65543](https://github.com/vllm-project/vllm/issues/65543) | RFC 1: scheduler-owned persistent state, initially for cache-aware streaming RNN-T ASR | 2026-11-04 | open | [#67374](https://github.com/vllm-project/vllm/pull/67374), [#67375](https://github.com/vllm-project/vllm/pull/67375), [#67583](https://github.com/vllm-project/vllm/pull/67583); closed [#65544](https://github.com/vllm-project/vllm/pull/65544) |
| [vllm#65545](https://github.com/vllm-project/vllm/issues/65545) | RFC 2: lifecycle hooks for endpoint plugins (readiness, graceful drain and teardown) | 2026-11-04 | open | [#65546](https://github.com/vllm-project/vllm/pull/65546), [#67294](https://github.com/vllm-project/vllm/pull/67294), [#67139](https://github.com/vllm-project/vllm/pull/67139); four fork branches below |
| [vllm-omni#9848](https://github.com/vllm-project/vllm-omni/issues/9848) | RFC 3: vLLM-Omni on Model Runner V2, with stateful streaming models (Nemotron 3.5 ASR) | 2026-11-04 | open | [vllm-omni#9849](https://github.com/vllm-project/vllm-omni/pull/9849), [vllm#65549](https://github.com/vllm-project/vllm/pull/65549) |
| [vllm-omni#5212](https://github.com/vllm-project/vllm-omni/issues/5212) | Engine-managed persistent state for cache-aware streaming ASR | 2026-07-20 | open | Earlier vLLM-Omni RFC; links to vllm#65543, vllm#65545 and vllm-omni#9848 |
| [vllm-omni#5426](https://github.com/vllm-project/vllm-omni/issues/5426) | Lifecycle-aware application plugins for vLLM-Omni serving | 2026-07-26 | open | Earlier vLLM-Omni RFC; links to vllm#65543, vllm#65545 and vllm-omni#9848 |

## Open pull requests

| PR | Title | RFC | Head | State | Next action |
| --- | --- | --- | --- | --- | --- |
| [vllm#67374](https://github.com/vllm-project/vllm/pull/67374) | [Core] Persistent state: state pool, scheduling and release fence | RFC 1 | `2041ef2db3` | draft, conflicting | Close when the state-layer PR opens ([plan](https://github.com/vllm-project/vllm/issues/65543#issuecomment-7069320948)) |
| [vllm#67375](https://github.com/vllm-project/vllm/pull/67375) | [Core] Persistent state: stream admission and admitted streams in AsyncLLM | RFC 1 | `baacf2fb3c` | draft, conflicting | Close when the state-layer PR opens; admission to be rewritten for the KV cache manager. The three bot findings on #67583, all in this PR's code, are fixed |
| [vllm#67583](https://github.com/vllm-project/vllm/pull/67583) | [Core] Persistent state: model hooks for CUDA graph capture, warmup, shutdown and memory contributors | RFC 1 | `dc3d58ccc5` | draft, conflicting | Replace the head with the hooks-only re-cut `81b7f7c8c8` once its A40 round passes, then update the title and body |
| [vllm#65546](https://github.com/vllm-project/vllm/pull/65546) | [Frontend] Add endpoint plugin startup and cleanup hooks | RFC 2 | `94da6c7187` | ready, conflicting; a review requested changes on 2026-11-19 | Answer the requested changes and resolve the conflict |
| [vllm#67294](https://github.com/vllm-project/vllm/pull/67294) | [Frontend] Add endpoint plugin readiness and coordinated draining | RFC 2 | `08a8b53f18` | ready, conflicting | Review (stacked on #65546) |
| [vllm#67139](https://github.com/vllm-project/vllm/pull/67139) | [Bugfix][Frontend] Refuse new /v1/realtime sessions during shutdown. | RFC 2 | `b7ec4c2660` | ready, mergeable, review required; a review requested changes on 2026-11-17 | Answer the requested changes |
| [vllm#67144](https://github.com/vllm-project/vllm/pull/67144) | [Bugfix][Frontend] Fail speech-to-text requests that finish with error or abort | none | `3e4ab32f04` | ready, mergeable, review required | Review |
| [vllm#67151](https://github.com/vllm-project/vllm/pull/67151) | [Frontend] Pass use_beam_search and max_completion_tokens to speech-to-text models | none | `dbae7c72cf` | ready, mergeable, review required | Review |
| [vllm#65549](https://github.com/vllm-project/vllm/pull/65549) | [Core][MRv2] Build ModelState through an overridable runner method | RFC 3 | `63fbc1cde2` | ready, mergeable, review required | Review; no RFC 1 dependency |
| [vllm#70368](https://github.com/vllm-project/vllm/pull/70368) | [Bugfix][Core] Validate multimodal placeholder ranges from engine clients | none | `33445c43dd` | draft, mergeable | Fixes [vllm#70367](https://github.com/vllm-project/vllm/issues/70367). Ask for review on Slack; mark ready when a ready slot frees under the limit |
| [vllm-omni#9849](https://github.com/vllm-project/vllm-omni/pull/9849) | [Core] Use vLLM's model-state hook in the MRv2 runner | RFC 3 | `a21675ecab` | ready, mergeable, review required | Review; needs only vllm#65549 |

## Pull requests to be opened

Each branch is on [pdtgct/vllm](https://github.com/pdtgct/vllm) and waits for the open pull request count to fall below the limit.

| Branch | Tip | Change | Tracks |
| --- | --- | --- | --- |
| [`fix/streaming-resumed-session-worker-slot`](https://github.com/pdtgct/vllm/tree/fix/streaming-resumed-session-worker-slot) | `0a09c28165` | [Bugfix][Core] Count resumed streaming sessions against `max_num_seqs` | Fixes [vllm#69139](https://github.com/vllm-project/vllm/issues/69139); held, not to be opened while [vllm#69316](https://github.com/vllm-project/vllm/pull/69316) is open |
| [`serve-http-repeated-signal-exit`](https://github.com/pdtgct/vllm/tree/serve-http-repeated-signal-exit) | `5815d88a6c` | [Frontend] Exit the API server on a repeated signal after engine shutdown | RFC 2 |
| [`endpoint-plugin-authenticated-prefixes`](https://github.com/pdtgct/vllm/tree/endpoint-plugin-authenticated-prefixes) | `f57d81500f` | [Frontend] Let endpoint plugins require the API key for their routes | RFC 2 |
| [`endpoint-plugin-drain-abandonment`](https://github.com/pdtgct/vllm/tree/endpoint-plugin-drain-abandonment) | `a7af02f9e0` | Drain abandonment, with `--endpoint-plugin-drain-timeout` | RFC 2, after #67294 |
| [`endpoint-plugin-exit-tests`](https://github.com/pdtgct/vllm/tree/endpoint-plugin-exit-tests) | `72ff538c88` | Exit-status tests and docs: a failed plugin startup exits with status 3 | RFC 2 |

## Issues

| Issue | Title | Opened | Status | Fix |
| --- | --- | --- | --- | --- |
| [vllm#69139](https://github.com/vllm-project/vllm/issues/69139) | Scheduler over-admits past `max_num_seqs` when it skips a resumed streaming-input session, and the GPU worker asserts "No free indices" | 2026-11-21 | open | [vllm#69316](https://github.com/vllm-project/vllm/pull/69316) by another author, open; Pete's `fix/streaming-resumed-session-worker-slot` waits unopened, because upstream's [AGENTS.md](https://github.com/vllm-project/vllm/blob/main/AGENTS.md) says not to open a second pull request for a fix that an open one already addresses |
| [vllm#70367](https://github.com/vllm-project/vllm/issues/70367) | Engine core accepts a malformed multimodal `PlaceholderRange` from an engine client and fails in the scheduler | 2026-11-27 | open | [vllm#70368](https://github.com/vllm-project/vllm/pull/70368), draft |

## Closed

| Item | Title | Closed | Outcome |
| --- | --- | --- | --- |
| [vllm#65544](https://github.com/vllm-project/vllm/pull/65544) | [Core] Scheduler-owned persistent state for streaming models | 2026-11-14 | Closed unmerged; split into #67374, #67375 and #67583 |

## Discussions on other contributors' work

| Item | Author | Pete's part |
| --- | --- | --- |
| [pdtgct/vllm#6](https://github.com/pdtgct/vllm/pull/6), [WIP][Core] Persistent state as a standard state layer | [@LucasWilkinson](https://github.com/LucasWilkinson) | Ported the streaming ASR plugin onto it and [reported the result](https://github.com/pdtgct/vllm/pull/6#issuecomment-7039667149) |

## Projects

Public repositories that carry or support this work.

| Repository | Role |
| --- | --- |
| [pdtgct/vllm](https://github.com/pdtgct/vllm) | Fork of vLLM; branches for every vLLM pull request above |
| [pdtgct/vllm-omni](https://github.com/pdtgct/vllm-omni) | Fork of vLLM-Omni; branches for vLLM-Omni pull requests |
| [pdtgct/vllm-nemotron-asr-streaming-plugin](https://github.com/pdtgct/vllm-nemotron-asr-streaming-plugin) | Out-of-tree streaming Nemotron ASR plugin for vLLM core, built on RFC 1 |
| [pdtgct/vllm-riva-frontend](https://github.com/pdtgct/vllm-riva-frontend) | Riva and Speech NIM compatibility frontend for vLLM-Omni |
| [pdtgct/asr-framework-benchmarking](https://github.com/pdtgct/asr-framework-benchmarking) | Benchmarking harness for ASR serving frameworks |
| [pdtgct/pdtgct-vllm-contrib-docs](https://github.com/pdtgct/pdtgct-vllm-contrib-docs) | These reviewer guides and this ledger |

## Refreshing this page

Add a new issue or pull request to the Project, and set its Effort, RFC, Next action and Fork branch fields. Add a pull request that is not yet open as a draft item titled `[To open] ...`, and convert it when the pull request opens. Refresh the tables here when an RFC or effort changes.

## Future roadmap

| Item | Trigger | Shape |
| --- | --- | --- |
| State-layer pull request for RFC 1 | not scheduled; the acceptance runs have not run | one upstream pull request from `pstate-impl-20261122`; #67374 and #67375 close when it opens |
| New fork branches open as pull requests | the open pull request count falls below the limit | move each row of "Pull requests to be opened" to "Open pull requests", except `fix/streaming-resumed-session-worker-slot` while vllm#69316 is open |
