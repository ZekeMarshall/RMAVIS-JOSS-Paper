---
title: '`MAVIS` v1.0: a R Shiny application for the analysis of vegetation survey data and assignment to UK NVC communities.'
tags:
  - Vegetation science
  - Classification
  - NVC
  - Ecology
  - Survey
authors:
  - name: Zeke Marshall
    orcid: 0000-0001-9260-7827
    affiliation: 1
  - name: Simon Smart
    orcid: 0000-0003-2750-7832
    affiliation: 1
  - name: Colin Harrower
    orcid: 0000-0001-5070-5293
    affiliation: 2
affiliations:
  - name: UK Centre for Ecology \& Hydrology, Lancaster Environment Centre, Library Avenue, Bailrigg,Lancaster, LA1 4AP, United Kingdom
    index: 1
  - name: UK Centre for Ecology \& Hydrology, Maclean Building, Benson Lane, Crowmarsh Gifford, Wallingford, Oxfordshire, OX10 8BB, United Kingdom
    index: 2
date: 21 February 2024
bibliography: paper.bib
output:
  html_document:
    keep_md: yes
---
  
# Summary

`MAVIS` is a R Shiny application for the assignment of vegetation sample
plot data to British National Vegetation Classification (NVC) 
communities [@rodwell2000].

The assignment of vegetation sample plot data to established vegetation 
classification units using computational methods is a well established and 
recognised practice [@maciejewski2020]. 
The results of this assignment process are used in various ways, 
including assisting in the phase 2 habitat survey
(or NVC survey) [@rodwell2006national] process; 
establishing an ecological baseline and identifying important ecological 
features such as protected habitats [@cieem2022]; 
and by providing a proxy for historical reference ecosystems 
against which to measure ecological restoration progress [@gann2019; @sturbois2023].

In the Great Britain (GB) the development of computational methods and programs 
for the assignment of vegetation survey data to NVC communities began with the 
development of TABLEFIT [@hill1989; @marrs2019] and was followed by MATCH [].
The most recent program, the Modular Analysis of Vegetation Information System 
(MAVIS), was developed by @smart2016 with the latest version released in 2021.


# Statement of need

The requirement for a new program in the form of an R Shiny [@chang2024] 
application arises from several needs in the GB ecology and conservation 
community, namely the need to: 
1) accommodate updates to the NVC;
2) provide a means to easily reproduce and attribute the results of the NVC
assignment process;
3) simplify access to and the use of NVC assignment software;
and
4) facilitate the continuous development of such NVC assignment software.
We developed `MAVIS` considering these needs, with the view to providing
a reliable system for use by the GB ecology and conservation community, 
analogous to the Engine for Relevés to Irish Communities Assignment (ERICA)
tool [@perrin2018; @perrin2019a], developed for Ireland.

# Application structure

Inspired by the extensible structure of the species niche and distribution 
modelling `shiny` application `wallace` [@kass2023], we constructed `MAVIS` with 
a modular architecture, enabling both the easy maintenance of existing modules, 
and easy development of additional modules in the future.

`MAVIS` currently contains a total of eighteen modules, with the fourteen main 
modules summarised in the following table.

|        **Module**       |                                                          **Description**                                                          |
|:-----------------------:|:---------------------------------------------------------------------------------------------------------------------------------:|
|         Sidebar         |                                               Contains the options for each module.                                               |
|        Data Input       |                      Facilitates the entry of data, three methods are provided: manual, upload, and example.                      |
|     Data Validation     |                 Checks the format of the inputted data and provides means to adjust incorrectly formatted entries.                |
|      Data Structure     | Checks the structure of the inputted data, displaying the availability of data by species and number of plots per year and group. |
|      NVC Assignment     |                                        Displays the results of the NVC assignment process.                                        |
|  Habitat Correspondence |           Displays the habitats from alternative habitat classifications associated with the top-fitted NVC communities.          |
|     Floristic Tables    |             Allows the comparison of floristic tables composed from the inputted data with the NVC's floristic tables.            |
|        Frequency        |                         Summarises the frequency of occurrence of each species across all plots over time.                        |
|           EIVs          |         Displays the mean Hill-Ellenberg ecological indicator values (EIVs) for each plot, group of plots, and all plots.         |
|        Diversity        |                              Displays a number of diversity metrics for each plot and group of plots.                             |
|       MVA National      |                        Allows the user to explore the position of the sample plots in an ordination space.                        |
|  MVA Local, restricted  |                        Allows the user to explore the position of the sample plots in an ordination space.                        |
| MVA Local, unrestricted |                        Allows the user to explore the position of the sample plots in an ordination space.                        |
|          Report         |                Provides the user with a downloadable report, containing user-selected outputs from the app session.               |

