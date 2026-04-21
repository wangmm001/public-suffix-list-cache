# public-suffix-list-cache

Daily snapshots of the [Public Suffix List](https://publicsuffix.org/) (PSL) — the canonical Mozilla-maintained registry of domain-name suffixes (TLDs, ccTLDs, and publicly-accessible private suffixes like `github.io`, `co.uk`, `s3.amazonaws.com`) — mirrored by [`wangmm001/feedcache`](https://github.com/wangmm001/feedcache).

## Why mirror it

If you have a top-million list and want to reduce e.g. `blog.example.co.uk` to its **registrable** domain `example.co.uk`, you need the PSL to know `co.uk` is a public suffix. Every domain-parsing library (`publicsuffix2`, `tldextract`, `psl`) ships with a copy of this file. This repo gives you a point-in-time version-controlled history of it.

## Layout

```
data/
  YYYY-MM-DD.dat.gz     # daily snapshot, gzipped
  current.dat.gz        # pointer to the most recent snapshot
```

## Consume

```bash
# latest only
curl -L https://raw.githubusercontent.com/wangmm001/public-suffix-list-cache/main/data/current.dat.gz | zcat | head

# point-in-time
curl -L https://raw.githubusercontent.com/wangmm001/public-suffix-list-cache/main/data/2026-04-21.dat.gz | zcat | grep '^co\.'
```

## License

The Public Suffix List data is licensed under [Mozilla Public License 2.0](https://mozilla.org/MPL/2.0/). The README and cron scaffolding here are MIT.

## How it works

Daily GitHub Actions cron (04:30 UTC) calls `wangmm001/feedcache`'s reusable workflow, which downloads `https://publicsuffix.org/list/public_suffix_list.dat` and gzips it. No commit is produced when the content hasn't changed.
