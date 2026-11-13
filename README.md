# Fifth Harmony: a career in data

A hand-compiled dataset covering Fifth Harmony, built the same way as the Why Don't We dataset. It is the data behind the interactive dashboard page `fifth_harmony-career.html`.

## Files

| File | Rows | What it holds |
|---|---|---|
| `releases.csv` | 33 | Singles, promotional singles, EPs and albums, with dates, certifications and chart notes |
| `videos.csv` | 17 | Music videos with directors |
| `events.csv` | 14 | Tours, TV, milestones and band-status events |
| `awards.csv` | 1 | Award entries and results where stated |
| `members.csv` | 5 | Members, origins and notes |
| `timeline.csv` | 15 | One row per year: era, members, releases and videos |
| `build.py` |  | Generates all CSVs, this README and the dashboard page. Edit the data modules, not the CSVs |

## Column notes

**`releases.csv`**
- `type`: `single`, `promo` (promotional single), `EP` or `album`. Singles where the group is a featured artist are `single` rows with the lead artist in `featured_artist` and "featured" in `notes`.
- `release_date` is ISO format, as precise as the sources allow. `date_precision` says `day`, `month` or `year`, so filter on it before doing date arithmetic.
- `project`: the album, EP or other release a track belongs to, or `Non-album`. This follows the grouping in the source table.
- `lineup`: who was in the group at release. `lineup` is `Quintet` through 2016 and `Quartet` from the first single after Camila Cabello's exit (Down, June 2017).
- `label`: Epic/Syco, as listed in the sources.
- `certifications` uses the source's codes (RIAA, AMPROFON, MC, ARIA, BPI, RMNZ, FIMI and others). `chart_notes` holds only peaks the source states in its tables.

**`events.csv`**: `verification` flags weaker or conflicting items.

**`awards.csv`**: `result` is the outcome where the source states one.

**`timeline.csv`**: `members_during_year` counts anyone in the group at any point in the year; `members_at_year_end` counts who was in it on December 31 (hiatus years still count the members).

## Quick start

```python
import pandas as pd
r = pd.read_csv("releases.csv")
print(r.groupby(["year", "type"]).size().unstack(fill_value=0))
print(r[r.date_precision == "day"].sort_values("release_date").head())
```

## Known limitations

- Birth dates and hometowns were not retrieved, so `members.csv` leaves `born` blank.
- Cabello's exit date is December 19, 2016 (announced the day after the iHeartRadio Jingle Ball date in Florida); Billboard's timeline places the last quintet appearance at New Year's Rockin' Eve instead.
- Worth It and Work from Home certification counts differ between the article text and table; the table is used.
- Noche de Paz sits in its own table in the source (12 promo rows here versus the source's count of 11).
- Reunion and comeback items (2025 and 2026) come from press reports, not the discography. Nothing after the July 24, 2026 teaser was confirmed in the sources found as of November 12, 2026.
- A few low-quality content-farm pages turned up in searches and were not used.
- **No streaming data**: none could be verified, so none is invented.

## Sources and licence

Compiled from Wikipedia and the press reports named in the dashboard footnote. Wikipedia text is licensed CC BY-SA, so if you redistribute this dataset, credit those sources and share it under a compatible licence. Verify anything important against the original sources before relying on it.
