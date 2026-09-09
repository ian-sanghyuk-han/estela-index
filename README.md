# estela-index

Search index for [Estela](https://ian-sanghyuk-han.github.io/estela/). Derived from
[estela-data](https://github.com/ian-sanghyuk-han/estela-data); no original data lives here.

**It is a dictionary, not a ranking.** A prefix cell holds every place under it as long as
there are 50 or fewer. Above that the cell holds only a count, and the names move one
character deeper — so `tr` says "153,112 below", `tra` narrows, `tratt` narrows again.
Nothing is promoted or demoted to make a particular restaurant reachable from a short prefix.

- `index/b0000.json` … `b4095.json` — bucket = `crc32(prefix) % 4096`, ~75 KB each.
- Entry `{"prefix": {"n": total, "p": [[name, lat, lon, sourceIndex], …]}}`.
  An entry with `n` but no `p` means: keep typing.
- Prefixes run 2–8 characters, taken from every word of a name, lowercased.
- A cell that still overflows at 8 characters keeps 50, ordered the way a dictionary
  orders its own page: the exact word you typed before longer words that merely start
  with it, then alphabetically, spread across areas so one city cannot take every slot.
- `sourceIndex` points into `srcs` in estela-data's `registry/manifest.json`.

Built by `tools/build_index.py` in the app repo. Licences follow the upstream sources
(Overture Maps CDLA-Permissive 2.0; government registries under their own open terms) —
see `registry/manifest.json` in estela-data for the per-source terms.
