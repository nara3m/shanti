# 🧘 Shanti Guide

>success create **SH**arable, interactive, st**AN**dalone html dashboard from **T**abular proteom**I**cs data

[**Shanti**](https://pypi.org/project/shanti/) is a Python library for creating interactive, standalone HTML dashboards from proteomics data (specifically input data in Tabular format such as Microsoft Excel). 

Instructions to create HTML file (in this page, HTML file is referred with the default file name `dashboard.html`) from [Test_Shanti_Proteins.xlsx](https://github.com/nara3m/shanti/dklfgjasg.xlsx) and [Test_Shanti_Proteins.xlsx](https://github.com/nara3m/shanti/dklfgjasg.xlsx) input files with **Shanti** python library are described later in [Section Test](http://link_to_the_section).

Let's begin with the main components of the `dashboard.html` file.

1. Filter Bars 
2. Search Tool
3. Volcano Plot
4. Histogram 1
5. Histogram 2
6. Protein Table
7. Peptide Table

## 1. Filter Bar

>info Depends on: Input files for **Shanti** python library (example, [Test_Shanti_Proteins.xlsx](https://github.com/nara3m/shanti/dklfgjasg.xlsx) and [Test_Shanti_Proteins.xlsx](https://github.com/nara3m/shanti/dklfgjasg.xlsx)).

>warning Influences: `3. Volcano Plot`

Filter bars can be adjusted to display a specific range of data points in `3. Volcano Plot`. For example, biomedical scientists frequently discard all data points with a -log 10 P value less than 1.3 (equals P value less than 0.05). 

## 2. Search Tool

>info Depends on: Input files for **Shanti** python library (example, [Test_Shanti_Proteins.xlsx](https://github.com/nara3m/shanti/dklfgjasg.xlsx) and [Test_Shanti_Proteins.xlsx](https://github.com/nara3m/shanti/dklfgjasg.xlsx)).

>warning Influences: 3. Volcano Plot

Search Tool can be used to search all `text` columns of `6. Protein Table`. For example, try search with `kinase` and notice the data points displayed in `3. Volcano Plot`.

>danger Remember to clear the search bar if you want to return to original display of all data points in `3. Volcano Plot`

>success Search Tool frequently used in combination with `Box Select` or `Tap` tool (will be explained in next section) of `3. Volcano Plot` to see information of searched Proteins in `6. Protein Table` and `7. Peptide Table`


**Shanti** package simplifies the process of creating interactive volcano plots, histograms and tables. **Shanti** uses [Bokeh](https://bokeh.org) library in the background to generate a HTML file that contains plots and tables. The HTML file can be opened in any browser (Firefox, Chrome, Safari, Edge etc.). It is very convinient to securely send HTML file to collaborators via email or dropbox. HTML file format is chosen becuase it allows end users can explore proteomics data with without requiring any server or software installation.

## 📦 Installation

You can install the package with pip:

```bash
pip install shanti
```

## 🚀 Key Components

`load_data()` loads proteomics data from Excel files, processes it, and prepares it for visualization. The volcano plot visualization includes threshold curves for significance. The curves are calculated based on the threshold function in [CurveCurator](https://github.com/kusterlab/curve_curator) package. Some default parameters are already set in example snippet below. Only one parameter `fc_lim` needs to adjusted frequently.

`make_histogram()` creates histograms of the control and treated sample groups. The bin sizes are set to 20 but can be adjusted in the source code.

`create_interactive_dashboard()` generates an interactive Bokeh dashboard

- A volcano plot showing log2 fold change vs. -log10 adjusted p-value
- Histograms overlaid with selected proteins from volcano plot
- Filter sliders and search functionality
- A protein data table and a peptide data table

`DataProcessor` is the internal Class that handles

- Statistical calculations specifically for protein level data
- Classification of volcano data points based on significance thresholds
- Creation of histograms for protein abundance visualization

## 📂 Input Files Required
- Protein data Excel file (e.g. Shanti_Test_Proteins.xlsx)
- Peptide data Excel file (e.g. Shanti_Test_PeptideGroups.xlsx)

## 🧪 Usage

Here's a simple example to demonstrate how to use the `shanti` package:

```python
from shanti import load_data, make_histogram, create_interactive_dashboard

# Load data with custom parameters
source = load_data(
    file_path = "shanti/data/Shanti_Test_Proteins.xlsx",
    sheet_name=0,
    alpha = 0.05,
    dfn = 10,
    dfd = 10,
    loc = 0,
    scale = 1,
    two_sided=False,
    fc_lim = 0.25,
    l2fc_col = "KO_WT_l2FC",
    pAdj_col = "KO_WT_pAdj"
)
```
> Normal Blockquote

>info Info Blockquote

>warning Warning Blockquote

>danger Danger Blockquote

>success Success Blockquote

Create histograms for visualization:

```python
hist1, hist1_data_filtered, hist1_bin_edges_log, hist1_bottoms, hist1_bar_height = make_histogram(
    source=source,
    hist_col="AN_KO_Mean",
    title="KO dTAG",
    visible=True,
    x_axis_label="protein count"
)

hist2, hist2_data_filtered, hist2_bin_edges_log, hist2_bottoms, hist2_bar_height = make_histogram(
    source,
    hist_col="AN_WT_Mean",
    title="DMSO",
    visible=True,
    x_axis_label="protein count"
)
```

Generate the interactive dashboard:

```python
dashboard_path = create_interactive_dashboard(
    source,
    l2fc_col="KO_WT_l2FC",
    pAdj_col="KO_WT_pAdj",
    html_title="Shanti Tool",
    color_column="color",
    volcano_title="KO dTAG vs DMSO Comparison",
    volcano_tools="pan, box_zoom, wheel_zoom, tap, box_select, reset, save",
    plot2=hist1,
    plot3=hist2,
    hist1_data_filtered=hist1_data_filtered,
    hist2_data_filtered=hist2_data_filtered,
    hist1_bin_edges_log=hist1_bin_edges_log,
    hist2_bin_edges_log=hist1_bin_edges_log,
    hist1_bottoms=hist1_bottoms,
    hist2_bottoms=hist1_bottoms,
    hist1_bar_height=hist1_bar_height,
    hist2_bar_height=hist1_bar_height,
    hist1_col="AN_KO_Mean",
    hist2_col="AN_WT_Mean",
    table_columns=["UniProtID", "Gene", "Description", "Peptides", "PeptidesU", "PSMs"],
    peptides_file="shanti/data/Shanti_Test_PeptideGroups.xlsx",
    peptide_columns=["UniProtID", "Sequence", "ProteinGroups", "Proteins", "PSMs", "Position", "MissedCleavages", "QuanInfo"],
    output_path="dashboard.html"
)
```

## 📊 Final Output

The result is a fully interactive HTML dashboard (`dashboard.html`) which you can open in any browser.

- Volcano Plot showing log fold change vs p-value
- Histograms comparing protein abundance distribution overlaid with selected proteins
- Interactive tables of proteins and peptides
- Ability to click/select proteins and see related peptides instantly

## 🧑‍💻 For Developers
To extend or modify this tool:

- Check the shanti source folder
- Edit the histogram, volcano, or dashboard layout logic
- Test using Jupyter notebooks or scripts

## 🙋 FAQ
**Q**: What kind of Excel format is expected?
**A**: The protein file should contain fold change and p-value columns. The peptide file should contain UniProt IDs and sequence-level info.

**Q**: Does it support .csv files?
**A**: Not yet, but it's easy to adapt by editing the load_data function.

## 📬 Questions?
Feel free to open an issue or reach out with feedback!
