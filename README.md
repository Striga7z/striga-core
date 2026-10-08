# striga-versions

Public version feed for the Striga FiveM suite. Not shipped to customers.

striga-core (server, `server/versions.lua`) downloads
`https://raw.githubusercontent.com/STRIGA_GITHUB_USER/striga-versions/main/versions.json`
about 10 s after start and then every 6 h (config.json `"versionCheck": false` turns it off).
For striga-core and every installed Striga resource (fxmanifest has `striga_core_min`) it compares the
installed `version` with the one here and prints `UPDATE: <resource> <installed> -> <latest>. Download: <url>`.

## versions.json

```json
{ "resources": { "<resource name>": { "version": "1.2.3", "url": "https://...", "notes": "short changelog" } } }
```

- `version`: `major.minor.patch` (numbers only). Must match the fxmanifest `version` of the release.
- `url`: `https://` download page (Keymaster / Tebex / GitHub release). Optional.
- `notes`: one short line (max 200 characters), printed under the UPDATE line. Optional.
- A resource missing here is simply not checked.

## Releasing an update

1. Bump `version` in the resource's fxmanifest (and `striga_core_min` if it now needs a newer striga-core).
2. Upload the release (Keymaster) and note the download URL.
3. Edit `versions.json`: new `version`, `url`, `notes`. Add a line to `CHANGELOG.md`.
4. Commit + push to `main`. Servers see it on their next check (raw.githubusercontent.com caches ~5 min).

Keep the JSON valid: a broken file only produces one warning on every server, but no update notices.
