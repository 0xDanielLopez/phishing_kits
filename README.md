# phishing_kits

Archive of phishing kits collected from suspicious domains detected by
[phishunt.io](https://phishunt.io/). Over 1,000 archives, 2020 to date. The
exact count is the number of rows in `index.csv`.

Layout: `YYYY/YYYYMM/<host> (<filename>.ext)/<filename>.ext`.

## Read this before cloning

These are live phishing kits, stored exactly as they were captured: **not
encrypted, not password-protected, not defanged**. That has consequences:

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
| `url_provenance` | how `source_url` was obtained: `recorded` if it is the URL the download actually came from, `inferred` if it was reconstructed by the rule below, empty if unknown |

Nothing marked in `note` has been deleted, so existing references stay valid.

### Reconstructed source URLs

Most of the archive predates any automated capture, so `source_url` was empty
for 899 of the entries. 833 of those were reconstructed from the per-month
`urls.txt` files and are marked `inferred`.

The rule: same month, the URL's host equals the host in the directory name
(tolerating a leading `www.`), and the URL's last path segment equals the
archive's filename, both percent-decoded. A row is only filled when exactly one
line in that month's `urls.txt` satisfies all three.

It was validated before being applied. Run against the entries that already had
a recorded URL, it reproduces that URL exactly in 190 of 190 cases, and on the
4 it cannot resolve it declines rather than guessing.

The 66 entries still empty are 42 where more than one line matched and 24 in a
month with no `urls.txt`. They were left empty on purpose. The evidence is
published, so the join can be re-derived, extended or disputed.

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
an index: a line carries no reference to an archive, and matching one to the
other takes the reconstruction described above. Use `index.csv` instead, which
already holds the result.

## Rights

The archives are third-party code, written by the phishing operators. No licence
is granted or implied over them and all rights remain with their respective
holders. What this repository offers is the collection, the layout and the
metadata in `index.csv`. Research and OSINT use only.

## Removal requests

If you are the owner of data inside a kit, a brand being impersonated, or the
owner of a compromised host, see [SECURITY.md](SECURITY.md). Report it privately
rather than opening a public issue, which would republish the exact path you are
asking to have removed.
