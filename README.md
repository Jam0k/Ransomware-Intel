# Ransomware-Intel

Free, no-auth ransomware leak-site intelligence from [ThreatCluster](https://threatcluster.io/dark-web):
victims, groups, and the onion infrastructure behind them — snapshotted here daily so the git
history is a timestamped record.

**TLP:CLEAR** — free to use, redistribute, and integrate. Attribution appreciated.

<!--STATS-->
_Last updated: 2026-09-23 06:56 UTC_

| | |
|---|---|
| Ransomware groups tracked | **161** |
| Victims, last 90 days | **2,913** |
| Victims, last 365 days | **9,395** |
| Victims read first-hand from leak sites | **1,185** |
| Onion addresses catalogued | **649** (44 confirmed up at last probe) |
| Known data breaches | **0** |
<!--/STATS-->

---

## Why this repo exists

Most public ransomware-victim datasets on GitHub stopped updating years ago, and the ones still
alive republish the same aggregator rows as each other. This one is different in one specific way:
**a growing share of it is read directly off the leak sites by our own crawler**, not imported
from an aggregator. Those rows are marked `first_party=yes` and have no equivalent elsewhere.

What this repo holds is the **listing**: who was named, by which group, when, and whether the post
is still up. What our crawler extracts from each post (claimed data size, file counts, publication
and negotiation state, the ransom figure, the victim's legal name and headquarters, our own summary
and screenshot) is the enriched record, and that lives in the
[API](https://threatcluster.io/api) rather than in these files. See
[Enriched records](#enriched-records) below.

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
| [`data/victims.json`](data/victims.json) | Last 30 days as JSON, same fields as the CSV (plus `website` where known) |
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

# Same, JSON, 30 days
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
| `first_party` | `yes` if the row was read off the leak site by ThreatCluster; `no` if imported from an aggregator |

`victims.json` carries the same columns, plus `website` where the leak page states it.

### Enriched records

Everything our crawler extracts from the post itself is in the API, one call per victim:

```bash
curl -H "X-API-Key: $TC_KEY" \
  "https://threatcluster.io/api/public/v1/darkweb/ransomware/victim/{id}"
```

That record carries the claimed data size and file count, publication and negotiation state
(`listed` · `countdown` · `published` · `negotiating` · `sold`, normalised from the site's own
wording), the ransom figure, the `.onion` page it was read from, the victim's legal name,
headquarters, revenue and employee count, extracted data categories, our screenshot, and a short
summary generated only from what we collected. It costs 10 credits; a free key is minted on every
account with 100 credits a day, no card. `/api/public/v1/darkweb/ransomware/victims` lists victims
with the same filters as the CSV. Quick start: https://threatcluster.io/api

**`status=delisted`** means the post is no longer reachable on the leak site. It is an
observation, not a claim that a ransom was paid — groups remove posts for their own reasons.

**`first_party=yes`** is the point of this dataset. Our crawler fetched the group's index, then
each victim's own page, and extracted the fields from what the site actually says. Where a
first-party value and an aggregator value both exist, the first-party one wins.

---

## Groups

Every active group, linked to its ThreatCluster page (leak-site status, mirrors,
claimed victims, our own screenshot). Regenerated daily.

<!--GROUPS-->
_161 active groups; top 150 by claimed victims._

| Group | Claimed victims | Read first-hand | Last seen |
|---|---:|---:|---|
| [qilin](https://threatcluster.io/dark-web/group/qilin) | 2,307 | 94 | 2026-09-22 |
| [lockbit3](https://threatcluster.io/dark-web/group/lockbit3) | 2,016 | 71 | 2025-12-05 |
| [akira](https://threatcluster.io/dark-web/group/akira) | 1,616 | 0 | 2026-09-22 |
| [clop](https://threatcluster.io/dark-web/group/clop) | 1,302 | 26 | 2026-09-10 |
| [incransom](https://threatcluster.io/dark-web/group/incransom) | 943 | 30 | 2026-09-21 |
| [thegentlemen](https://threatcluster.io/dark-web/group/thegentlemen) | 868 | 0 | 2026-09-21 |
| [dragonforce](https://threatcluster.io/dark-web/group/dragonforce) | 652 | 35 | 2026-09-16 |
| [safepay](https://threatcluster.io/dark-web/group/safepay) | 569 | 51 | 2026-09-15 |
| [lynx](https://threatcluster.io/dark-web/group/lynx) | 417 | 0 | 2026-08-27 |
| [everest](https://threatcluster.io/dark-web/group/everest) | 398 | 213 | 2026-09-07 |
| [lockbit5](https://threatcluster.io/dark-web/group/lockbit5) | 365 | 139 | 2026-09-20 |
| [nightspire](https://threatcluster.io/dark-web/group/nightspire) | 329 | 21 | 2026-09-21 |
| [rhysida](https://threatcluster.io/dark-web/group/rhysida) | 288 | 0 | 2026-09-19 |
| [killsec](https://threatcluster.io/dark-web/group/killsec) | 286 | 236 | 2026-09-18 |
| [ransomhouse](https://threatcluster.io/dark-web/group/ransomhouse) | 209 | 0 | 2026-09-12 |
| [nova](https://threatcluster.io/dark-web/group/nova) | 178 | 0 | 2026-07-25 |
| [handala](https://threatcluster.io/dark-web/group/handala) | 175 | 0 | 2026-04-07 |
| [funksec](https://threatcluster.io/dark-web/group/funksec) | 172 | 0 | 2025-03-18 |
| [cloak](https://threatcluster.io/dark-web/group/cloak) | 166 | 0 | 2026-06-18 |
| [shinyhunters](https://threatcluster.io/dark-web/group/shinyhunters) | 160 | 0 | 2026-09-22 |
| [apt73](https://threatcluster.io/dark-web/group/apt73) | 157 | 219 | 2026-07-24 |
| [silentransomgroup](https://threatcluster.io/dark-web/group/silentransomgroup) | 152 | 0 | 2026-08-27 |
| [krybit](https://threatcluster.io/dark-web/group/krybit) | 150 | 179 | 2026-09-20 |
| [sarcoma](https://threatcluster.io/dark-web/group/sarcoma) | 141 | 22 | 2026-03-30 |
| [ragnarlocker](https://threatcluster.io/dark-web/group/ragnarlocker) | 128 | 0 | 2023-10-11 |
| [interlock](https://threatcluster.io/dark-web/group/interlock) | 126 | 43 | 2026-09-15 |
| [pear](https://threatcluster.io/dark-web/group/pear) | 119 | 31 | 2026-09-09 |
| [toufan](https://threatcluster.io/dark-web/group/toufan) | 117 | 0 | 2023-12-27 |
| [arcusmedia](https://threatcluster.io/dark-web/group/arcusmedia) | 113 | 70 | 2026-09-19 |
| [anubis](https://threatcluster.io/dark-web/group/anubis) | 112 | 91 | 2026-09-22 |
| [eldorado](https://threatcluster.io/dark-web/group/eldorado) | 112 | 0 | 2025-01-22 |
| [payoutsking](https://threatcluster.io/dark-web/group/payoutsking) | 110 | 0 | 2026-08-24 |
| [deadlock](https://threatcluster.io/dark-web/group/deadlock) | 101 | 19 |  |
| [kairos](https://threatcluster.io/dark-web/group/kairos) | 97 | 12 | 2026-09-22 |
| [medusalocker](https://threatcluster.io/dark-web/group/medusalocker) | 94 | 0 | 2026-09-12 |
| [chaos](https://threatcluster.io/dark-web/group/chaos) | 92 | 107 | 2026-09-17 |
| [threeam](https://threatcluster.io/dark-web/group/threeam) | 92 | 0 | 2026-09-21 |
| [abyss](https://threatcluster.io/dark-web/group/abyss) | 91 | 53 | 2026-08-26 |
| [ransomexx](https://threatcluster.io/dark-web/group/ransomexx) | 86 | 11 | 2026-06-20 |
| [braincipher](https://threatcluster.io/dark-web/group/braincipher) | 80 | 51 | 2026-07-22 |
| [settra](https://threatcluster.io/dark-web/group/settra) | 76 | 0 | 2026-09-11 |
| [payload](https://threatcluster.io/dark-web/group/payload) | 74 | 72 | 2026-08-20 |
| [beast](https://threatcluster.io/dark-web/group/beast) | 73 | 45 | 2026-08-30 |
| [storm](https://threatcluster.io/dark-web/group/storm) | 64 | 0 |  |
| [gunra](https://threatcluster.io/dark-web/group/gunra) | 54 | 0 | 2026-09-04 |
| [metaencryptor](https://threatcluster.io/dark-web/group/metaencryptor) | 53 | 77 | 2026-09-21 |
| [insomnia](https://threatcluster.io/dark-web/group/insomnia) | 52 | 47 | 2026-09-15 |
| [termite](https://threatcluster.io/dark-web/group/termite) | 52 | 45 | 2026-09-22 |
| [ailock](https://threatcluster.io/dark-web/group/ailock) | 51 | 60 | 2026-08-26 |
| [auditteam](https://threatcluster.io/dark-web/group/auditteam) | 48 | 31 | 2026-08-27 |
| [global secret group](https://threatcluster.io/dark-web/group/global%20secret%20group) | 48 | 49 |  |
| [blacknevas](https://threatcluster.io/dark-web/group/blacknevas) | 47 | 1 | 2026-09-16 |
| [crypto24](https://threatcluster.io/dark-web/group/crypto24) | 47 | 7 | 2026-08-31 |
| [orova](https://threatcluster.io/dark-web/group/orova) | 47 | 0 |  |
| [securotrop](https://threatcluster.io/dark-web/group/securotrop) | 44 | 39 | 2026-09-18 |
| [cmdorganization](https://threatcluster.io/dark-web/group/cmdorganization) | 43 | 3 | 2026-07-31 |
| [embargo](https://threatcluster.io/dark-web/group/embargo) | 41 | 15 | 2026-09-09 |
| [j](https://threatcluster.io/dark-web/group/j) | 41 | 0 | 2025-11-01 |
| [aurora](https://threatcluster.io/dark-web/group/aurora) | 38 | 37 | 2026-09-07 |
| [crpxo](https://threatcluster.io/dark-web/group/crpxo) | 37 | 0 |  |
| [m3rx](https://threatcluster.io/dark-web/group/m3rx) | 37 | 35 | 2026-08-14 |
| [moneymessage](https://threatcluster.io/dark-web/group/moneymessage) | 36 | 0 | 2026-09-21 |
| [lamashtu](https://threatcluster.io/dark-web/group/lamashtu) | 34 | 0 | 2026-06-17 |
| [dan0n](https://threatcluster.io/dark-web/group/dan0n) | 33 | 0 | 2024-08-23 |
| [panzer](https://threatcluster.io/dark-web/group/panzer) | 32 | 0 |  |
| [alphalocker](https://threatcluster.io/dark-web/group/alphalocker) | 31 | 2 | 2026-02-28 |
| [dark project](https://threatcluster.io/dark-web/group/dark%20project) | 30 | 23 |  |
| [bravox](https://threatcluster.io/dark-web/group/bravox) | 29 | 0 | 2026-09-20 |
| [l group](https://threatcluster.io/dark-web/group/l%20group) | 28 | 0 |  |
| [fulcrumsec](https://threatcluster.io/dark-web/group/fulcrumsec) | 27 | 28 | 2026-09-11 |
| [titan](https://threatcluster.io/dark-web/group/titan) | 26 | 26 | 2026-09-22 |
| [underground](https://threatcluster.io/dark-web/group/underground) | 26 | 13 | 2025-08-15 |
| [unsafe](https://threatcluster.io/dark-web/group/unsafe) | 26 | 13 | 2026-09-21 |
| [werewolves](https://threatcluster.io/dark-web/group/werewolves) | 26 | 0 | 2024-03-04 |
| [lapsus$](https://threatcluster.io/dark-web/group/lapsus%24) | 25 | 0 | 2026-06-23 |
| [emperador](https://threatcluster.io/dark-web/group/emperador) | 24 | 30 |  |
| [radar](https://threatcluster.io/dark-web/group/radar) | 24 | 0 | 2026-04-29 |
| [morpheus](https://threatcluster.io/dark-web/group/morpheus) | 23 | 0 | 2026-07-30 |
| [majinahanashi](https://threatcluster.io/dark-web/group/majinahanashi) | 22 | 0 |  |
| [zawoo](https://threatcluster.io/dark-web/group/zawoo) | 22 | 0 |  |
| [daixin](https://threatcluster.io/dark-web/group/daixin) | 21 | 46 | 2025-09-11 |
| [shadowbyt3$](https://threatcluster.io/dark-web/group/shadowbyt3%24) | 21 | 0 | 2026-08-29 |
| [booba project](https://threatcluster.io/dark-web/group/booba%20project) | 20 | 15 |  |
| [kazu](https://threatcluster.io/dark-web/group/kazu) | 20 | 0 | 2026-09-07 |
| [ralord](https://threatcluster.io/dark-web/group/ralord) | 19 | 0 | 2025-04-27 |
| [alp-001](https://threatcluster.io/dark-web/group/alp-001) | 17 | 0 | 2026-04-08 |
| [tridentlocker](https://threatcluster.io/dark-web/group/tridentlocker) | 17 | 17 | 2026-09-04 |
| [wallstreet](https://threatcluster.io/dark-web/group/wallstreet) | 17 | 1 |  |
| [exfilsquad](https://threatcluster.io/dark-web/group/exfilsquad) | 15 | 18 |  |
| [vexy ransomware](https://threatcluster.io/dark-web/group/vexy%20ransomware) | 15 | 15 |  |
| [orion](https://threatcluster.io/dark-web/group/orion) | 14 | 21 | 2026-07-27 |
| [n0n](https://threatcluster.io/dark-web/group/n0n) | 13 | 21 |  |
| [blackout](https://threatcluster.io/dark-web/group/blackout) | 12 | 0 | 2026-07-19 |
| [doommageddon](https://threatcluster.io/dark-web/group/doommageddon) | 12 | 13 |  |
| [icarus](https://threatcluster.io/dark-web/group/icarus) | 12 | 0 | 2026-06-23 |
| [imncrew](https://threatcluster.io/dark-web/group/imncrew) | 12 | 0 | 2025-09-16 |
| [blackwater](https://threatcluster.io/dark-web/group/blackwater) | 11 | 22 | 2026-08-24 |
| [black x](https://threatcluster.io/dark-web/group/black%20x) | 11 | 0 |  |
| [cryp70n1c0d3](https://threatcluster.io/dark-web/group/cryp70n1c0d3) | 11 | 0 | 2021-12-18 |
| [dysphor1a](https://threatcluster.io/dark-web/group/dysphor1a) | 11 | 0 |  |
| [eclipse](https://threatcluster.io/dark-web/group/eclipse) | 10 | 1 |  |
| [barracuda](https://threatcluster.io/dark-web/group/barracuda) | 9 | 0 |  |
| [leakbazaar](https://threatcluster.io/dark-web/group/leakbazaar) | 9 | 0 | 2026-05-09 |
| [helix](https://threatcluster.io/dark-web/group/helix) | 8 | 8 |  |
| [iah6477](https://threatcluster.io/dark-web/group/iah6477) | 8 | 8 | 2026-09-15 |
| [vanhelsing](https://threatcluster.io/dark-web/group/vanhelsing) | 8 | 0 | 2025-04-05 |
| [0mega](https://threatcluster.io/dark-web/group/0mega) | 7 | 0 | 2024-01-25 |
| [karma](https://threatcluster.io/dark-web/group/karma) | 7 | 0 | 2021-10-04 |
| [malekteam](https://threatcluster.io/dark-web/group/malekteam) | 7 | 0 | 2024-04-05 |
| [secp0](https://threatcluster.io/dark-web/group/secp0) | 7 | 7 | 2026-09-22 |
| [netrunner](https://threatcluster.io/dark-web/group/netrunner) | 6 | 8 | 2026-04-03 |
| [runsomewares](https://threatcluster.io/dark-web/group/runsomewares) | 6 | 9 | 2025-07-10 |
| [0day syndicate](https://threatcluster.io/dark-web/group/0day%20syndicate) | 5 | 0 |  |
| [atomsilo](https://threatcluster.io/dark-web/group/atomsilo) | 5 | 0 | 2026-02-24 |
| [gammax](https://threatcluster.io/dark-web/group/gammax) | 5 | 6 |  |
| [gdlockersec](https://threatcluster.io/dark-web/group/gdlockersec) | 5 | 0 | 2025-01-26 |
| [prinzeugen](https://threatcluster.io/dark-web/group/prinzeugen) | 5 | 1 | 2026-06-04 |
| [ulose](https://threatcluster.io/dark-web/group/ulose) | 5 | 0 |  |
| [valencialeaks](https://threatcluster.io/dark-web/group/valencialeaks) | 5 | 0 | 2024-09-18 |
| [exitium](https://threatcluster.io/dark-web/group/exitium) | 4 | 6 | 2026-04-14 |
| [linkc](https://threatcluster.io/dark-web/group/linkc) | 4 | 0 | 2026-02-27 |
| [satanlockv2](https://threatcluster.io/dark-web/group/satanlockv2) | 4 | 0 | 2025-07-07 |
| [triple x](https://threatcluster.io/dark-web/group/triple%20x) | 4 | 0 |  |
| [cry0](https://threatcluster.io/dark-web/group/cry0) | 3 | 0 | 2026-09-19 |
| [d1r](https://threatcluster.io/dark-web/group/d1r) | 3 | 0 |  |
| [endzone](https://threatcluster.io/dark-web/group/endzone) | 3 | 2 |  |
| [ethics](https://threatcluster.io/dark-web/group/ethics) | 3 | 0 |  |
| [falcon](https://threatcluster.io/dark-web/group/falcon) | 3 | 2 |  |
| [timc](https://threatcluster.io/dark-web/group/timc) | 3 | 0 | 2026-04-09 |
| [blackfield](https://threatcluster.io/dark-web/group/blackfield) | 2 | 0 |  |
| [bluewhale](https://threatcluster.io/dark-web/group/bluewhale) | 2 | 0 |  |
| [redact](https://threatcluster.io/dark-web/group/redact) | 2 | 1 |  |
| [sensayq](https://threatcluster.io/dark-web/group/sensayq) | 2 | 0 | 2024-06-04 |
| [spirals](https://threatcluster.io/dark-web/group/spirals) | 2 | 3 |  |
| [the green blood group](https://threatcluster.io/dark-web/group/the%20green%20blood%20group) | 2 | 0 |  |
| [blacklocks](https://threatcluster.io/dark-web/group/blacklocks) | 1 | 0 |  |
| [loki](https://threatcluster.io/dark-web/group/loki) | 1 | 0 | 2026-03-12 |
| [notpetya](https://threatcluster.io/dark-web/group/notpetya) | 1 | 0 |  |
| [robinhood](https://threatcluster.io/dark-web/group/robinhood) | 1 | 0 | 2021-12-06 |
| [sovcali](https://threatcluster.io/dark-web/group/sovcali) | 1 | 0 |  |
| [abrahams_ax](https://threatcluster.io/dark-web/group/abrahams_ax) | 0 | 0 |  |
| [agl0bgvycg](https://threatcluster.io/dark-web/group/agl0bgvycg) | 0 | 0 |  |
| [aptlock](https://threatcluster.io/dark-web/group/aptlock) | 0 | 0 |  |
| [bolt team](https://threatcluster.io/dark-web/group/bolt%20team) | 0 | 0 |  |
| [contfr](https://threatcluster.io/dark-web/group/contfr) | 0 | 0 | 2026-04-22 |
| [cyberleek](https://threatcluster.io/dark-web/group/cyberleek) | 0 | 0 |  |
| [darkmatter](https://threatcluster.io/dark-web/group/darkmatter) | 0 | 0 |  |
| [dread](https://threatcluster.io/dark-web/group/dread) | 0 | 0 | 2026-04-22 |
| [galago](https://threatcluster.io/dark-web/group/galago) | 0 | 0 |  |
| [goddamn ransomwhere](https://threatcluster.io/dark-web/group/goddamn%20ransomwhere) | 0 | 0 |  |
<!--/GROUPS-->


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
- **Claimed sizes are verbatim** in the API record. `0.00 GB` alongside a large file count is
  the group's page being wrong, faithfully recorded; we don't correct it.
- **Screenshots are ours** — captured by our crawler with faces blurred. No third-party images
  are redistributed.

---

## Reporting

Listed in error, or an organisation that wants its entry reviewed:
[hello@threatcluster.io](mailto:hello@threatcluster.io).

## Links

- Browse: [threatcluster.io/dark-web](https://threatcluster.io/dark-web)
- IOC feeds (domains, IPs, hashes, wallets): [Public-Feeds-IOCs](https://github.com/Jam0k/Public-Feeds-IOCs)
- API quick start: [threatcluster.io/api](https://threatcluster.io/api) · client and examples: [Cyber-Threat-Intelligence-API](https://github.com/Jam0k/Cyber-Threat-Intelligence-API)
