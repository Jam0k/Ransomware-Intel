# Ransomware-Intel

Free, no-auth ransomware leak-site intelligence from [ThreatCluster](https://threatcluster.io/dark-web):
victims, groups, and the onion infrastructure behind them — snapshotted here daily so the git
history is a timestamped record.

**TLP:CLEAR** — free to use, redistribute, and integrate. Attribution appreciated.

<!--STATS-->
_Last updated: 2026-08-31 07:01 UTC_

| | |
|---|---|
| Ransomware groups tracked | **161** |
| Victims, last 90 days | **2,847** |
| Victims, last 365 days | **9,200** |
| Victims read first-hand from leak sites | **740** |
| Onion addresses catalogued | **1,690** (41 confirmed up at last probe) |
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

Every active group, linked to its ThreatCluster page (leak-site status, mirrors,
claimed victims, our own screenshot). Regenerated daily.

<!--GROUPS-->
_161 active groups; top 150 by claimed victims._

| Group | Claimed victims | Read first-hand | Last seen |
|---|---:|---:|---|
| [qilin](https://threatcluster.io/dark-web/group/qilin) | 2,247 | 31 | 2026-08-30 |
| [lockbit3](https://threatcluster.io/dark-web/group/lockbit3) | 2,016 | 68 | 2025-12-05 |
| [akira](https://threatcluster.io/dark-web/group/akira) | 1,584 | 0 | 2026-08-28 |
| [clop](https://threatcluster.io/dark-web/group/clop) | 1,300 | 0 | 2026-08-14 |
| [incransom](https://threatcluster.io/dark-web/group/incransom) | 919 | 0 | 2026-08-29 |
| [thegentlemen](https://threatcluster.io/dark-web/group/thegentlemen) | 806 | 0 | 2026-08-28 |
| [dragonforce](https://threatcluster.io/dark-web/group/dragonforce) | 645 | 0 | 2026-08-24 |
| [safepay](https://threatcluster.io/dark-web/group/safepay) | 549 | 0 | 2026-08-25 |
| [lynx](https://threatcluster.io/dark-web/group/lynx) | 417 | 0 | 2026-08-27 |
| [everest](https://threatcluster.io/dark-web/group/everest) | 392 | 147 | 2026-08-20 |
| [conti](https://threatcluster.io/dark-web/group/conti) | 351 | 0 | 2022-06-07 |
| [lockbit5](https://threatcluster.io/dark-web/group/lockbit5) | 345 | 0 | 2026-08-28 |
| [nightspire](https://threatcluster.io/dark-web/group/nightspire) | 319 | 0 | 2026-08-10 |
| [killsec](https://threatcluster.io/dark-web/group/killsec) | 285 | 214 | 2026-08-23 |
| [rhysida](https://threatcluster.io/dark-web/group/rhysida) | 280 | 0 | 2026-08-28 |
| [ransomhouse](https://threatcluster.io/dark-web/group/ransomhouse) | 205 | 0 | 2026-08-03 |
| [nova](https://threatcluster.io/dark-web/group/nova) | 178 | 0 | 2026-07-25 |
| [handala](https://threatcluster.io/dark-web/group/handala) | 175 | 0 | 2026-04-07 |
| [funksec](https://threatcluster.io/dark-web/group/funksec) | 172 | 0 | 2025-03-18 |
| [cloak](https://threatcluster.io/dark-web/group/cloak) | 166 | 0 | 2026-06-18 |
| [shinyhunters](https://threatcluster.io/dark-web/group/shinyhunters) | 154 | 0 | 2026-08-29 |
| [spacebears](https://threatcluster.io/dark-web/group/spacebears) | 152 | 28 | 2026-08-22 |
| [sarcoma](https://threatcluster.io/dark-web/group/sarcoma) | 141 | 22 | 2026-03-30 |
| [silentransomgroup](https://threatcluster.io/dark-web/group/silentransomgroup) | 137 | 0 | 2026-08-27 |
| [ragnarlocker](https://threatcluster.io/dark-web/group/ragnarlocker) | 128 | 0 | 2023-10-11 |
| [interlock](https://threatcluster.io/dark-web/group/interlock) | 122 | 35 | 2026-08-14 |
| [krybit](https://threatcluster.io/dark-web/group/krybit) | 119 | 0 | 2026-08-26 |
| [direwolf](https://threatcluster.io/dark-web/group/direwolf) | 118 | 0 | 2026-08-30 |
| [toufan](https://threatcluster.io/dark-web/group/toufan) | 117 | 0 | 2023-12-27 |
| [pear](https://threatcluster.io/dark-web/group/pear) | 116 | 29 | 2026-08-24 |
| [genesis](https://threatcluster.io/dark-web/group/genesis) | 115 | 21 | 2026-08-24 |
| [eldorado](https://threatcluster.io/dark-web/group/eldorado) | 112 | 0 | 2025-01-22 |
| [arcusmedia](https://threatcluster.io/dark-web/group/arcusmedia) | 109 | 52 | 2026-08-24 |
| [payoutsking](https://threatcluster.io/dark-web/group/payoutsking) | 109 | 0 | 2026-08-24 |
| [anubis](https://threatcluster.io/dark-web/group/anubis) | 106 | 14 | 2026-08-28 |
| [deadlock](https://threatcluster.io/dark-web/group/deadlock) | 101 | 14 |  |
| [kairos](https://threatcluster.io/dark-web/group/kairos) | 94 | 9 | 2026-08-20 |
| [abyss](https://threatcluster.io/dark-web/group/abyss) | 91 | 49 | 2026-08-26 |
| [threeam](https://threatcluster.io/dark-web/group/threeam) | 91 | 0 | 2026-08-30 |
| [ransomexx](https://threatcluster.io/dark-web/group/ransomexx) | 86 | 10 | 2026-06-20 |
| [chaos](https://threatcluster.io/dark-web/group/chaos) | 85 | 48 | 2026-08-28 |
| [payload](https://threatcluster.io/dark-web/group/payload) | 74 | 63 | 2026-08-20 |
| [beast](https://threatcluster.io/dark-web/group/beast) | 72 | 40 | 2026-08-19 |
| [braincipher](https://threatcluster.io/dark-web/group/braincipher) | 69 | 24 | 2026-07-22 |
| [gunra](https://threatcluster.io/dark-web/group/gunra) | 52 | 0 | 2026-08-18 |
| [ailock](https://threatcluster.io/dark-web/group/ailock) | 51 | 50 | 2026-08-26 |
| [termite](https://threatcluster.io/dark-web/group/termite) | 49 | 42 | 2026-08-13 |
| [settra](https://threatcluster.io/dark-web/group/settra) | 47 | 0 | 2026-08-11 |
| [crypto24](https://threatcluster.io/dark-web/group/crypto24) | 46 | 6 | 2026-04-17 |
| [blacknevas](https://threatcluster.io/dark-web/group/blacknevas) | 45 | 1 | 2026-08-13 |
| [insomnia](https://threatcluster.io/dark-web/group/insomnia) | 45 | 40 | 2026-08-19 |
| [cmdorganization](https://threatcluster.io/dark-web/group/cmdorganization) | 43 | 3 | 2026-07-31 |
| [orova](https://threatcluster.io/dark-web/group/orova) | 43 | 0 |  |
| [global secret group](https://threatcluster.io/dark-web/group/global%20secret%20group) | 42 | 41 |  |
| [securotrop](https://threatcluster.io/dark-web/group/securotrop) | 42 | 37 | 2026-08-18 |
| [j](https://threatcluster.io/dark-web/group/j) | 41 | 0 | 2025-11-01 |
| [storm](https://threatcluster.io/dark-web/group/storm) | 41 | 0 |  |
| [embargo](https://threatcluster.io/dark-web/group/embargo) | 40 | 10 | 2026-06-30 |
| [m3rx](https://threatcluster.io/dark-web/group/m3rx) | 38 | 35 | 2026-08-29 |
| [metaencryptor](https://threatcluster.io/dark-web/group/metaencryptor) | 38 | 32 | 2026-08-23 |
| [crpxo](https://threatcluster.io/dark-web/group/crpxo) | 37 | 0 |  |
| [global](https://threatcluster.io/dark-web/group/global) | 36 | 4 | 2026-08-26 |
| [moneymessage](https://threatcluster.io/dark-web/group/moneymessage) | 35 | 0 | 2026-08-28 |
| [lamashtu](https://threatcluster.io/dark-web/group/lamashtu) | 34 | 0 | 2026-06-17 |
| [dan0n](https://threatcluster.io/dark-web/group/dan0n) | 33 | 0 | 2024-08-23 |
| [aurora](https://threatcluster.io/dark-web/group/aurora) | 32 | 0 | 2026-08-26 |
| [alphalocker](https://threatcluster.io/dark-web/group/alphalocker) | 31 | 1 | 2026-02-28 |
| [l group](https://threatcluster.io/dark-web/group/l%20group) | 28 | 0 |  |
| [bravox](https://threatcluster.io/dark-web/group/bravox) | 27 | 0 | 2026-08-17 |
| [underground](https://threatcluster.io/dark-web/group/underground) | 26 | 13 | 2025-08-15 |
| [werewolves](https://threatcluster.io/dark-web/group/werewolves) | 26 | 0 | 2024-03-04 |
| [dark project](https://threatcluster.io/dark-web/group/dark%20project) | 25 | 0 |  |
| [fulcrumsec](https://threatcluster.io/dark-web/group/fulcrumsec) | 25 | 23 | 2026-06-16 |
| [lapsus$](https://threatcluster.io/dark-web/group/lapsus%24) | 25 | 0 | 2026-06-23 |
| [titan](https://threatcluster.io/dark-web/group/titan) | 24 | 0 | 2026-08-20 |
| [morpheus](https://threatcluster.io/dark-web/group/morpheus) | 23 | 0 | 2026-07-30 |
| [unsafe](https://threatcluster.io/dark-web/group/unsafe) | 22 | 8 | 2026-08-28 |
| [daixin](https://threatcluster.io/dark-web/group/daixin) | 21 | 20 | 2025-09-11 |
| [majinahanashi](https://threatcluster.io/dark-web/group/majinahanashi) | 21 | 0 |  |
| [auditteam](https://threatcluster.io/dark-web/group/auditteam) | 19 | 5 | 2026-08-27 |
| [kazu](https://threatcluster.io/dark-web/group/kazu) | 18 | 0 | 2026-08-23 |
| [alp-001](https://threatcluster.io/dark-web/group/alp-001) | 17 | 0 | 2026-04-08 |
| [panzer](https://threatcluster.io/dark-web/group/panzer) | 17 | 0 |  |
| [shadowbyt3$](https://threatcluster.io/dark-web/group/shadowbyt3%24) | 17 | 0 | 2026-08-29 |
| [booba project](https://threatcluster.io/dark-web/group/booba%20project) | 16 | 0 |  |
| [tridentlocker](https://threatcluster.io/dark-web/group/tridentlocker) | 16 | 16 | 2026-04-26 |
| [zawoo](https://threatcluster.io/dark-web/group/zawoo) | 16 | 0 |  |
| [exfilsquad](https://threatcluster.io/dark-web/group/exfilsquad) | 15 | 13 |  |
| [orion](https://threatcluster.io/dark-web/group/orion) | 14 | 21 | 2026-07-27 |
| [weyhro](https://threatcluster.io/dark-web/group/weyhro) | 14 | 0 | 2025-08-10 |
| [blackout](https://threatcluster.io/dark-web/group/blackout) | 12 | 0 | 2026-07-19 |
| [icarus](https://threatcluster.io/dark-web/group/icarus) | 12 | 0 | 2026-06-23 |
| [imncrew](https://threatcluster.io/dark-web/group/imncrew) | 12 | 0 | 2025-09-16 |
| [blackwater](https://threatcluster.io/dark-web/group/blackwater) | 11 | 10 | 2026-08-24 |
| [cryp70n1c0d3](https://threatcluster.io/dark-web/group/cryp70n1c0d3) | 11 | 0 | 2021-12-18 |
| [black x](https://threatcluster.io/dark-web/group/black%20x) | 10 | 0 |  |
| [doommageddon](https://threatcluster.io/dark-web/group/doommageddon) | 10 | 11 |  |
| [emperador](https://threatcluster.io/dark-web/group/emperador) | 10 | 10 |  |
| [leakbazaar](https://threatcluster.io/dark-web/group/leakbazaar) | 9 | 0 | 2026-05-09 |
| [barracuda](https://threatcluster.io/dark-web/group/barracuda) | 8 | 0 |  |
| [dysphor1a](https://threatcluster.io/dark-web/group/dysphor1a) | 8 | 0 |  |
| [helix](https://threatcluster.io/dark-web/group/helix) | 8 | 8 |  |
| [vanhelsing](https://threatcluster.io/dark-web/group/vanhelsing) | 8 | 0 | 2025-04-05 |
| [wallstreet](https://threatcluster.io/dark-web/group/wallstreet) | 8 | 0 |  |
| [xpl0itrs](https://threatcluster.io/dark-web/group/xpl0itrs) | 8 | 0 | 2026-08-20 |
| [0mega](https://threatcluster.io/dark-web/group/0mega) | 7 | 0 | 2024-01-25 |
| [iah6477](https://threatcluster.io/dark-web/group/iah6477) | 7 | 7 | 2026-08-29 |
| [karma](https://threatcluster.io/dark-web/group/karma) | 7 | 0 | 2021-10-04 |
| [malekteam](https://threatcluster.io/dark-web/group/malekteam) | 7 | 0 | 2024-04-05 |
| [netrunner](https://threatcluster.io/dark-web/group/netrunner) | 6 | 7 | 2026-04-03 |
| [runsomewares](https://threatcluster.io/dark-web/group/runsomewares) | 6 | 6 | 2025-07-10 |
| [secp0](https://threatcluster.io/dark-web/group/secp0) | 6 | 6 | 2026-04-27 |
| [0day syndicate](https://threatcluster.io/dark-web/group/0day%20syndicate) | 5 | 0 |  |
| [atomsilo](https://threatcluster.io/dark-web/group/atomsilo) | 5 | 0 | 2026-02-24 |
| [gdlockersec](https://threatcluster.io/dark-web/group/gdlockersec) | 5 | 0 | 2025-01-26 |
| [ms13089](https://threatcluster.io/dark-web/group/ms13089) | 5 | 8 | 2026-08-15 |
| [prinzeugen](https://threatcluster.io/dark-web/group/prinzeugen) | 5 | 1 | 2026-06-04 |
| [ulose](https://threatcluster.io/dark-web/group/ulose) | 5 | 0 |  |
| [valencialeaks](https://threatcluster.io/dark-web/group/valencialeaks) | 5 | 0 | 2024-09-18 |
| [eclipse](https://threatcluster.io/dark-web/group/eclipse) | 4 | 0 |  |
| [exitium](https://threatcluster.io/dark-web/group/exitium) | 4 | 6 | 2026-04-14 |
| [gammax](https://threatcluster.io/dark-web/group/gammax) | 4 | 4 |  |
| [linkc](https://threatcluster.io/dark-web/group/linkc) | 4 | 0 | 2026-02-27 |
| [satanlockv2](https://threatcluster.io/dark-web/group/satanlockv2) | 4 | 0 | 2025-07-07 |
| [triple x](https://threatcluster.io/dark-web/group/triple%20x) | 4 | 0 |  |
| [d1r](https://threatcluster.io/dark-web/group/d1r) | 3 | 0 |  |
| [ethics](https://threatcluster.io/dark-web/group/ethics) | 3 | 0 |  |
| [rebornvc](https://threatcluster.io/dark-web/group/rebornvc) | 3 | 0 | 2025-07-09 |
| [timc](https://threatcluster.io/dark-web/group/timc) | 3 | 0 | 2026-04-09 |
| [blackfield](https://threatcluster.io/dark-web/group/blackfield) | 2 | 0 |  |
| [bluewhale](https://threatcluster.io/dark-web/group/bluewhale) | 2 | 0 |  |
| [cry0](https://threatcluster.io/dark-web/group/cry0) | 2 | 0 | 2026-08-06 |
| [falcon](https://threatcluster.io/dark-web/group/falcon) | 2 | 1 |  |
| [redact](https://threatcluster.io/dark-web/group/redact) | 2 | 1 |  |
| [sensayq](https://threatcluster.io/dark-web/group/sensayq) | 2 | 0 | 2024-06-04 |
| [the green blood group](https://threatcluster.io/dark-web/group/the%20green%20blood%20group) | 2 | 0 |  |
| [kyber](https://threatcluster.io/dark-web/group/kyber) | 1 | 0 | 2026-03-18 |
| [loki](https://threatcluster.io/dark-web/group/loki) | 1 | 0 | 2026-03-12 |
| [notpetya](https://threatcluster.io/dark-web/group/notpetya) | 1 | 0 |  |
| [robinhood](https://threatcluster.io/dark-web/group/robinhood) | 1 | 0 | 2021-12-06 |
| [sovcali](https://threatcluster.io/dark-web/group/sovcali) | 1 | 0 |  |
| [abrahams_ax](https://threatcluster.io/dark-web/group/abrahams_ax) | 0 | 0 |  |
| [agl0bgvycg](https://threatcluster.io/dark-web/group/agl0bgvycg) | 0 | 0 |  |
| [aptlock](https://threatcluster.io/dark-web/group/aptlock) | 0 | 0 |  |
| [bolt team](https://threatcluster.io/dark-web/group/bolt%20team) | 0 | 0 |  |
| [contfr](https://threatcluster.io/dark-web/group/contfr) | 0 | 0 | 2026-04-22 |
| [darkmatter](https://threatcluster.io/dark-web/group/darkmatter) | 0 | 0 |  |
| [datakeeper](https://threatcluster.io/dark-web/group/datakeeper) | 0 | 0 | 2026-04-22 |
| [goddamn ransomwhere](https://threatcluster.io/dark-web/group/goddamn%20ransomwhere) | 0 | 0 |  |
| [lockbit3_fs](https://threatcluster.io/dark-web/group/lockbit3_fs) | 0 | 64 | 2026-04-22 |
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
