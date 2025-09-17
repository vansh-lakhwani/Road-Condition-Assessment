# Road Condition Assessment using Smartphone Sensor Data
This project leverages sensor data from a smartphone—specifically the accelerometer, gravity sensor, and GPS—to assess and classify road surface conditions. By applying machine learning techniques, this analysis categorizes road segments into 'Smooth', 'Moderate', or 'Rough', and visualizes the results on an interactive map.
# Project Overview
The primary goal is to provide an automated, data-driven method for road quality monitoring. This can be useful for municipal authorities for maintenance planning, for navigation apps to suggest smoother routes, or for vehicle dynamics research. The workflow involves collecting and merging time-series sensor data, engineering relevant features, handling outliers, and using K-Means clustering to classify the road conditions.
## Methodology
The project follows a comprehensive data science pipeline:
1.  **Data Ingestion:** Three separate CSV files containing Accelerometer, Gravity, and Location data are loaded into pandas DataFrames.
2.  **Data Preprocessing and Merging:**
      * The timestamps across the accelerometer and gravity datasets are validated to ensure alignment.
      * A time-based merge is performed using `pandas.merge_asof`. This is crucial for combining datasets with different sampling frequencies (e.g., high-frequency accelerometer data with lower-frequency GPS data) by matching records to their nearest timestamp.
3.  **Feature Engineering:**
      * The individual x, y, and z components from the accelerometer and gravity sensors are used to calculate the total vector magnitude. The **Total Acceleration** is the key feature for detecting bumps and vibrations.
      * It is calculated as:
        $$
        $$$$\\text{Total Acceleration} = \\sqrt{x\_{acc}^2 + y\_{acc}^2 + z\_{acc}^2}
        $$
        $$$$
        $$
4.  **Data Cleaning and Scaling:**
      * **Outlier Detection:** Boxplots are used to visualize the distribution of sensor readings and identify outliers, which often represent significant road events like potholes or speed bumps.
      * **Outlier Capping:** The Interquartile Range (IQR) method is applied to cap extreme outliers, preventing them from disproportionately influencing the clustering algorithm.
      * **Feature Scaling:** The data is scaled using `MinMaxScaler` and `Normalizer` to ensure that all features contribute equally to the clustering process.
5.  **Machine Learning - K-Means Clustering:**
      * An unsupervised K-Means clustering algorithm with 3 clusters (`n_clusters=3`) is applied to the processed data.
      * The features used for clustering include the three-axis accelerometer data (`x_acc`, `y_acc`, `z_acc`), `speed`, `altitude`, and the engineered `Total Acceleration`.
      * The algorithm groups data points into three distinct clusters based on their characteristics.
6.  **Cluster Interpretation and Visualization:**

      * The clusters are semantically labeled by analyzing the mean 'Total Acceleration' for each group. The cluster with the lowest mean acceleration is labeled **'Smooth Road'**, the middle is **'Moderate Road'**, and the highest is **'Rough Road'**.
      * An interactive map is generated using the **Folium** library. GPS coordinates are plotted and color-coded based on their assigned road condition label:
          * \<span style="color:green"\>**Green:**\</span\> Smooth Road
          * \<span style="color:orange"\>**Orange:**\</span\> Moderate Road
          * \<span style="color:red"\>**Red:**\</span\> Rough Road
      * The final map is saved as an interactive HTML file (`road_conditions_map.html`).

## Key Libraries Used

  * **Data Handling:** `pandas`, `numpy`
  * **Machine Learning:** `scikit-learn` (for KMeans, StandardScaler, MinMaxScaler, Normalizer)
  * **Data Visualization:** `matplotlib`, `seaborn`
  * **Mapping:** `folium`

## Data Sources

The analysis relies on three time-series datasets:

  * `Accelerometer.csv`: Raw accelerometer readings (x, y, z axes).
  * `Gravity.csv`: Gravity sensor readings (x, y, z axes).
  * `Location.csv`: GPS data, including latitude, longitude, speed, and altitude.

## How to Run

1.  **Clone the Repository:**

    ```bash
    git clone https://github.com/your-username/road-condition-assessment.git
    cd road-condition-assessment
    ```

2.  **Install Dependencies:**
    It is recommended to create a virtual environment. Install the required libraries using:

    ```bash
    pip install pandas numpy scikit-learn matplotlib seaborn folium jupyterlab
    ```

3.  **Prepare Data:**
    Place the `Accelerometer.csv`, `Gravity.csv`, and `Location.csv` files in the root directory of the project.

4.  **Run the Analysis:**
    Launch the Jupyter Notebook:

    ```bash
    jupyter lab ex.ipynb
    ```

    Execute the cells in the notebook sequentially.

5.  **View the Results:**
    After running the notebook, an interactive map named `road_conditions_map.html` will be generated in the project directory. Open this file in your web browser to explore the results.

## Future Work

  * **Hyperparameter Tuning:** Experiment with the number of clusters (`k`) in the K-Means algorithm.
  * **Advanced Models:** Explore other clustering algorithms like DBSCAN, which can be more effective at handling noise and clusters of varying densities.
  * **Real-time Analysis:** Develop a mobile application that can perform this analysis in real-time to provide live feedback to drivers.
  * **Enrich Features:** Incorporate additional data sources, such as gyroscope data or weather information, to build a more robust model.
