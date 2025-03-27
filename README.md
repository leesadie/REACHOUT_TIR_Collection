# T1D REACHOUT Time in Range Data Collection and Analysis

To outline collection of Time in Range (TIR) data, defined as time spent by the patient within the 3.9-10.0 mmol/L range. TIR will be collected for the 14 days before Day 0, and 14 days before the last day of month 6 to observe the effect of the T1D REACHOUT intervention as an exploratory outcome.

## Devices
This protocol applies to the following continuous glucose monitors (CGMs):
- Dexcom G Series - 5 minute collection rtCGM (288 readings per day)
	- Dexcom G6
	- Dexcom G7
- Freestyle Libre Series  - 1 minute collection rtCGM (1440 readings per day)
	- Libre 2
	- Libre 3

## Data
The `data_test` folder contains sample data that was used in the development of the scripts.

The final `data` folder should have the following structure: 
```
├── Data
│   ├── Participants
│   │   ├── Waitlist
│   │   │   ├── Dexcom
│   │   │   ├── Libre2
        ├── Treatment
│   │   │   ├── Dexcom
│   │   │   ├── Libre2
│   ├── PeerSupporters
│       ├── Dexcom
│       ├── Libre2
```

## Analysis
**File**
The code for analysis is in `TIR_Final.ipynb`. Libraries used are: 
- `tidyverse` - for reading, analyzing, and visualizing data
- `lubridate` - for working with datetime data

For Dexcom and FreeStyle Libre users, functions are defined separately given the differences in the format of their data. The functions are then applied to each group for each CGM: waitlist participants, treatment participants, and peer supporters, with a dataframe created for each group. The dataframes are then binded across CGMs: waitlist participants across Dexcom and FreeStyle Libre, treatment participants across Dexcom and FreeStyle Libre, and peer supporters across Dexcom and FreeStyle Libre.

> [!NOTE]  
> We are not comparing between types of CGMs, but rather looking to see differences, if any, of the intervention across all participants regardless of CGM.

TIR is collected with the following formulas, where `in_range` summates the number of glucose values in a vector, $x_i$, between 3.9 and 10.0 (mmol/L).  Let $y_i$ be a vector of the total number of readings in timepoints, from vector space $Y_i$, where TIR calculates the percentage of `in_range` for the vector, $y_i$:

$$
in \textunderscore range = \sum_{i=1}^{N}(3.9 \leq x_i \leq 10.0)
$$

$$
TIR = (in \textunderscore range \times 1000 / dim(y_i)) / 10
$$

**Additional metrics**
Time above range (TAR) and time below range (TBR) follow the above formulas, where TAR calculates the percentage of glucose values strictly greater than 10.0 and TBR calculates the percentage of glucose values strictly less than 3.9.

Other CGM-derived glycemic metrics are also calculated, being the other core endpoints as suggested by international consensus [1].
- Mean sensor glucose
- Standard deviation of mean glucose
- Glucose management indicator (GMI)
- Coefficient of variation

**Visualization**
The visualization shows differences in TIR from baseline to post-intervention (14 days before Day 0 to 14 days after the 6-month end point). The code for visualization is meant to be run once TIR has been calculated for both baseline and 6-months. Example output of the visualization was run on test data and can be found in `Visualization.ipynb`.

## References
[1] Battelino T, Alexander CM, Amiel SA, Arreaza-Rubin G, Beck RW, Bergenstal RM, Buckingham BA, Carroll J, Ceriello A, Chow E, Choudhary P, Close K, Danne T, Dutta S, Gabbay R, Garg S, Heverly J, Hirsch IB, Kader T, Kenney J, Kovatchev B, Laffel L, Maahs D, Mathieu C, Mauricio D, Nimri R, Nishimura R, Scharf M, Del Prato S, Renard E, Rosenstock J, Saboo B, Ueki K, Umpierrez GE, Weinzimer SA, Phillip M. Continuous glucose monitoring and metrics for clinical trials: an international consensus statement. Lancet Diabetes Endocrinol. 2023 Jan;11(1):42-57. doi: 10.1016/S2213-8587(22)00319-9. Epub 2022 Dec 6. PMID: 36493795.