# Ransomware-Intel

Free, no-auth ransomware leak-site intelligence from [ThreatCluster](https://threatcluster.io/dark-web):
victims, groups, and the onion infrastructure behind them — snapshotted here daily so the git
history is a timestamped record.

**TLP:CLEAR** — free to use, redistribute, and integrate. Attribution appreciated.

<!--STATS-->
_Last updated: 2026-08-30 14:25 UTC_

| | |
|---|---|
| Ransomware groups tracked | **158** |
| Victims, last 90 days | **2,852** |
| Victims, last 365 days | **9,205** |
| Victims read first-hand from leak sites | **697** |
| Onion addresses catalogued | **1,682** (78 confirmed up at last probe) |
| Known data breaches | **1,032** |
<!--/STATS-->

---

## Why this repo exists

Most public ransomware-victim datasets on GitHub stopped updating years ago, and the ones still
alive republish the same aggregator rows as each other. This one is different in one specific way:
**a growing share of it is read directly off the leak sites by our own crawler** — claimed data
size, file counts, publication status, the ransom note, and the exact `.onion` URL the claim came
from. Those rows are marked `first_party=yes` and have no equivalent elsewhere.

Everything is **the group's claim, not a confirmed breach.** Treat it as such.

---

## Files in this repo

Refreshed daily from the API. Use these for a pinned, versioned copy — or `git log -p` to see when
a victim first appeared, when a post was delisted, or when a leak site went dark.

| File | Contents |
|---|---|
| [`data/victims.csv`](data/victims.csv) | Victims listed in the last **90 days** |
| [`data/victims-365d.csv`](data/victims-365d.csv) | Victims listed in the last **365 days** |
| [`data/victims-firstparty.csv`](data/victims-firstparty.csv) | **Only the rows we read off the leak site ourselves** (365 days) |
| [`data/victims.json`](data/victims.json) | Last 30 days with the full enrichment fields (legal name, HQ, revenue, ransom amount, data categories, analyst summary) |
| [`data/groups.csv`](data/groups.csv) | Every extortion group tracked: activity window, claimed-victim count, how many we verified first-hand, mirrors currently up |
| [`data/onions.csv`](data/onions.csv) | Leak-site and underground `.onion` addresses, with our own up/down probe result |

```bash
# pin to this repo
curl -s https://raw.githubusercontent.com/Jam0k/Ransomware-Intel/main/data/victims.csv
```

The live API below is always current; these files lag by up to a day.

---

## Quick start

```bash
# Victims, last 90 days (CSV)
curl https://threatcluster.io/api/darkweb/public/victims.csv

# Same, full year
curl "https://threatcluster.io/api/darkweb/public/victims.csv?days=365"

# Victims with enrichment (JSON, 30 days)
curl https://threatcluster.io/api/darkweb/public/victims.json

# Groups and onion infrastructure
curl https://threatcluster.io/api/darkweb/public/groups.csv
curl https://threatcluster.io/api/darkweb/public/onions.csv

# Headline counts
curl https://threatcluster.io/api/darkweb/stats
```

All endpoints are unauthenticated and rate-limited to 60 requests/minute.

---

## Victims

`victims.csv` — `?days=` caps the window (default 90, max 365).

| Column | Meaning |
|---|---|
| `group` | Extortion group that listed the victim |
| `victim` | Organisation name as the group wrote it |
| `country`, `sector` | Where known; blank means unknown, never guessed |
| `discovered` | Date the listing was first seen |
| `post_url` | The leak-site post |
| `status` | `listed` or `delisted` — see below |
| `delisted_date` | When the post stopped being reachable |
| `data_size` | Volume the group **claims** to hold, verbatim from the page (`4.6TB`, `151.0 MB`) |
| `file_count` | File count the group claims |
| `publication_status` | `listed` · `countdown` · `published` · `negotiating` · `sold` — normalised from the site's own wording |
| `leak_url` | The `.onion` page our crawler read this from |
| `first_party` | `yes` if the row was read off the leak site by ThreatCluster; `no` if imported from an aggregator |

`victims.json` adds, where the leak page states them: `website`, `legal_name`, `headquarters`,
`revenue`, `employee_count`, `ransom_amount`, `data_categories`, and `summary` — a short analyst
summary generated only from what we collected, never from third-party blurbs.

**`status=delisted`** means the post is no longer reachable on the leak site. It is an
observation, not a claim that a ransom was paid — groups remove posts for their own reasons.

**`first_party=yes`** is the point of this dataset. Our crawler fetched the group's index, then
each victim's own page, and extracted the fields from what the site actually says. Where a
first-party value and an aggregator value both exist, the first-party one wins.

---

## Groups

`groups.csv`

| Column | Meaning |
|---|---|
| `group` | Canonical lowercase name |
| `active` | Whether our crawler could reach a leak site recently — not whether the group has disbanded |
| `first_seen`, `last_seen` | Activity window |
| `victim_count` | Total claimed victims (aggregated) |
| `first_party_victims` | Victims we read off this group's own site |
| `mirrors_up` | Onion addresses that answered on the last probe |
| `screenshot_url` | Our own capture of the leak site front page, face-blurred |

Every group has a page at `https://threatcluster.io/dark-web/group/{group}` and every victim at
`https://threatcluster.io/dark-web/victim/{slug}`.

---

## Onion infrastructure

`onions.csv`

| Column | Meaning |
|---|---|
| `onion_url` | The address |
| `kind` | `leak_site` (actively monitored) or an archived category (`market`, `forum`, …) |
| `operator` | Group / site name where known |
| `reachability` | `up` / `down` from **our own probe**; blank means archived and no longer probed |
| `last_checked` | When we last probed it |

This is a directory of criminal infrastructure published for research, blocking and
monitoring — the same basis on which other public trackers publish theirs. Addresses are ones
the operators advertise themselves; nothing here is a credential or an access route.

---

## Caveats

- **Claims, not confirmations.** A listing means a group *said* it breached an organisation.
- **Names are as the group wrote them**, including typos and stylisation. Match on `website`
  where present rather than on `victim`.
- **Sizes are verbatim.** `0.00 GB` alongside a large `file_count` is the group's page being
  wrong, faithfully recorded; we don't correct it.
- **Screenshots are ours** — captured by our crawler with faces blurred. No third-party images
  are redistributed.

---

## Reporting

Listed in error, or an organisation that wants its entry reviewed:
[hello@threatcluster.io](mailto:hello@threatcluster.io).

## Links

- Browse: [threatcluster.io/dark-web](https://threatcluster.io/dark-web)
- IOC feeds (domains, IPs, hashes, wallets): [Public-Feeds-IOCs](https://github.com/Jam0k/Public-Feeds-IOCs)
- Full API: [threatcluster.io/developers](https://threatcluster.io/developers)
