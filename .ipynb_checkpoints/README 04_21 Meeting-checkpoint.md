# Rhode Island State Senate Redistricting Analysis

Completed in 4/23

* Island handling + population documented (New Shoreham: 1,410, 0.128%)
* Population deviation justified (±10%, adjusted to 0.30)
* Ensemble saved (DataFrame + CSV)
* Histograms with enacted reference lines

To do:
* Write metric interpretations (distribution, outliers, partisan/minority impact)
* Add enacted values to box plots
* Write Latino-majority district analysis

From the FinalProject.ipynb: Saw this Note:
TODO: Evidence of convergence (that you have run the chain long enough)

What I think we have left to do: 
1. Project report: OVERLEAF-ONGOING
2. Outlier histograms: Are we planning on doing a Polsby-Popper .. I think its optional
3. Evidence of convergence — still a TODO (multi-size histograms exist, no running means, no write-up) (Done?)
4. Explain parameter choices: OVERLEAF-ONGOING 
5. Marginal box plots: we have, but we still need to do enacted-map overlay missing (Done)
6. visualizations w/ captions/explanations: need to add captions 
7. presentation slides


------------



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



04.28 RECAP


Key Concepts and Requirements

1. Burn-in and Chain Mixing

* The first portion of the Markov chain (approximately the first 10,000 steps) should be treated as burn-in.
* During burn-in, the chain is not yet well mixed and observations are highly correlated.
* After this phase, the chain is considered to be sampling from a stable distribution.
* Analyses and summary statistics should either:
    * exclude the burn-in period, or
    * clearly explain its role in the report.

⸻

2. Running Mean vs Final Mean

* Convergence plots should distinguish between:
    * Running mean (mean up to step t)
    * Final mean (mean across all steps)
* Titles and legends should clearly reflect this distinction.
* Recommended title:

Running Mean vs Final Mean

⸻

3. Convergence Analysis

* Convergence plots must be included and explained.
* Observations:
    * Metrics such as cut edges and efficiency gap stabilize over time.
    * Senate seat counts show very small variation.
* Explanation:
    * Rhode Island is strongly Democratic.
    * Most districts consistently favor the same party, limiting variability.

⸻

4. Population Tolerance (Done that part, need to run)

* The population deviation parameter (epsilon) should be:

epsilon = 0.10

* This reflects typical legal standards (±10% deviation).
* If a higher value (e.g., 0.30) is used, a clear justification must be provided.

⸻

5. Island Connectivity

* Rhode Island includes Block Island, which has population but is geographically disconnected.
* To ensure a valid dual graph:
    * the island must be connected to the mainland.
* This should be briefly mentioned in the report as a Rhode Island–specific preprocessing step.

⸻

6. Scope of Explanation

The report should:

* Focus on interpretation and results
* Avoid detailed explanations of:
    * data cleaning steps
    * Markov chains
    * ReCom algorithm

These are assumed background knowledge.

⸻

Required Plots and Results

Include the following metrics:

For both elections (Presidential and Senate):

* Cut edges
* Latino-majority districts
* Democratic-won districts
* Efficiency gap

⸻

Include results at:

* 20,000 steps
* 40,000 steps

This demonstrates convergence and stability of the chain.

⸻

Additional required visuals:

* Convergence plots (running mean vs final mean)
* Histograms of key metrics
* Marginal box plots (if included in analysis)

⸻

Interpretation Requirements

Your report should clearly explain:

* Why the first 10,000 steps are treated as burn-in
* Evidence of convergence across metrics
* Why Senate seat variation is extremely small
* The significance of Rhode Island’s political composition
* The need to connect the island in the dual graph

⸻

Presentation Guidelines

The presentation should emphasize:

* State-specific insights (Rhode Island characteristics)
* Key findings from plots
* Clear and concise explanations of results

Focus on:

* Political landscape (strong Democratic bias)
* Limited variability in outcomes
* Unique geographic features (island with population)

Avoid:

* General theoretical explanations
* Repetition of standard methodology

⸻

Tasks to Complete

Code Updates

* Set population tolerance to epsilon = 0.10
* Ensure histogram bins are appropriate for discrete data
* Label convergence plots clearly

⸻

Report Updates

* Add explanation of burn-in and convergence
* Include discussion of small-scale variation in Senate results
* Mention island connectivity
* Ensure all plots are clearly labeled and explained

⸻

Final Checks

* Include all required plots for both elections
* Compare results at 20k and 40k steps
* Verify that explanations align with visual results