# Key methodologies

The core function of `MAVIS` is to compare the compositional similarity of a set of
sample plots against GB NVC communities.

To assess the similarity for grouped plots (by year) and all plots (by year) the 
Czekanowski index, also known as the proportional similarity index is used.
To use the Czekanowski index sample plot data is composed into floristic tables
and compared against the NVC's florstic tables, or a subset thereof.
Equation~\autoref{eq:czekanowski_index} outlines the 
Czekanowski index, where $N_{ij}$ is the abundance of species $i$ in group $j$, 
$N_{ik}$ is the abundance of species $i$ in group $k$, $p$ is the total number of 
species in the samples.
As floristic tables from user-inputted sample data tend to be less species-rich Following @malloch species absent from the survey data, but present in the NVC
floristic tables at a constancy of 0.2 are down-weighted to 0.1.

\begin{equation}\label{eq:czekanowski_index}
C_{jk} = 2 \cdot \frac{\sum_{i=1}^{p} min(N_{ij}, N_{ik})}{\sum_{i=1}^{p} N_{ij} + \sum_{i=1}^{p} N_{ik}}
\end{equation}

The similarity between individual plots (by year) and NVC communities are 
quantified by calculating the Jaccard similarity between the sample plots and
pseudo-quadrats generated from the NVC floristic tables. This approach
is noted as improving the accuracy of fit relative to the Czekanowski
index for samples with low species richness \citep{tipping2013}.

\begin{equation}\label{eq:jaccard_similarity}
j = \frac{a}{a + b + c}
\end{equation}

Equation~\autoref{eq:jaccard_similarity} . Where $a$, $b$, and $c$ represent the

To generate the pseudo-quadrats.....

The ordination spaces in the MVA modules are constructed using Detrended 
Correspondence Analysis (DCA) [@hill1980] using the R package `vegan` 
[@oksanen2023]. In the National Reference method

For a more comprehensive description of the methodologies used please
refer to the documentation bundled in `MAVIS` <ADD REF TO AVIS ZENODO REPO HERE>

# Data sources

The list of accepted species was constructed using 
the vascular plant (*Tracheophyta*) taxa present in the 
Botanical Society of Britain and Ireland's (BSBI) database [@bsbi2022TaxonLists];
filtered to include taxa at the genus, species, hybrid species, and subspecies 
ranks which have an associated UK Species Inventory (UKSI) Taxon Version Key 
(TVK).
The moss (*Bryophyta*), liverwort (*Marchantiophyta*), 
and hornwort (*Anthocerotophyta*) taxa present in the UKSI, 
retrieved from the National Biodiversity Network (NBN), filtered to include taxa 
at the species, species aggregate, subspecies, species sensu lato, 
and genus ranks.
The limited number of lichen (*Lecanoromycetes*), charophyte (*Charophyta*), 
and 'algae' taxa present in the NVC floristic tables.

The NVC communities present in `MAVIS` are composed from @rodwell2000, 
@wallace2017, and @prosser2023.

Data for habitat correspondences is derived from: 
the Joint Nature Conservation Committee (JNCC) Spreadsheet of Habitat 
Correspondences [@jncc2008],
UKHab V1.1 [@butcher2020], and
the National Plant Monitoring Scheme (NPMS) habitat correspondences
[@pescott2019].

Data for the vascular plant Hill-Ellenberg values were retrieved from the
BSBI checklists, specifically the Nitrogen Score (N)
[@bsbi2017ellenbergN], Moisture Score (F) [@bsbi2017ellenbergF],
Reaction Score (R) [@bsbi2017ellenbergR], Salinity Score (S)
[@bsbi2017ellenbergS], and Light Score (L) [@bsbi2017ellenbergL]. Data
for bryophytes was taken from BRYOATT [@hill2007].

Four example datasets are bundled with `MAVIS`: 
1) Parsonage Down [@ridding2020],
2) Leith Hill Place Wood [@wood2015; @smart2024],
3) Whitwell Common [@smart2000],
and
4) Newborough Warren [@wallace2018].

# Funding

# Acknowledgements

We would like to thank...for testing `MAVIS`.

We would also like to thank Laurence Jones for providing the Newborough
Warren example dataset and Lucy Ridding for providing the Parsonage Down
example dataset.

# References
