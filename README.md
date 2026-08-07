# phishing_kits

Archive of phishing kits collected from suspicious domains detected by
[phishunt.io](https://phishunt.io/). 1,092 archives, 2020 to date.

Layout: `YYYY/YYYYMM/<host> (<filename>.ext)/<filename>.ext`.

## Read this before cloning

These are live phishing kits, stored exactly as they were captured and **not
password-protected**, unlike theZoo, MalwareBazaar or vx-underground. That has
consequences:

- Antivirus will quarantine files mid-checkout, and may flag the clone as a whole.
- Do not clone into a synced folder (Dropbox, OneDrive, Google Drive) or onto a
  managed corporate machine.
- The kits contain working credential-harvesting code. Do not deploy them.
- Some contain residual data from real victims, harvested by the operator before
  the kit was left behind on the server. Do not republish it, and do not submit
  samples to sandboxes or scanners in public mode.

Analyse in an isolated environment.

## index.csv

A manifest of every archive in the repository.

| column | meaning |
| --- | --- |
| `path` | repo-relative path of the archive |
| `size` | size in bytes |
| `git_sha1` | git object id, so `git hash-object <file>` verifies a local copy against this file alone |
| `sha256` | file hash, which is what VirusTotal, MalwareBazaar and MISP index by |
| `source_url` | URL the archive was downloaded from, where it is known |
| `note` | set on 34 entries that are not usable kits: `empty` (0 bytes), `not-an-archive` (no archive signature, mostly saved error pages) and `no-files` (a valid zip containing no files) |

Nothing marked in `note` has been deleted, so existing references stay valid.

Look up a hash without cloning anything:

```
curl -s https://raw.githubusercontent.com/0xDanielLopez/phishing_kits/master/index.csv | grep <sha256>
```

## Metadata without cloning

The repository is around 3 GB and almost all of it is content rather than
history, so `--depth 1` saves nothing. What does help:

```
git clone --filter=blob:none --sparse https://github.com/0xDanielLopez/phishing_kits.git
```

The per-month `urls.txt` files list the source URLs seen that month. They are not
an index and their lines cannot be mapped to individual archives. Use `index.csv`
for that.

## Rights

The archives are third-party code, written by the phishing operators. No licence
is granted or implied over them and all rights remain with their respective
holders. What this repository offers is the collection, the layout and the
metadata in `index.csv`. Research and OSINT use only.

## Removal requests

If you are the owner of data inside a kit, a brand being impersonated, or the
owner of a compromised host, report it privately through
[GitHub private reporting](https://github.com/0xDanielLopez/phishing_kits/security/advisories/new)
or at info@phishunt.io, rather than opening a public issue. Include the exact
path, the reason and your relation to it.

Substantiated requests are removed from the tip of `master`. Git history and the
existing forks are outside my control, so removal cannot be guaranteed to be
complete.
