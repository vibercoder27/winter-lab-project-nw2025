# Interactive Gene Expression Visualization Tool

A lightweight R Shiny app to explore single‑cell or bulk gene expression tables with your own metadata. Upload two TSV files and get interactive Feature, Bubble, Violin, Heatmap, and Box plots. Exports are available for the Feature plot (PNG and self‑contained HTML).

## Features

- Upload expression matrix (.tsv or .tsv.gz) and metadata (.tsv or .tsv.gz)
- UMAP Feature plot
  - Color by any metadata column
  - Or color by a selected gene’s expression
  - Multiple genes compute a simple “ModuleScore” (row mean) for coloring
  - Facet by a “Split By” metadata column
  - Rich hover with cell, group, and summary stats
  - Download as PNG or self‑contained HTML
- Bubble plot (per group/split): average expression and percent expressing
- Violin plot: expression by group; optional split overlay
- Heatmap: z‑scored average expression by group and split
- Box plot: expression by group; optional split overlay

## Input files

You need two files. Both can be `.tsv` or `.tsv.gz`.

1) **Expression matrix**  
   - First column: `gene` (gene symbols or IDs). Case is normalized to upper‑case.  
   - Remaining columns: one column per cell/sample. Values are numeric expression.  
   - Example:
   ```tsv
   gene    cellA   cellB   cellC
   GAPDH   1.2     0.0     3.4
   ACTB    2.1     0.5     1.0
   ```

2) **Metadata**  
   - First column: `cell` (must exactly match the column names in the expression matrix).  
   - Include any categorical columns you want to group/split by (e.g., `Cluster`, `Sample`, `Condition`).  
   - Include 2 numeric columns named **UMAP_Xaxis** and **UMAP_Yaxis** for Feature plots.  
   - Example:
   ```tsv
   cell    Sample  Cluster UMAP_Xaxis UMAP_Yaxis
   cellA   S1      T_cell  -3.12      5.88
   cellB   S1      B_cell   0.40      2.11
   cellC   S2      Mono     4.92     -1.77
   ```

> Notes
> - Only cells present in both files will be plotted.  
> - The app trims whitespace in `cell` and upper‑cases `gene` names.  
> - Large uploads are supported up to ~600 MB by default. Gzipped TSVs load faster.

## How to run

### Option A: as an app.R (recommended)
1. Save the provided script as `app.R` in a new folder.
2. In R:
   ```r
   install.packages(c(
     "shiny","dplyr","tidyr","readr","ggplot2","plotly",
     "patchwork","RColorBrewer","data.table","shinycssloaders","htmlwidgets","tibble"
   ))
   shiny::runApp()
   ```

### Option B: run from a custom file name
If the file is not named `app.R`, you can still run it by sourcing the file in an R session that has `shiny` loaded:
```r
install.packages(c(
  "shiny","dplyr","tidyr","readr","ggplot2","plotly",
  "patchwork","RColorBrewer","data.table","shinycssloaders","htmlwidgets","tibble"
))
source("Internship Project")  # replace with your actual file name
```

## Using the app

1. Upload **Expression matrix** and **Metadata**.  
2. Choose **Group By** (color legend) and optionally **Split By** (facets).  
3. For Feature plot:
   - Leave “Feature Plot: Color by gene expression” unchecked to color by metadata.
   - Check it and choose one or more genes to color by expression or ModuleScore.
4. Switch tabs for Bubble, Violin, Heatmap, and Box plots.
5. Use the **Download Center** to export the Feature plot as PNG or HTML.

## Plot details

- **Feature plot**
  - Base: UMAP_Xaxis vs UMAP_Yaxis
  - Color by metadata, or by selected gene/ModuleScore
  - Facet on “Split By” when set
  - Downloads: PNG via `ggsave`, HTML via `htmlwidgets::saveWidget` on a `ggplotly` version

- **Bubble plot**
  - Groups by your chosen `Group By` and `Split By`
  - Size = percent of cells with expression > 0
  - Color = average expression

- **Violin plot**
  - One facet per selected gene; y = expression
  - Fill can reflect `Split By` when set
  - Filters out tiny groups (< 2 cells) to avoid noisy violins

- **Heatmap**
  - z‑scored mean expression per group (and split when set)
  - Hover shows gene, group, z‑score

- **Box plot**
  - Expression by group; optional split fill

## Dependencies

- R ≥ 4.1 recommended
- Packages: `shiny`, `dplyr`, `tidyr`, `readr`, `ggplot2`, `plotly`, `patchwork`, `RColorBrewer`, `data.table`, `shinycssloaders`, `htmlwidgets`, `tibble`

## Tips

- Use `.tsv.gz` for faster uploads.
- Ensure `cell` names match exactly between expression and metadata.
- If Feature plot is blank, confirm metadata has `UMAP_Xaxis` and `UMAP_Yaxis`.
- If violins are missing, your groups may have < 2 cells after filtering.

## Roadmap (nice to have)

- Import gene sets to compute weighted module scores
- Add CSV export of per‑group summary stats
- Download buttons for all plot types

## Contributors

Sukirtthan Elanjchezhiyan, Zach Shaprio, Sohum Metha, Kim Shon

## License

Add your license of choice (e.g., MIT) here.
