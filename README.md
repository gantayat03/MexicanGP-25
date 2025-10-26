# F1 Mexican GP 2025 Winner Prediction - Data Analysis

## Description

This project uses Python and the FastF1 library to perform data analysis and generate a **simulated prediction** for the winner of the 2025 Formula 1 Mexican Grand Prix. It focuses on combining historical performance data with recent practice and qualifying results to create a quantifiable ranking of driver chances.

## Motivation

This analysis aims for transparency in its methodology, building upon feedback received on a previous F1 data analysis project (regarding Carlos Sainz's Singapore stint). Instead of relying on a complex, potentially opaque Machine Learning model for the final prediction step, this project uses:
1.  **Explicitly calculated performance metrics** from real F1 data.
2.  A **clear, weighted scoring simulation** to generate the final win probabilities.
The goal is to "show the work" and explain *how* the data leads to the prediction.

## Features

* Loads qualifying, FP2, and historical race data using the FastF1 library.
* Calculates the **Adjusted Starting Grid** by incorporating official/confirmed grid penalties.
* Determines **Average Long Run Pace** from FP2, filtering for relevant tyre compounds and cleaning data (removing in/out laps).
* Calculates **Tyre Degradation** as a linear slope (seconds/lap increase) based on cleaned FP2 long run stints.
* Computes a **Historical Dominance Factor** based on average finishing positions at the specific track over recent years.
* Combines these features into a single dataset.
* Handles missing data using median imputation.
* **Simulates** win probabilities using a simple weighted scoring method based on the combined features.
* Generates visualizations for the starting grid, long run pace, and tyre degradation.

## Data Sources

* **FastF1 Library:** Provides access to F1 timing data, session information, and results (often via the Ergast API).
* **Official FIA/F1 Sources (Manual Input):** Grid penalty information must be obtained from official FIA documents or reputable F1 news sources and manually entered into the script (Block 2b). FastF1 data alone does not contain penalty details.

## Methodology (Prediction Simulation)

1.  **Feature Extraction:** Calculate the metrics described above (Grid Pos, Pace, Degradation, History) from FastF1 data.
2.  **Data Cleaning:** Filter irrelevant laps, handle DNFs in historical results, impute missing FP2 data.
3.  **Feature Combination:** Create a pandas DataFrame aligning all features per driver.
4.  **Weighted Scoring:** Assign simple, pre-defined weights to each feature (e.g., lower grid position = better score, lower pace = better score). *Note: These weights are based on general F1 knowledge for this simulation, not statistically learned by an ML model.*
5.  **Probability Simulation:** Calculate a weighted score for each driver and normalize these scores to produce pseudo-probabilities that sum to 100%, indicating the simulated likelihood of winning.

## Requirements

* Python 3.x
* Required libraries (install via pip):
    * `fastf1`
    * `pandas`
    * `numpy`
    * `matplotlib`
    * `seaborn`
    * `scikit-learn` (used for StandardScaler/LogisticRegression structure, though model is simulated)

    It's recommended to install using a requirements file:
    ```bash
    pip install fastf1 pandas numpy matplotlib seaborn scikit-learn
    ```
    *(Or create a `requirements.txt` file and use `pip install -r requirements.txt`)*

## Usage

1.  **Clone/Download:** Get the Python script/Jupyter Notebook.
2.  **Install Requirements:** Make sure all necessary libraries are installed.
3.  **Configure:** Check the configuration variables at the top of the script (`YEAR_CURRENT`, `EVENT_NAME`, `YEARS_HISTORICAL`, `RACE_COMPOUND`, `MIN_STINT_LENGTH`).
4.  **Input Penalties:** **Crucially**, update the `penalties` dictionary in **Block 2b** with any *confirmed* grid penalties for the race weekend from official sources.
5.  **Run Script/Notebook:** Execute the Python script or run the Jupyter Notebook cells sequentially. Ensure the FastF1 cache directory (`cache` by default) has write permissions.

## Outputs

* **Console Output:** Status messages during data loading and analysis, calculated average pace/degradation/historical finish per driver, and the final predicted win probability table.
* **Image Files:** PNG files saved in the script's directory:
    * `mexico_gp_2025_top10_adjusted_grid_confirmed.png` (Top 10 starting grid after penalties)
    * `mexico_gp_2025_fp2_long_run_pace.png` (Boxplot comparing FP2 long run pace)
    * `mexico_gp_2025_fp2_degradation_trends.png` (Plot showing lap times and degradation trendlines)

## Limitations

* **Simulated Prediction:** The final win probability is based on *arbitrary weights*, not a statistically trained Machine Learning model. It serves as a demonstration of how features *could* be combined.
* **Race Day Variables:** Does not account for crucial real-time factors like race start performance, pit stop strategy execution, safety cars, changing weather conditions, driver errors, or car reliability.
* **Data Availability:** Relies on data being available via FastF1 and its underlying sources (like Ergast). Recent session data might sometimes be incomplete.
* **Penalty Input:** Requires manual lookup and input of official grid penalties.
* **Linear Degradation:** Assumes tyre degradation is linear, which is a simplification.
* **Data Cleaning/Imputation:** The methods used (median fill) are basic and might not perfectly represent a driver's true potential if their data was missing.

## Future Improvements

* Train an actual ML model (e.g., Logistic Regression, Gradient Boosting) on historical data to learn optimal feature weights.
* Incorporate more features (e.g., tyre compound choices, weather forecasts, performance at similar track types, teammate comparisons).
* Develop a more sophisticated method for handling missing practice data.
* Explore non-linear degradation models.
