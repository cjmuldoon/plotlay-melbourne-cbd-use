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
3. After the refresh, check page 1: the office line ends near the README's 2024 CBD office figure and the apartments line near its apartments figure. If both match, everything is joined correctly.

## The model

- **Blocks**: one row per census block (`data/geometry/blocks_wkt.csv`), keyed by `block_id`, with Plotlay's formula columns (office and apartments in 2002 and 2024, the changes, `office_to_living`).
- **Floor**: the census table (`data/clue/floor_space_by_use.csv`), one row per block and year; held back cells are blank.
- **Years** (2002 to 2024): for the year slicer.
- Measures: `Office m2`, `Apartments m2`, `Hotels m2`, `All floor space m2`, `Apartments share`, `Office share`, and on Blocks the two changes.

## Two map settings Power BI asks for by hand

- **Shape Map** (page 1). Select the map. In **Format visual, Visual, Map settings**, set the map type to **Custom map**, choose **Add map type**, and pick `..\data\geometry\blocks.topojson`. Under **View map keys** the block ids should match. If the Shape Map isn't in the Visualizations pane, turn it on in **Options, Preview features, Shape map visual**.
- **Azure Maps** (Map of the blocks page). Your organisation's admin must allow it. For the block outlines, go to **Format visual, Reference layer**, set the data source to **File** and choose `..\data\geometry\blocks.geojson`, with join property `key`. Use the **Grayscale (light)** or **Blank** map style for screenshots.

If a map visual comes up empty, drag the fields back:
- Shape Map: Location `Blocks[key]`, Color saturation `[Apartments share]`.
- Azure Maps: Latitude `Blocks[centroid_lat]`, Longitude `Blocks[centroid_lng]`.

Then save with **File, Save**, and if you like **File, Save as** a `.pbix`, which includes the data.

## Publish (only if you choose to)

Use **Home, Publish**. In the Power BI service, set the semantic model's **Data source credentials** for the GitHub address to **Anonymous**. Scheduled refresh then works with no gateway. **Publish to web** makes a public link, so only use it if you mean the report to be public.

## Shot list for the website and the video

Capture at 1920 × 1080 (Windows display scaling 100%), with the Plotlay theme. Save them to `powerbi/screenshots/`.

| # | Shot | How | Length |
|---|---|---|---|
| 1 | The CBD over the years | page 1 | still |
| 2 | Apartments by block, 2002 and 2024 | the year slicer | 2 stills |
| 3 | The years stepping | click 2002 to 2024, about 0.5 s each | 12 s clip |
| 4 | Office out, homes in | page 3 | still |

Keep the footer with the attribution visible in every shot.
