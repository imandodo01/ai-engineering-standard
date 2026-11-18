# Project Compatibility Matrix

Use this template when a project has meaningful runtime, operating-system, architecture, browser, database, or dependency compatibility requirements. Keep it in the project repository (for example, `docs/COMPATIBILITY.md`) and update it when support changes.

## Support policy

- Project: 
- Last reviewed: YYYY-MM-DD
- Compatibility owner: 
- Support policy: Explicitly state which environments are supported and which are best-effort or unsupported.

## Runtime and platform matrix

| Component | Supported version(s) | Minimum version | Tested version(s) | Verification evidence | Status / notes |
|---|---|---|---|---|---|
| Operating system | | | | | |
| Runtime / language | | | | | |
| Architecture | | | | | |
| Browser / client | | | | | |
| Database / server | | | | | |

Add or remove rows to match the project. Do not list irrelevant components.

## Key dependency constraints

| Dependency | Version constraint | Reason / compatibility note | Verification |
|---|---|---|---|
| | | | |

## Verification rules

- Distinguish **supported** environments from environments actually **tested**. A support claim is not test evidence.
- Record exact runtime, OS, architecture, and dependency versions for meaningful test runs.
- Test the oldest supported runtime/platform when compatibility risk is material; add newer versions where the project explicitly supports them.
- Identify known incompatibilities, conditional support, and untested combinations.
- Do not claim a matrix cell is verified based only on a successful run in a different environment.
- Keep test evidence traceable to CI runs, commands, reports, or release records.

## Known limitations

- 

## Change history

| Date | Change | Evidence / reason |
|---|---|---|
| YYYY-MM-DD | Initial matrix | |
