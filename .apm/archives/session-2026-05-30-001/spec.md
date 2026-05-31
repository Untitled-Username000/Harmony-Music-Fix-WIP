---
title: Harmony Music
modified: Spec creation by the Planner.
---

# APM Spec

## Overview

Harmony Music is a Flutter-based cross-platform music streaming app focused here on Windows release quality. The current work targets three user-visible problems: unstable playback state after Windows audio-device switching, intermittent loading/search stalls in the Windows Home experience, and missing desktop keyboard navigation for Home search suggestions. The session also includes a supporting setup effort for better documentation and web lookup so the implementation work can be verified against current sources when needed. Success means the requested fixes are implemented without regressing the existing Windows release flow, and the resulting code is ready for build verification through the repository's normal Windows build path and GitHub build task.

## Workspace

The workspace is a single Flutter repository at `/workspaces/Harmony-Music-Fix-WIP` with the application source in `lib/`, platform code in `windows/`, and supporting platform projects in `android/`, `linux/`, `macos/`, and `ios/`. The project is already initialized for APM with `.apm/` templates, but `AGENTS.md` was not present at the workspace root when discovery began. Authoritative project guidance came from `README.md`, `TODO.md`, and `.github/copilot-instructions.md`; these describe the app architecture, the current feature set, and the Flutter/GetX/audio stack used by the codebase. The working target for this session is the Windows release path and the desktop app behavior it depends on; the other platform directories are present as reference context unless a change naturally touches shared code.

---

> **Notes:** The repository is on `main` and appears to be the only working repository in the workspace. `README.md` states the project is no longer maintained upstream, but the local workspace is clearly active and already contains APM scaffolding. The user wants the session ordered by priority: audio player fixes first, Windows API/loading reliability second, and desktop search navigation last. The user also wants build confidence proven through normal checks plus a GitHub build task, so execution should preserve clear validation boundaries.

## Product Scope

The requested work is limited to three end-user behaviors plus a small enabling setup item. First, the playback state must remain consistent when Windows switches output devices automatically between speakers and Bluetooth headphones, even when the underlying audio engine briefly changes state. Second, the Windows Home experience and search flow must stop getting stuck in loading/searching states and continue returning results consistently for songs, playlists, albums, and artists. Third, the desktop Home search suggestions/history dropdown must support arrow-key selection and Enter-to-search behavior while staying confined to that one search surface. Fourth, a documentation/search tooling improvement may be added if it directly supports current implementation work, but it should not broaden the product scope beyond these fixes.

## Priorities and Acceptance

The implementation order is fixed by the user: playback first, API/loading reliability second, and search keyboard navigation last. For each item, local verification must be possible before the GitHub build is used as the final confidence check. The user expects the app to compile successfully after each change set, so the work should preserve analyzer/build green signals and avoid broad refactors that would obscure build regressions. Any open questions about behavior should be resolved in terms of observable UI state, playback state, or search interaction rather than internal implementation preferences.

## Audio Playback State and Device Switching

The playback bug is a state-synchronization issue that becomes visible when Windows automatically changes audio output between speakers and Bluetooth devices. The user reports that audio usually keeps playing, but the UI can show loading until the player is nudged by scrubbing, pausing, resuming, or switching tracks; rarely, audio itself can drift until that nudge occurs. The fix should keep the play/loading indicators aligned with actual playback state after device switching, and it should avoid requiring a restart. The target platform for this work is Windows desktop, but any shared audio-state code should remain safe for the other supported desktop targets if it is touched.

## Windows Streaming and Home/Search Reliability

The Windows release path currently exhibits intermittent stuck-loading behavior on the Home tab and in the top search flow. The user did not identify a timeout problem; instead, results sometimes fail to finish loading or fail to render even though some playlist results still appear and can be played. The immediate goal is to improve the current implementation rather than replacing the YouTube API layer, while keeping the work scoped to the Windows release path. Any helper tooling added during the session should support diagnosis and verification of this current implementation, not redefine the product architecture.

## Desktop Search Keyboard Navigation

The Home search suggestions/history dropdown on desktop must support arrow-key selection within the single search surface the user described. Down and Up should move a visible highlight through the available suggestions or history entries, Enter should replace the search bar contents with the highlighted entry and immediately begin the search, and repeated Up should eventually return focus to typing mode. This feature is desktop-only and must stay isolated to the Home search suggestions/history behavior; it should not expand into category browsing or tab navigation. The existing spacebar behavior is already correct and should remain unchanged.

## Validation Expectations

The work should be organized so each deliverable can be checked locally with Flutter analysis or focused tests before the GitHub build task is used for final verification. The user wants a green build signal for confidence, so tasks should include a static or local executable check plus the external GitHub build confirmation where appropriate. Validation should report whether the UI/state bug is corrected, whether the Windows search/loading flow now returns results consistently, and whether keyboard navigation works in the narrow desktop Home search scope described above.

