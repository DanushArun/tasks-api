# Tasks API — Evaluation guide

Start with the smallest path that exercises the project. Distinguish source inspection,
syntax/build checks, functional behavior and domain validation when recording a result.

## Guided reading and demonstration

1. **Check health.** Start the process and request GET /health. A health response establishes
process behavior, not database connectivity or service uptime.

2. **Create a task.** POST a JSON object with a nonblank title. The service trims it, assigns a
UUID, sets done false and records a creation timestamp.

3. **List the store.** GET /tasks returns tasks from the current app store. A fresh app has an
empty store; there is no cross-process persistence.

4. **Inspect failure paths.** Malformed JSON, missing/non-string/blank titles and unsupported
routes have explicit error responses. The exported title-length constant is not enforced.

## Declared checks

These commands/checks describe the intended verification path. Their presence in this
guide does not claim that they passed. See the dated evidence below and the README for setup.

```text
npm test
```

## Evidence levels

| Level | What it establishes | What it does not establish |
| --- | --- | --- |
| Source review | A path exists in tracked code | Successful runtime behavior |
| Syntax/build | Parser/compiler accepts that path | End-to-end correctness |
| Behavioral check | A specific input/output case passed | Generalization beyond cases |
| Domain evaluation | Performance on a stated target setting | Other users/data/environments |

## What to record

- Commit, environment, dependency versions and date.
- Input provenance and whether data is synthetic, public or privately supplied.
- Absolute pass/fail/skip counts; keep failed cases and their root causes.
- Whether external services, hardware or a production deployment were actually exercised.
- Expected output and an artifact showing the observation.

## Review scenarios

- **Framework-free HTTP:** Node primitives keep route/store behavior visible.

- **Isolated app stores:** Each created app can be tested without shared global task state.

- **Limits are explicit:** An exported constant is not proof that a validation rule enforces it.

## Documentation inspection — 7 October 2026

The documentation was traced to committed source and checked for local links, balanced
code fences and supported implementation claims. Historical notebook outputs remain labeled
as historical. Live provider access, private databases and hardware behavior are not inferred
from configuration or dependency files. Any fresh run is recorded separately in the README.

## Next evidence to collect

- Record deployment/body-limit assumptions if expanding beyond learning use.
- Test persistence and authorization only after they are implemented.
- Retain explicit error-path behavior in future changes.

## Fresh checks

The README records fresh checks on 7 October 2026, including commands and absolute results.
Those measured checks supersede an inspection-only description for the paths they cover.
