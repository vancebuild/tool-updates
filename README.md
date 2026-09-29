# tool-updates

Update metadata for Redpack tools. Each tool has its own folder with a `version.json`. The tool
reads that file when it starts and shows an update banner if the version is newer than the one
running.

The repo is public so the check works without a sign-in. The tools and their releases can stay
private.

## Tools

| Folder                 | Tool                | Releases                                                  |
| ---------------------- | ------------------- | --------------------------------------------------------- |
| `redpack-ipc-toolkit/` | Redpack IPC Toolkit | https://github.com/vancebuild/redpack-ipc-toolkit/releases |

## version.json

```json
{"version": "1.3.0", "url": "https://github.com/vancebuild/redpack-ipc-toolkit/releases/tag/v1.3.0"}
```

| Field     | Required | Meaning                                                                                  |
| --------- | -------- | ---------------------------------------------------------------------------------------- |
| `version` | yes      | `MAJOR.MINOR.PATCH`, optional leading `v`. Suffixes such as `-beta` are not recognised, so no banner is shown. |
| `url`     | no       | Where the banner's button goes. If left out, the tool opens its releases page.           |

- The banner appears only when `version` is **higher** than the running version. Equal or lower
  shows nothing, so setting a lower version is harmless.
- Tools fetch the raw file:
  `https://raw.githubusercontent.com/vancebuild/tool-updates/main/<tool>/version.json`
- A timeout, a 404 or bad JSON counts as "no update". The tool never shows an error.
- If the release page is in a private repo, a colleague needs read access to that repo to
  download from it.

## Releasing a new version

Bump `version.json` **last**:

1. Push and tag the release in the tool's own repo.
2. Create the GitHub release there and attach the binaries.
3. Update `<tool>/version.json` here and commit to `main`.

If you bump it before the release exists, every running copy's banner points at a page that
isn't there yet.

`raw.githubusercontent.com` caches for about 5 minutes, so it can take that long for a change to
show up in the tools.

## Adding a tool

1. Create `<tool-name>/version.json` in the format above.
2. Point the tool's update check at that file's raw URL. Use a short timeout, compare the
   versions numerically, and treat any failure as "no update".
3. Add a row to the **Tools** table.

## Notes

- Redpack IPC Toolkit 1.3.0 and later read this repo. 1.2.0 reads an older Gist which points towards 1.3.0
