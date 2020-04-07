* A repository name that is descriptive of the analysis conducted
# Maryland Election Cluster Analysis
Exploring [Maryland's Official Turnout Statistics for the 2016 General Election](https://elections.maryland.gov/elections/2016/index.html) in Maryland by Party and Precinct.

Original dataset used can be found [here](https://github.com/CamilaCamacho/baltimore_election_cluster_analysis/blob/master/Official%20by%20Party%20and%20Precinct.csv).

### Maryland 2016 Election Data: How are Maryland voting precincts grouped based on how they vote?
This information could be helpful in allocating resources to different precinct clusters based on the expected demand.

#### Metrics
How/When Maryland citizens vote:
* **Polls**: November 8, 2016 (7am-8pm)
* **Early Voting**: October 27, 2016 - November 3, 2016 (8am-8pm)
* **Absentee**: Absentee Ballot requested by mail or digitally before November 01, 2016. Sent in before 
* **Provisional**: Ballot where vote has questionable eligibility that must be resolved before vote can count [(more info)](https://en.wikipedia.org/wiki/Provisional_ballot)

## Data Analysis & Visualization
Perform calculations and model-building to answer the data-related questions
Relate the data findings back to your initial question and outline what your cluster model tells us about the related organization

[] Analyzed Excel documents with Excel formulas and data visualizations
* Excel Data Analysis
Use Excel Solver to perform a cluster analysis for an organization listed in the project outline. Calculate z-scores, define clusters, and identify which data points are categorized in each cluster.

## Data Interpretation & Findings 
* README that summarizes findings (less than 250 words), outlines data analytics process, includes at least one data visualization, and provides links to outside sources
* What do your findings mean and why might this be important for an organization relevant to this datasource? Highlight the characteristics of each cluster, how this information should be used in practice, and what additional data might be useful for further analysis.
* Excel Data Visualization
Create at least one visualization that shows the groupings based on your cluster analysis. This should show either the groupings in your cluster analysis (the list of data for each cluster) or the characteristics of the cluster nodes.

--- 
## Step-by-Step Instructions for Excel Data Analysis
* Step-by-step instructions for Excel data analysis (either in README or as a separate document in the repository)

0. As mentioned above, we will be using [Maryland's Official Turnout Statistics for the 2016 General Election by Party and Precinct](https://github.com/CamilaCamacho/baltimore_election_cluster_analysis/blob/master/Official%20by%20Party%20and%20Precinct.csv) dataset.
1. Clean Data:
* Delete _CONGRESSIONAL_DISTRICT_CODE_ and _LEGISLATIVE_DISTRICT_CODE_ columns because they are not necessary in our calculations. 
* The _PRECINCT_ column contains an identifier for each precinct in a county, however, that identifier is non-unique between counties. To create a unique identifier for each precinct, we will combine the county name in the _LBE_ column and the precinct identifier in the _PRECINCT_ column. 
  * Create a new column after _PRECINCT_ column and before _PARTY_ column called _LBE Precinct_.
  * Copy the following formula in the _LBE Precinct_ column:
   `=(LBEcell)&"-"&(PRECINCTcell)`
* Each precinct in every county has their turnout data separated by party (DEMOCRAT, GREEN, LIBERTARIAN, OTHER PARTIES, REPUBLICAN, UNAFFILIATED). To consolidate the turnout data for each precinct in every county:
  * Place cursor where you want the new consolidated data to begin.
  * Go to the _Data_ tab and select the _Consolidate_ functionality.
  * _Function_ should be *Sum* since we want the turnout data for each party to be added together. 
  * Select _Reference_ as the range containing the _LBE Precinct_ as the left-most column and containing the rest of the data to the right. 
  * Add this range as under *All references* and under *Use labels in* check *Left column*, then press *Okay*. 
* Create _LBE Precinct #_ column at the very left that numbers all the precincts. 
* Name this whole data range as *precinct_dataset*
2. Preparing for Cluster Analysis:
* Calculate `=Average()` and `=STDEV()` for each data column including Polls,	Early Voting,	Absentee,	Provisional, and	Eligible Voters. 
* Since the number of eligible voters varies greatly between precincts, we standardize the data to be on the same scale in order for us to adequately compare it them to one another to cluster accordingly.
  * Create new rows for the standardized values for each data row called: _z-Polls_,	_z-Early Voting_,	_z-Absentee_,	_z-Provisional_, and	_z-Eligible Voters_. 
  * Use the formula `=STANDARDIZE((DATAcell),(DATAaverage),(DATAstdev))` for all columns of standardized data values. 
3. Cluster Analysis
* For simplicity, we will start with the first 3 precincts as the trial cluster anchors.
  * Create _Cluster LBE Precinct #_ column with values: 1,2,3
  * Use the following to identify the names of each LBE Precinct cluster center. 
  * Use `=VLOOKUP((ClusterLBEPrecinct#cell),*precinct_dataset*,2)` where 2 refers to the _LBE Precinct_ column because it's second.
  * Use `=VLOOKUP((ClusterLBEPrecinct#cell),*precinct_dataset*,(z-DATAcell))` to identify the standardized z-scores for each clustor center for each data column.
* Now, compute the squared distance of each precinct to each clustor center.
  * Create new _SqDist from cluster #_ columns for each cluster
  * Use formula  `=SUMXMY2(CLUSTERzScoreRange,PRECINCTzScoreRange)` for each _SqDist from cluster #_ column.
* Compute distance from each precinct to the "closest" cluster anchor
  * Create new _Min Distance_ column 
  * Use formula `=MIN(PRECINCTSqDists)`
  * Use formula `=SUM(MinDistances)` to compute the sum of squared distances of all precincts from their cluster anchor
* Find clustor anchor for each precinct
  * Create new column _Cluster Assigned_
  * Use forumula `=MATCH((PRECINCTMinDist), (PRECINCTSqDists), 0)` to identify the clustor anchor with the smallest squared distance. 
* Find optimal clustor anchors
  * Go to the _Data_ tab and select the _Solver_ functionality.
  * *Set Objective* should be the sum of squared distances which we want to be optimized to *Min* value.
  * Select the _LBE Precinct #_ column range to be the *Changing Variable Cells*.
  * *Subject to the Constraints*: (LBEPrecinct#range) <= (number of precincts (2,014)), (LBEPrecinct#range) >= 1, (LBEPrecinct#range) = integer.
  * *Select a Solving Method* to be *Evolutionary*, select *Options* and in the *Evolutionary* tab make the *Mutation rate* 0.5 to improve performance. 
  * Unselect *Make Unconstrained Variables Non-Negative*
  * Press *Solve* and wait for *Sovler Results* pop-up. *Keep Solver Solutions* and press *Okay*.
