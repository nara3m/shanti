# 🧘 Shanti HTML Guide

>success **SH**arable, interactive, st**AN**dalone html dashboard from **T**abular proteom**I**cs data

**Shanti** is a Python library for creating HTML file from proteomics data. Instructions to create HTML file are described [here](https://pypi.org/project/shanti/). A demo HTML file is available [here](https://shanti-v010.netlify.app/). This page explains features of HTML file using the demo file as example.

1. Filter Bars 
2. Search Tool
3. Volcano Plot
4. Histogram 1
5. Histogram 2
6. Protein Table
7. Peptide Table

![Image of HTML file with highlighted components](https://github.com/nara3m/shanti/raw/refs/heads/main/img/components.png)

## 1. Filter Bar

Ue filter bars to adjust x and y axes of Volcano Plot. For example, 

a. display data points with adjusted P value less than 0.05 (biomedical scientists use this cutoff frequently)

![Image of HTML file with p value filter](https://github.com/nara3m/shanti/raw/refs/heads/main/img/filter1.png)

b. to display data points that have log2 fold change greater than 0.4 (not frequently used cutoff, only used for the demo dataset)

![Image of HTML file with log2 fold change filter](https://github.com/nara3m/shanti/raw/refs/heads/main/img/filter2.png)

The default range of filter bars come from the original Proteomics data, therefore fixed before HTML file was created.

⚠️ Only one filter can be adjusted at a time. To adjust both x and y axis at the same time, use Volcano Plot buttons (Buttons will be described in detail in Volcano Plot section)

📝 Adjusting filter bars will automatically change data points in Volcano Plot. But adjusting filter bars will **NOT** automatically re-adjust existing histograms or tables. Remember to reset filters if you would like to see all data points!

## 2. Search Tool

Search Tool works on all _text_ columns (UniProtID, Description, Gene) of full Protein-level table (**6**). For example, try search with `kinase` and notice the data points in Volcano Plot.

![Image of HTML file with **Kinase** in Search bar](https://github.com/nara3m/shanti/raw/refs/heads/main/img/kinase1.png)

📝 To see Protein and Peptide tables of all Kinases, data points must be selected with the __Box Select__ tool of Volcano Plot (described more in Volcano Plot section)

![Image of HTML file with all **Kinase** data points selected](https://github.com/nara3m/shanti/raw/refs/heads/main/img/kinase2.png)

⚠️ Remember to clear the search bar if you want to return to original display of all data points

## 3. Volcano Plot

This is the main component of HTML page. Components **4** to **7** depend on the selected data points in Volcano Plot. There are two ways to select data points: Single data point selection with __Tap__ tool and multiple selection with __Box Select__ tool (located below Volcano Plot). Other tools, [Pan, Box Zoom, Wheel Zoom](https://www.tutorialspoint.com/bokeh/bokeh_plot_tools.htm) allow futher interactions in Volcano Plot.

## 4. Histogram 1 

This is typically the histogram of Protein abundance values (or normalized abundance values) of the Treatment group. In demo example, it is labelled **KO dTAG**. The person creating the HTML file decides which data column from the input Protein table (example, [Test_Shanti_Proteins.xlsx](https://github.com/n3m4u/shanti/raw/refs/heads/main/tests/Shanti_Test_Proteins.xlsx) is used for creating Histogram. The Plot label **KO dTAG** is also defined by the person creating HTML file. 

Number of bins are fixed (20) and the bin sizes are automatically assigned based on the data distribution. 

Data points selected in Volcano Plot are displayed as horizontal lines on top of Histogram. y axis intersection of lines always correspond to the respective bin. For example, if Protein `P09382` is selected in demo example, then the average abundance value of that Protein in **KO dTAG** 107.4 is log2 transformed to 6.75 and displayed in Histogram 1.

⚠️ Note: although frequency bins are correctly assigned, the exact positon (y intersect) of horizontal line within the assigned bin is generated using a random seed to automatically adjust for overlapping lines. The horizontal lines should therefore be interpreted with care.

## 5. Histogram 2

Similar to previous histogram but for the Control group. In demo example, it is labelled **DMSO**

Taken both histograms together, selected proteins overlaid as horizontal lines allow basic interpretations. For example, if protein P09382 is seleced in demo example, then:

![Image of HTML file with selected Proteins overlaied on Histograms](https://github.com/nara3m/shanti/raw/refs/heads/main/img/histogram.png)

a. The selected protein has relatively a low abundance (becuase the y intersects at the lower end)
b. The selected protein has relatively higher abundance in **KO dTAG** group compared to **DMSO** group

## 6. Protein Table

Protein level information of the data points selected in Volcano Plot are displayed as a table. The exact columns to display can be adjusted when creating HTML file. In demo example, columns, UniProtID, Gene, Description, Peptides, PeptidesU (Unique Peptides), PSMs columns from [Test_Shanti_Proteins.xlsx](https://github.com/n3m4u/shanti/raw/refs/heads/main/tests/Shanti_Test_Proteins.xlsx) were displayed.

If multiple data points were selected (with __Box Select__ tool), then it is useful to sort the table.

![Image of HTML file with Peptide Table sorted with Gene column](https://github.com/nara3m/shanti/raw/refs/heads/main/img/table_sort.png)

## 7. Peptide Table

Peptide level information of data points selected in Volcano Plot are displayed as a table. The exact columns to display can be adjusted when creating HTML file. In demo example, columns, UniProtID, Sequence, ProteinGroups, Proteins, PSMs, Position, MissedCleavages, QuanInfo columns from [Test_Shanti_PeptideGroups.xlsx](https://github.com/n3m4u/shanti/raw/refs/heads/main/tests/Shanti_Test_PeptideGroups.xlsx) were displayed.

If multiple data points were selected in Volcano Plot (with __Box Select__ tool), then it is useful to sort the table

# Cite

Marella, N. (2025). Shanti: create SHarable, interactive, stANdalone html dashboard from Tabular proteomIcs data (v0.1.0). Zenodo. https://doi.org/10.5281/zenodo.15307776

########################

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
