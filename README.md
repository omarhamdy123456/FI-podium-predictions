# FI podium predictions
> **Milestone 1 final submission**
> - Notebook: `aclFinal.ipynb` (same as the Kaggle notebook)
> - Report: `F1_Podium_Prediction_Report_Final.pdf`
>
> The other notebooks and `F1_Podium_Prediction_Report.pdf` are earlier work by team members, kept for history.
omar hamdy

This repository holds one notebook, `feature 1 , 2 , 3.ipynb`. It cleans the Formula 1 results, builds a leakage-safe feature table, and answers three questions about the data.

One row is one driver entering one race. The label is a podium finish, `positionOrder <= 3`. The cutoff is after qualifying and before the race starts. The result of the current race, the laps, the pit stops, the status, the points, and the standings published for that same race are not used as features.

## Dataset

Formula 1 World Championship (1950–2024) by Vopani:

https://www.kaggle.com/datasets/rohanrao/formula-1-world-championship-1950-2020

The notebook reads the 14 CSVs from that dataset. It does not overwrite the source files.

## What the notebook does

1. **Setup.** The cleaning, feature, and question code is embedded in the notebook. Kaggle does not need a separate code folder.
2. **File inventory.** Seasons, race counts, and result rows.
3. **Sentinel values.** The Ergast placeholder `\N` is counted before it is converted.
4. **Keys before cleaning.** Primary keys and foreign keys that fail on the raw files.
5. **Repeated driver–race rows.** Early shared drives, plus the one repeated entry after 1964.
6. **Podium definition.** The brief label is compared with the official classification.
7. **Coverage.** Races with no results, years without qualifying, null grid, and unranked standings.
8. **Cleaning rules.** Types, the missing-value policy, and the time checks.
9. **Which row is kept.** Shared drives collapse to one row, with a small example where two rules disagree.
10. **Driver–race table.** What changed after the extra rows were removed.
11. **Handoff checks.** The cleaned table is unique on `(raceId, driverId)` and the raw files are unchanged.
12. **Pre-race features.** Grid, qualifying, recent form, entering standings, and home race for 2003–2024. The split is train 2003–2018, validation 2019–2021, and test 2022–2024.
13. **Front-row circuits.** Which circuits turn a front-row start into a podium, before 2014 and from 2014.
14. **Home races.** Whether a driver podiums more often at home once the grid slot is held fixed.
15. **Mechanical retirements.** Which constructors retire because a component failed, in 2014–2021 and in 2022–2024.

## Run it on Kaggle

1. Open the dataset link above and choose **New Notebook**, so the CSVs are attached.
2. Use **File → Import notebook** and select `feature 1 , 2 , 3.ipynb`.
3. Run all cells from the top.

The setup cell searches `/kaggle/input` for `results.csv`, `races.csv`, `drivers.csv`, and `status.csv`. Run the setup cell before the later cells. Those cells import the code that the setup cell registers.

## Rules that matter

- `\N` is a placeholder, not a number. It becomes a real blank. It is not filled with a mean, median, or mode.
- In `constructor_results`, `\N` means the constructor was not disqualified. That is stored as `disqualified = False`.
- Grid `0` means the driver had no grid slot. A blank grid stays blank.
- A shared drive keeps the better classification: lowest `positionOrder`, then most laps, then smallest `resultId`.
- Form uses only the previous five starts. Entering standings are the table after the previous round.
