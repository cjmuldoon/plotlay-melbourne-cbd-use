# Opening the Power BI report

`MelbourneCBDUse.pbip` is a Power BI Project: the report (`MelbourneCBDUse.Report`, PBIR format) and the semantic model (`MelbourneCBDUse.SemanticModel`, TMDL) saved as text, so the repo can show every change. Power BI Desktop opens it like a `.pbix`.

## What you need

- Power BI Desktop for Windows, a recent version (2026). If Desktop is older than about mid 2025, turn on **File, Options and settings, Options, Preview features, Power BI Project (.pbip) save option** and **Store reports using enhanced metadata format (PBIR)**, then restart Desktop.
- The repo folder on the Windows machine. In Parallels the Mac home folder is usually shared as `\\Mac\Home`.

## Open it

1. In Windows, go to the repo's `powerbi` folder and double-click `MelbourneCBDUse.pbip`.
2. The visuals are empty the first time. Select **Refresh** on the Home ribbon.
   - By default the data is read from the repo's public copy on GitHub (`DataFolder` is `https://raw.githubusercontent.com/cjmuldoon/plotlay-melbourne-cbd-use/main/data/`). If Desktop asks how to connect, choose **Anonymous** and then **Connect**.
   - To read the files from disk instead, go to **Home, Transform data, Edit parameters** and set `DataFolder` to the full path of the repo's `data` folder, **ending with a backslash**. Then select **OK** and **Apply changes**.
   - If Desktop warns about privacy levels, choose **Ignore privacy levels** (all the sources are public open data).
3. Check the numbers (below).

If the report was updated while open, close Power BI and reopen the .pbip rather than using Apply external changes. (After Apply external changes one export drew the page at about 57% inside a much larger canvas; a full close and reopen put it right.)

## Check the numbers after Refresh

After **Refresh**, with the **Census year** dropdown on **2024** (as the report opens; it takes one year at a time), the cards on **The CBD over the years** read:

| Card | Should read |
|---|---|
| Office floor space, CBD, m² | 2,833,795 |
| Apartment floor space, CBD, m² | 2,276,604 |
| Hotel floor space, CBD, m² | 427,960 |

Pick **2002** and they read 2,745,609, 306,545 and 228,332 (the README's figures). The line chart shows every year whatever the dropdown says; office peaks in 2008 at 3,194,201 m².

**Office out, homes in** lists 19 blocks, most office lost first: Collins St, William St to King St at -33,043 m².

## The model

- **Blocks**: one row per census block (`data/geometry/blocks_wkt.csv`), keyed by `block_id`, with Plotlay's formula columns (office and apartments in 2002 and 2024, the changes, `office_to_living`).
- **Floor**: the census table (`data/clue/floor_space_by_use.csv`), one row per block and year; held back cells are blank.
- **Years** (2002 to 2024): for the year slicers.
- Measures: `Office m2`, `Apartments m2`, `Hotels m2`, `All floor space m2`, `Apartments share`, `Office share`, and on Blocks the two changes. The floor space measures count one census year: the year picked, or the latest year with data (2024) when none is (`Year shown`). Blocks also gets each block's street name (`block_name`, from `data/clue/block_names.csv`) for the charts.

## The two maps

- **Shape Map** (Blocks by year page). The custom map is saved inside the report (`MelbourneCBDUse.Report/StaticResources/RegisteredResources/blocks.topojson`, a copy of `data/geometry/blocks.topojson` that keeps only the block id), so the 603 blocks draw on open, coloured by apartments share for the year picked. It matches `Blocks[key]`. The Shape Map has no base map of its own, so the saved file also carries the context round the municipality: neighbouring suburbs (ABS) in a pale ground tone, with Port Phillip, the Maribyrnong and the Yarra outside the blocks showing as the map's pale blue background (cut out using ABS water mesh blocks and Vicmap Hydro). The plain file from `data/` has the blocks only. If the map shows the United States instead (an older Desktop can ignore a saved map): select the map, **Format visual** (the paintbrush), **Visual**, **Map settings**, **Map type: Custom map**, **Add map type** (or **Browse**), and choose `..\data\geometry\blocks.topojson`. Under **View map keys** the block ids should be matched. If the Shape Map isn't in the Visualizations pane at all, turn it on in **File, Options and settings, Options, Preview features, Shape map visual** and restart Desktop.
- **Azure Maps** (Map of the blocks page). The blocks are plotted from their centroids (averaged, one point per block), with the block id in Location and the street name in the tooltip. Your organisation's admin must allow Azure Maps. For the block outlines add the reference layer by hand (Power BI can't take it from a file in the project): **Format visual**, **Visual**, **Reference layer**, **Data source: File**, and choose `..\data\geometry\blocks.geojson`; its `key` property is the block id, the same value as the map's Location. Use the **Grayscale (light)** or **Blank** map style for screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Blocks[key]`, Color saturation `[Apartments share]`.
- Azure Maps: Location `Blocks[key]`, Latitude `Blocks[centroid_lat]` (Average), Longitude `Blocks[centroid_lng]` (Average), Size `[Homes hotels students change m2]`.

Then, if you like, **File, Save as** a `.pbix`, which includes the data.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Save them to `powerbi/Screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | The CBD over the years | page 1 | still |
| 2 | Apartments by block, 2002 and 2024 | the Census year dropdown on 2002, then 2024 | 2 stills |
| 3 | The years stepping | pick 2002 to 2024 in the dropdown, about 0.5 s each | 12 s clip |
| 4 | Office out, homes in | page 3 | still |

Keep the footer with the attribution visible in every shot.
