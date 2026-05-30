APM_RULES {

## Scope and Change Control
- Keep each change set narrowly aligned to the active task.
- Prefer the smallest code path that fixes the behavior; avoid unrelated refactors.
- Do not expand task scope to other platforms, features, or architecture changes unless the task explicitly requires it.

## Framework and Architecture
- Follow the existing Flutter, GetX, audio_service, and media_kit/just_audio patterns already used in the touched area.
- Preserve existing service and controller boundaries unless the fix requires a local adjustment inside that boundary.
- When shared code must change, preserve behavior on unaffected platforms unless the task explicitly targets them.

## Validation
- Run the most focused analyzer or test check that can catch the touched behavior before handoff.
- If a task affects Windows behavior, include a Windows-oriented verification step in addition to local static checks.
- Treat build failures as blocking until the touched slice is corrected or the cause is clearly outside the current task.

## UI and Interaction
- Preserve unrelated input behavior when adding keyboard or pointer interactions.
- Keep desktop-only UI changes isolated to desktop surfaces when that is the requested scope.
- Keep loading, playing, and selection states consistent with observable UI behavior.

## Documentation and Lookup
- If current documentation or search tooling is added, keep it isolated from app runtime behavior.
- Prefer the lightest viable configuration that improves current implementation work.

} //APM_RULES