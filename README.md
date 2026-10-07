# Tasks API — In-Memory HTTP Service

> Plain Node.js, zero runtime dependencies, explicit HTTP behavior.
> Tasks exist only for the lifetime of one server process.

A small tasks service for learning and testing HTTP APIs. It lists tasks, creates them from a
validated title, and exposes a health endpoint. The implementation uses Node's HTTP server,
UUID generation and a Map rather than a framework or database.

## Evaluate locally

Requires Node.js **24 or newer**, as declared in [package.json](package.json).
From the repository root:

```sh
npm test
npm start
```

In another terminal:

```sh
curl http://localhost:3000/health
curl http://localhost:3000/tasks
curl -X POST http://localhost:3000/tasks \
  -H 'Content-Type: application/json' -d '{"title":"Write tests"}'
```

Expected: health returns `{"status":"ok"}`, a fresh store returns `{"tasks":[]}`, and creation
returns HTTP 201 with `id`, a trimmed `title`, `done: false` and an ISO `createdAt` timestamp.
Use `PORT=3001 npm start` to choose another port. Restarting empties the store.

**Verified 7 October 2026:** `npm test` passed **10/10 tests**, with zero failures or skips,
using Node v26.0.0. This checks local HTTP behavior, not deployment or load capacity.

## API contract

| Method and path | Result |
|---|---|
| `GET /health` | 200, process health response |
| `GET /tasks` | 200, array of tasks in the current process |
| `POST /tasks` | 201 for a nonblank string title; 400 for invalid/missing input |
| Other routes | 404, JSON error |

Create requests should be JSON objects. Invalid JSON, missing titles, non-string titles and
whitespace-only titles are rejected. The response has a JSON content type.

## How it works

`HTTP request → route/body validation → in-memory Map → JSON response`.

[server.js](server.js) exposes `createApp()` for an isolated server and `createTaskStore()` for
storage. Starting the file directly listens on `PORT` or 3000. Each app has its own store.
[server.test.js](server.test.js) exercises health, empty listing, creation, retrieval through the
list, distinct IDs, invalid titles/JSON and missing routes.

## Scope and limits

- Built: health, list and create routes; local behavior tests; GitHub Actions workflows.
- Absent: update/delete endpoints, completion changes, persistence, authentication and pagination.
- `MAX_TITLE_LENGTH` is exported but not enforced by the creation route.
- Request bodies are accumulated without an explicit size limit. This is a learning service;
  the README does not establish readiness for untrusted production traffic.
- The listener does not restrict its bind address to localhost. Choose deployment exposure
  deliberately; there is no per-user authorization or database isolation.

## Repository and CI

| Path | Purpose |
|---|---|
| `server.js` | Server, routing and task store |
| `server.test.js` | Ten HTTP behavior tests |
| `package.json` | Runtime requirement and start/test commands |
| `.github/workflows/ci.yml` | Tests on pushes and pull requests |
| `.github/workflows/agent-pr.yml` | Agent-branch pull request workflow |
| `AGENTS.md` | Contribution and branch rules |

See [AGENTS.md](AGENTS.md) before contributing. The project has no install step for runtime
packages, and no published uptime or performance measurements.
