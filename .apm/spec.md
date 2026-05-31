---
title: Desktop Return-to-Source Button
modified: Spec creation by the Planner.
---

# APM Spec

## Overview
This project adds a "Go to Playlist/Album" button in the Desktop player panel of Harmony Music. The core problem is that users cannot currently navigate back to the context screen (Playlist or Album) that the active song originated from. The essential scope includes updating the `PlaylingFrom` data model to store the source ID, updating the playback methods to pass this ID, adding the new button to the UI between the "Up Next" and "Sleep Timer" buttons, and wiring up the navigation logic. Success is defined by the absence of Dart analyzer errors and correct manual verification on the Windows build.

## Workspace
- **Working Repository:** `/workspaces/Harmony-Music-Fix-WIP`
- **Target Directories:** `lib/models/`, `lib/ui/player/`, and instances of playback execution (`lib/ui/screens/`, `lib/ui/widgets/`).
- **Frameworks:** Flutter 3.24.2+, GetX for state management.
- **Authoritative Assets:** `AGENTS.md` contains existing UI, validation, and framework patterns. Project structure definitions are derived from the `.github/copilot-instructions.md`.

---

> **Notes:**
> - The workspace uses GetX for state management and nested routing (`Get.toNamed` with `ScreenNavigationSetup.id`).
> - The Manager should be aware that manual verification must occur on Windows as requested by the user, so tasks must leave the codebase in an error-free state for compilation.

## Data Model Extensions

### Tracking Source IDs
- The `PlaylingFrom` model in `lib/models/playling_from.dart` needs a `String? id` property to store the `browseId` (the Playlist ID or Album ID).
- This ensures the UI has access to the originating location of the current playback.

## State Pipeline

### State Propagation
- Playback trigger methods in `PlayerController` (such as `playPlayListSong` and `pushSongToQueue`) must accept the source ID or a populated `PlaylingFrom` instance.
- Components triggering playback (`PlaylistScreen`, `AlbumScreen`, `ListWidget`) must pass their active `id` into `PlaylingFrom` when playback starts.

## User Interface

### Desktop Player Panel
- A new `IconButton` is inserted into `lib/ui/player/components/mini_player.dart` inside the desktop layout row (rendered conditionally via `size.width > 860`).
- **Placement:** The new button must be placed strictly between the "queue_music" (Up Next) button and the "timer" (Sleep Timer) button.
- **Iconography:** Utilize a recognizable icon aligned with the UI for returning context (e.g. `Icons.my_library_music` or `Icons.album`).

## Navigation Routing

### Return-to-Source Logic
- The button's `onPressed` logic evaluates `playerController.playinfrom.value`.
- If the type is `PlaylingFromType.PLAYLIST` and `id` is present, it routes to the Playlist screen using `Get.toNamed(ScreenNavigationSetup.playlistScreen, id: ScreenNavigationSetup.id, arguments: [null, id])`.
- If the type is `PlaylingFromType.ALBUM` and `id` is present, it routes to the Album screen via `Get.toNamed(ScreenNavigationSetup.albumScreen, id: ScreenNavigationSetup.id, arguments: (null, id))`.

