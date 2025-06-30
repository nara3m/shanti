# 🧘 Shanti HTML Guide

>success **SH**arable, interactive, st**AN**dalone html dashboard from **T**abular proteom**I**cs data

**Shanti** is a Python library for creating HTML file from proteomics data. Instructions to create HTML file are described [here](https://pypi.org/project/shanti/). A demo HTML file is available [here](https://shanti-v010.netlify.app/). This page explains 7 features of HTML file using the demo file as example.

1. Filter Bars 
2. Search Tool
3. Volcano Plot
4. Histogram 1
5. Histogram 2
6. Protein Table
7. Peptide Table

![Image of HTML file with highlighted components](https://github.com/nara3m/shanti/raw/refs/heads/main/img/components.png)

If images are not displayed correctly here, visit the original documentation page 
[nara3m.github.io/shanti](https://nara3m.github.io/shanti/index.html)

## 1. Filter Bar

Ue filter bars to adjust x and y axes of Volcano Plot. For example, 

a. display data points with adjusted P value less than 0.05 (biomedical scientists use this cutoff frequently)

![Image of HTML file with p value filter](https://github.com/nara3m/shanti/raw/refs/heads/main/img/filter1.png)

b. to display data points that have log2 fold change greater than 0.4 (not frequently used cutoff, only used for the demo dataset)

![Image of HTML file with log2 fold change filter](https://github.com/nara3m/shanti/raw/refs/heads/main/img/filter2.png)

The default range of filter bars come from the original Proteomics data, therefore fixed before HTML file was created.

⚠️ Only one filter can be adjusted at a time. To adjust both x and y axis at the same time, use Volcano Plot [buttons](https://www.tutorialspoint.com/bokeh/bokeh_plot_tools.htm)

Adjusting filter bars will automatically change data points in Volcano Plot but will **NOT** automatically re-adjust existing histograms or tables. 

Remember to reset filters if you would like to see all data points!

## 2. Search Tool

Search Tool works on all _text_ columns (in demo HTML file, columns UniProtID, Description, Gene are __text__ columns) of full Protein-level table. For example, try search with `kinase` and notice the data points in Volcano Plot.

![Image of HTML file with **Kinase** in Search bar](https://github.com/nara3m/shanti/raw/refs/heads/main/img/kinase1.png)

💡 To see Protein and Peptide tables of all searched `Kinase`s, data points must be selected with the [__Box Select__](https://www.tutorialspoint.com/bokeh/bokeh_plot_tools.htm) tool of Volcano Plot

![Image of HTML file with all **Kinase** data points selected](https://github.com/nara3m/shanti/raw/refs/heads/main/img/kinase2.png)

⚠️ Remember to clear the search bar if you want to return to original display of all data points

## 3. Volcano Plot

This is a core component of HTML page becasue components **4** to **7** depend on the selected data points in Volcano Plot. 

There are two ways to select data points: Single data point selection with __Tap__ tool and multiple selection with __Box Select__ tool (located below Volcano Plot). Other tools, [Pan, Box Zoom, Wheel Zoom](https://www.tutorialspoint.com/bokeh/bokeh_plot_tools.htm) allow futher interactions in Volcano Plot.

All significantly up and down regulated proteins are highlighted with blue and orange colors respectively. The threshold for significance are shown as curved lines. For example, all proteins that are upregulated in **KO dTAG** compared to **DMSO** and above significance threshold are displayed as blue data points in demo example. 

The threshold curves are defined at the time of HTML file creation. If you would like to set different thresholds, then the HTML file must be re-created with [Shanti](https://pypi.org/project/shanti/) python library.

## 4. Histogram 1 

This is typically the histogram of Protein abundance values (or normalized abundance values) of the Treatment group. In demo example, it is labelled **KO dTAG**. The person creating the HTML file decides which data column from the input Protein table should be used for Histogram 1. For example, column **AN_KO_Mean** in [Test_Shanti_Proteins.xlsx](https://github.com/n3m4u/shanti/raw/refs/heads/main/tests/Shanti_Test_Proteins.xlsx) is used for creating Histogram 1. The Plot label **KO dTAG** can be adjusted at the time of creating HTML file. 

Number of bins are fixed (20) and the bin sizes are automatically assigned based on the data distribution. Number of proteins per bin are displayed in x axis label of Histogram. 

Data points selected in Volcano Plot are displayed as horizontal lines on top of Histogram. y axis intersection of lines always correspond to the respective bin. For example, if Protein `P09382` is selected in demo example, then the average abundance value of that Protein in **KO dTAG** 107.4 is log2 transformed to 6.75 and displayed in Histogram 1.

⚠️ Note: although frequency bins are correctly assigned to the selected protein, the exact positon (y intersect) of horizontal line within the assigned bin is generated using a random seed to automatically adjust for overlapping lines. Therefore, if the same data point is selected multiple times, then the horizontal line is drawn at slightly different y intersect (but always within correct bin). The horizontal lines should therefore be interpreted with care.

## 5. Histogram 2

Similar to previous histogram but for the Control group. In demo example, it is labelled **DMSO**

Taken both histograms together, selected proteins overlaid as horizontal lines allow basic interpretations. For example, if protein P09382 is seleced in demo example, then:

a. The selected protein has relatively a low abundance (becuase the y intersects at the lower end)

b. The selected protein has relatively higher abundance in **KO dTAG** group compared to **DMSO** group

![Image of HTML file with selected Proteins overlaied on Histograms](https://github.com/nara3m/shanti/raw/refs/heads/main/img/histogram.png)

## 6. Protein Table

Protein level information of the data points selected in Volcano Plot are displayed as a table. The exact columns to display can be adjusted when creating HTML file. In demo example, columns, UniProtID, Gene, Description, Peptides, PeptidesU (Unique Peptides), PSMs columns from [Test_Shanti_Proteins.xlsx](https://github.com/n3m4u/shanti/raw/refs/heads/main/tests/Shanti_Test_Proteins.xlsx) were displayed.

If multiple data points were selected (with __Box Select__ tool), then it is useful to sort the table.

![Image of HTML file with Peptide Table sorted with Gene column](https://github.com/nara3m/shanti/raw/refs/heads/main/img/table_sort.png)

## 7. Peptide Table

Peptide level information of data points selected in Volcano Plot are displayed as a table. The exact columns to display can be adjusted when creating HTML file. In demo example, columns, UniProtID, Sequence, ProteinGroups, Proteins, PSMs, Position, MissedCleavages, QuanInfo columns from [Test_Shanti_PeptideGroups.xlsx](https://github.com/n3m4u/shanti/raw/refs/heads/main/tests/Shanti_Test_PeptideGroups.xlsx) were displayed.

If multiple data points were selected in Volcano Plot (with __Box Select__ tool), then it is useful to sort the table

## Cite

Marella, N. (2025). Shanti: create SHarable, interactive, stANdalone html dashboard from Tabular proteomIcs data (v0.1.1). Zenodo. https://doi.org/10.5281/zenodo.15307776


## 📬 Questions?
Feel free to contact [Nara Marella](https://linkedin.com/in/nara3m)
