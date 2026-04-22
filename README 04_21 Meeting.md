# Rhode Island State Senate Redistricting Analysis

## Overview
This project analyzes the fairness of the 2022 Rhode Island State Senate map using ensemble methods with GerryChain. The enacted map is compared against a large set of randomly generated districting plans to evaluate metrics such as partisan outcomes, compactness, and demographic representation.

## Data Collection and Preparation

### Data Sources
- 2022 Rhode Island State Senate districts  
- 2020 Census population data (PL 94-171)  
- 2020 election results (VEST dataset, imported from 2018 format)  

### Island Handling (Graph Connectivity)

Rhode Island includes geographic regions (islands) that are not naturally connected to the mainland in the adjacency graph.

#### Problem
GerryChain requires the graph to be fully connected. Disconnected components, such as islands, prevent the Markov Chain from running correctly.

#### Solution
The island node (precinct 89) was connected to a nearby mainland node (precinct 91):

```python
RI_graph.add_edge(89, 91)
```

#### Justification
- Only one connection was added to avoid unrealistic district shapes  
- Connecting to multiple nodes could create unnatural districts or bottlenecks  
- The connection is geographically reasonable (nearest mainland neighbor)  

#### Population Consideration
The island has a relatively small population (to be verified and cited), so its effect on districting outcomes is minimal.

#### Note
This modification is documented because it is a non-standard preprocessing step.

## Population Deviation

A 10 percent population deviation constraint was used in this analysis.

### Justification
- This is a State Senate map, not a Congressional map  
- Courts generally allow up to ±10 percent deviation for state legislative districts  
- This is standard practice in redistricting analysis  

## Data Storage Strategy

### Initial Approach
Simulation outputs were stored in Python lists:

```python
pres_dem_seats = []
sen_dem_seats = []
cutedge_ensemble = []
pres_eg_scores = []
sen_eg_scores = []
latino_districts_ensemble = []
```

### Improved Approach
Results are stored in a Pandas DataFrame and exported to CSV:

```python
import pandas as pd

df = pd.DataFrame({
    "pres_dem_seats": pres_dem_seats,
    "sen_dem_seats": sen_dem_seats,
    "cut_edges": cutedge_ensemble,
    "pres_eg": pres_eg_scores,
    "sen_eg": sen_eg_scores,
    "latino_districts": latino_districts_ensemble
})

df.to_csv("ensemble_results.csv", index=False)
```

### Benefits
- Prevents data loss after long simulation runs  
- Simplifies analysis and visualization  
- Improves reproducibility  

## Ensemble Analysis

A large number of districting plans are generated using a Markov Chain Monte Carlo (MCMC) approach.

### Metrics Collected
- Democratic seats (Presidential and Senate elections)  
- Efficiency Gap  
- Number of cut edges (compactness proxy)  
- Number of Latino-majority districts  

## Implemented Map Comparison

The enacted map is compared to the ensemble to evaluate fairness.

### Requirement
Each histogram must include:
- Distribution of ensemble plans  
- A vertical line representing the implemented map  

### Example

```python
plt.hist(cutedge_ensemble, bins=30)
plt.axvline(implemented_cut_edges, color='red', linewidth=2)
```

## Interpretation of Results

For each metric, the following questions should be addressed:

- Where does the implemented map fall within the distribution?  
- Is it typical or an outlier?  
- Does it favor a particular political party?  
- Does it affect minority representation?  

Visualizations should include:
- Histograms with reference lines  
- Box plots with highlighted points  

## Key Insights (Preliminary)

- Rhode Island appears to be heavily Democratic, which aligns with observed seat distributions  
- Ensemble analysis helps determine whether outcomes are expected given geography or suggest potential bias  

## Important Notes

- Any non-standard preprocessing steps, such as connecting islands, must be clearly documented and justified  
- Results should be interpreted and explained, not just presented  

## Future Work

- Verify and include exact island population  
- Expand interpretation in the discussion section  
- Add additional visualizations and comparisons  

## Summary

This project:
- Constructs a valid redistricting graph, including island connectivity  
- Generates an ensemble of districting plans  
- Compares the enacted map against neutral alternatives  
- Evaluates fairness using multiple quantitative metrics  
