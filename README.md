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
# Step-by-Step Instructions for Excel Data Analysis
* Step-by-step instructions for Excel data analysis (either in README or as a separate document in the repository)

Since the number of eligible voters varies greatly between counties, we standardize the data to be on the same scale in order for us to adequately compare it them to one another to cluster accordingly. 

The number of people voting in polls, ev,... drives the clusters but because these numbers are dependant on total eligible voters in a county, we standardize each voting option. By working with these standardized values, we ensure that our analysis 
