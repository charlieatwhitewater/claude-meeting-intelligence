# Weekly Architecture Sync

**Date**: 2026-03-03
**Participants**: Sarah Chen, Mike Rodriguez, Lisa Park, Tom Anderson
**Duration**: ~3 minutes (example — real meetings will be longer)

## Summary

The team reviewed progress on the shader migration (40% performance improvement), made a final decision on texture compression formats (ASTC for mobile, BC7 for desktop), and assessed readiness for next Friday's client demo. The real-time collaboration feature will be excluded from the demo due to a cursor desync bug. A dry run is scheduled for Thursday at 2pm.

## Decisions Made

- **Texture compression**: Use ASTC for mobile builds, BC7 for desktop builds with automated conversion in the build pipeline — decided by group, proposed by Mike
- **Real-time collaboration demo**: Will NOT be shown at client demo due to cursor desync bug — decided by Sarah

## Action Items

| Item | Owner | Due | Notes |
|------|-------|-----|-------|
| Update material export settings for new shaders | Lisa Park | Thursday | Tom to review Wednesday afternoon |
| Review Lisa's material export changes | Tom Anderson | Wednesday PM | |
| Set up dual-format texture compression pipeline | Mike Rodriguez | Next sprint | Create Jira ticket after meeting |
| Finish showcase scene interactive elements + walkthrough | Tom Anderson | Wednesday | Currently 80% complete |
| Diagnose WebSocket cursor desync bug (root cause analysis) | Mike Rodriguez | Thursday | Not fixing yet, just diagnosis |
| Client demo dry run | All | Thursday 2pm | |

## Open Questions

- **WebSocket desync root cause**: Mike to investigate — is it a reconnection issue, a state sync issue, or something deeper?
- **Client demo scope**: Are there other features that should be included/excluded based on reliability?

## Key Discussion Points

### Shader Migration Complete
Mike completed the migration to the new shader system. 40% performance improvement on complex scenes. Some edge cases with transparency layers still need cleanup this week.

### Texture Compression Decision
After two meetings of discussion, the team decided on a dual-format approach: ASTC for mobile (better quality), BC7 for desktop (better quality). Adds ~3 minutes to build pipeline. Mike will automate the conversion.

### Client Demo Readiness
Showcase scene is 80% done. Interactive elements and walkthrough path still needed. Dry run Thursday at 2pm. Real-time collaboration feature excluded due to cursor desync bug after 10 minutes.

## Notable Quotes

> "Performance is up about 40% on complex scenes." — Mike Rodriguez (on shader migration)
