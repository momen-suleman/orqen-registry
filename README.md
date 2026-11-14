# orqen-registry

Community capability index for [Orqen Mission Studio](https://github.com/momen-suleman/Orqen).

`index.json` is the index Orqen's Discover page reads. Submitting a package
is a normal pull request to this file — review of PRs here is the only
moderation this registry has, by design.

## Adding your package

Open a PR that appends an entry to the JSON array in `index.json`:

```json
{
  "name": "Your Tool Name",
  "repo": "yourname/your-tool-repo",
  "description": "What it does, in one or two sentences.",
  "category": "Analyze",
  "publisher": "yourname",
  "runCommand": "docker run -p 9321:9321 ghcr.io/yourname/your-tool",
  "tags": ["example-tag"]
}
```

Field notes:

- `name` and `repo` are required. Everything else is optional and defaults
  to empty/`"External"`.
- `repo` must be `owner/repo` for **your tool's own source repository** —
  Orqen pulls that repo's real, live GitHub star count to rank it. It does
  not have to be this registry repo.
- `category` should be one of `Transform`, `Analyze`, `Validate`, `AI`,
  `Security`, `External` to group cleanly in Orqen's UI. Anything else still
  works, it just gets its own filter group.
- `runCommand` is shown as a copy-to-clipboard suggestion when someone clicks
  **Install** — Orqen never runs it for you. See
  [`docker/`](https://github.com/momen-suleman/Orqen/tree/main/docker) in the
  Orqen repo for a working template that implements the contract below.
- Your tool itself does **not** live in this repo. This repo only ever holds
  pointers (`index.json` entries) to tools that run as their own services
  elsewhere.

## The contract your tool's endpoint must speak

```
POST /   { "input": "<text>" }   ->   { "result": "<text>" }
GET  /   (any response = reachable, used by Orqen's "Test connection")
```

## Consuming this index

Point an Orqen install at this repo with:

```
ORQEN_REGISTRY_URL=https://raw.githubusercontent.com/momen-suleman/orqen-registry/main/index.json
ORQEN_REGISTRY_REPO=momen-suleman/orqen-registry
```
