# Multiclass-Flight-Anomaly-Detection-Incorporating-Arrival-Airport-and-Runway-Context
## **PROJECT SUMMARY**

**PROJECT BACKGROUND AND OBJECTIVES**

Despite the historically high safety performance of commercial aviation, the continued reduction of aviation accidents and safety-related incidents requires **proactive identification of flight anomalies and their associated risk factors**. Machine learning (ML) techniques have increasingly been applied to aviation safety analysis to identify complex patterns in flight data, improve anomaly detection, and support data-driven safety risk assessment.
However, many existing studies have primarily employed **binary classification approaches**, distinguishing between nominal (Normal Operation) and anomalous flights without explicitly differentiating among anomaly types. Although some studies, such as **Memarzadeh et al. (2021)**, have explored multiclass anomaly classification, relatively limited attention has been devoted for identifying distinct types of flight anomalies.

Accordingly, this project pursued two primary objectives. The first was to **develop and compare multiple machine learning models for multiclass flight anomaly classification** in commercial aviation, distinguishing among **Nominal, Speed High, Path High, and Flaps Late** flight conditions. The second objective was to **identify and assess the key flight characteristics and operational factors associated with different types of flight anomalies**, providing insights that could support proactive aviation safety management and risk identification.



**FLIGHT DATA:**

This project used the **NASA DASHlink Multi-Class Anomaly Classification Dataset**, publicly available through **[NASA DASHlink repository](https://c3.nasa.gov/dashlink/projects/85/)**. The dataset contains time-series flight trajectory data collected during the approach-to-landing phase, with flights categorized into four classes: **Nominal, Speed High, Path High, and Flaps Late**. Each flight consists of **160 time-step observations**, with each time step containing measurements of multiple flight parameters, such as airspeed, altitude, and other aircraft operational characteristics.

### **Target Variable Distribution and Classification**

| Class Index | Original Class | Count |
|:-----------:|--------------------|------:|
| 0 | Nominal | 89,663 |
| 1 | Speed High | 7,013 |
| 2 | Path High | 2,207 |
| 3 | Flaps Late Setting | 954 |
| **Total** | | **99,837** |

<br>

### **Reshaping Flight Data from a 3D Array into a 2D DataFrame:**
<img width="800" height="500" alt="3D-to-2D Data Conversion" src="https://github.com/user-attachments/assets/51725313-d176-4285-8339-8c649409f06a" />

Several statistical aggregation methods, including **mean and standard deviation, maximum, and median**, were evaluated to transform the time-step observations into flight-level features. Based on this comparison, **mean and standard deviation were selected** to represent the flight-level characteristics used in the classification models.
Exploratory data analysis revealed a **substantial class imbalance**, with three anomalous flights together accounting for approximately **10%** of the total observations, indicating that the target variable was **imbalanced**.


**MACHINE LEARNING MODEL DEVELOPMENT AND EVALUATION:**

Four supervised machine learning algorithms were built and compared to detect multicalss anomalous flights and contributing factors, including:  

- Balanced Random Forest (**BRF**)
- Extreme Gradient Boosting Classifier (**XGBoost**) *(with/without SMOTE resampling)*
- Light Gradient Boosting Machine (**LightGBM**) *(with/without SMOTE resampling)*
- Artificial Neural Networks (**NN**)  

The training set was used for model development and hyperparameter tuning, while the test set was reserved for final performance evaluation. Model performance was assessed using **Macro Precision, Macro Recall, Macro F1-score, and Macro Average Precision (AP)**. Given the substantial class imbalance, **macro-averaged metrics** were employed to assign equal importance to all four flight classes, regardless of their relative frequencies. Particular emphasis was placed on **Macro F1-score as the primary model-selection criterion**, as it balances precision and recall across classes and provides a more representative assessment of multiclass classification performance than overall accuracy. Additionally, **class-specific precision and recall** were examined to evaluate each model's ability to identify minority anomaly classes.


**MODELING RESULTS AND DISCUSSION:**

The comparative analysis showed that **XGBoost yielded the highest overall classification performance**, with a **Macro F1-score of 0.811**, closely followed by the **LightGBM** and **NN**, both with a Macro F1-score of **0.808**. **BRF** had lower Macro F1-scores of **0.570**, while it achieved the highest **Macro Recall (0.850)**, indicating greater sensitivity across the four flight classes.

<img width="1189" height="590" alt="Model GOF Measures (Multiclass)" src="https://github.com/user-attachments/assets/c3072cdc-3d8d-4393-8e7f-17514d5441dd" />


**XGBoost** was subsequently selected for **SHAP (SHapley Additive exPlanations) analysis** to evaluate the relative importance of the selected predictors and examine their contributions to model predictions across the four flight classes. Both global and class-specific SHAP analyses were conducted to assess the magnitude and direction of each feature's influence on the model output.

The **SHAP analysis** identified `airspeed_mean` as the most influential predictor, followed by `airspeed_std`, `tot_pressure_std`, and `altitude_std`. However, the effect of each predictor on class probability varied by class. For example, the **class-specific SHAP analysis** revealed higher mean airspeed (`airspeed_mean`) generally contributed positively to **Speed High** and **Flaps Late** predictions but negatively to **Nominal** predictions. Greater altitude variability (`altitude_std`), pitch-angle variability (`pitch_angle_std`), and flight-path deviations (`loc_dev_std` and `gl_slp_dev_mean`) were particularly influential in identifying **Path High** anomalies. For **Flaps Late**, higher mean airspeed and mean pitch angle (`pitch_angle_mean`) generally increased the class prediction scores, whereas higher airspeed variability (`airspeed_std`) contributed negatively.

<img width="989" height="390" alt="SHAP Stacked Barplot" src="https://github.com/user-attachments/assets/77ac5100-9495-4542-ae96-4cd2d628a977" />

---

<img width="1000" height="700" alt="SHAP Beeswarm Plot (Multiclass)" src="https://github.com/user-attachments/assets/186723b9-0442-4b79-a3c3-54d40ca9eef9" />

---

**PROJECT CONTRIBUTIONS/MERIT:**

The primary contribution of this project is the incorporation of **arrival airport and runway information as contextual predictors** in a multiclass flight anomaly classification framework. Unlike previous similar projects that have primarily relied on flight-specific parameters, this study has integrated operational context to distinguish among four flight categories: **Nominal, Speed High, Path High, and Flaps Late**. The inclusion of these contextual features also improved predictive performance by approximately **4%**, highlighting their potential value for anomaly detection. **A similar improvement was observed in my earlier binary classification project**, where incorporating arrival airport and runway information enhanced the predictive performance of the classification models.

<img width="1690" height="755" alt="Top Airports with Highest Anomaly Rates" src="https://github.com/user-attachments/assets/b69dbd24-e68a-443e-9925-9cea191a2a6a" />

---

<img width="1082" height="754" alt="Map" src="https://github.com/user-attachments/assets/8492890d-29e3-44ee-bb88-6c09ce206ecf" />


Another key contribution of this project is the development of a **multiclass flight anomaly classification framework**, extending beyond the binary classification approaches commonly employed in previous studies, where different anomaly types have often been grouped into a single category. Rather than grouping the target variable into nominal and anomalous flights, this study enables the identification of **three distinct anomaly types: Speed High, Path High, and Flaps Late**, alongside the Nominal class. Furthermore, the project provides a **systematic comparative evaluation of four machine learning algorithms and class imbalance-handling strategies** within a multiclass classification framework.

Collectively, these contributions support the development of a more **interpretable, context-aware multiclass flight anomaly classification framework**, capable of distinguishing among different anomalous flight behaviors rather than treating all anomalies as a single category.


---
---
---
---

## **Dataset Description**

### **Target Variable Distribution and Binary Classification**

| Class Index | Original Class | Count |
|:-----------:|--------------------|------:|
| 0 | Nominal | 89,663 |
| 1 | Speed High | 7,013 |
| 2 | Path High | 2,207 |
| 3 | Flaps Late Setting | 954 |
| **Total** | | **99,837** |


---
---


### **Flight Features Used for Anomaly Detection**
It should be noted that the flap position (`flap_pos`) variable and its derived features (mean and standard deviation) were excluded to prevent potential target leakage, as flap position is directly associated with the definition of the **late-flap anomaly**. An initial analysis showed that **`flap_pos_mean` had the strongest influence on the binary anomaly response**, likely due to its direct relationship with late-flap settings. Therefore, **`flap_pos` and its derived features were excluded from the final model development.**


| Variable | Description |
|:---------|:------------|
| flight_id | Unique identifier for each flight |
| aileron_lh_mean | Mean left aileron position (degrees) |
| aileron_lh_std | Standard deviation of left aileron position (degrees) |
| aileron_rh_mean | Mean right aileron position (degrees) |
| aileron_rh_std | Standard deviation of right aileron position (degrees) |
| aoa_mean | Mean corrected angle of attack (degrees) |
| aoa_std | Standard deviation of corrected angle of attack (degrees) |
| altitude_mean | Mean barometric corrected altitude (feet) |
| altitude_std | Standard deviation of barometric corrected altitude (feet) |
| airspeed_mean | Mean computed airspeed relative to the surrounding air (knots) |
| airspeed_std | Standard deviation of computed airspeed (knots) |
| sel_course_mean | Mean selected course for the aircraft/autopilot (degrees) |
| sel_course_std | Standard deviation of selected course (degrees) |
| drift_angle_mean | Mean angle between aircraft heading and aircraft track (degrees) |
| drift_angle_std | Standard deviation of the drift angle (degrees) |
| elevator_l_mean | Mean left elevator position used to control aircraft pitch (degrees) |
| elevator_l_std | Standard deviation of left elevator position (degrees) |
| gl_slp_dev_mean | Mean deviation from the desired glide slope during approach (%) |
| gl_slp_dev_std | Standard deviation of glide-slope deviation (%) |
| sel_heading_mean | Mean selected heading for the aircraft/autopilot (degrees) |
| sel_heading_std | Standard deviation of selected heading (degrees) |
| loc_dev_mean | Mean localizer deviation from the runway centerline during approach (%) |
| loc_dev_std | Standard deviation of localizer deviation (%) |
| core_speed_mean | Mean engine core speed, indicating engine/throttle operating level (%) |
| core_speed_std | Standard deviation of engine core speed (%) |
| tot_pressure_mean | Mean total pressure (millibar) |
| tot_pressure_std | Standard deviation of total pressure (millibar) |
| pitch_angle_mean | Mean angle between the aircraft longitudinal axis and the horizon (degrees) |
| pitch_angle_std | Standard deviation of aircraft pitch angle (degrees) |
| roll_angle_mean | Mean aircraft roll angle (degrees) |
| roll_angle_std | Standard deviation of aircraft roll angle (degrees) |
| rudder_pos_mean | Mean rudder position (degrees) |
| rudder_pos_std | Standard deviation of rudder position (degrees) |
| true_heading_mean | Mean aircraft true heading measured clockwise from true north (degrees) |
| true_heading_std | Standard deviation of true heading (degrees) |
| vert_accel_mean | Mean aircraft vertical acceleration (G) |
| vert_accel_std | Standard deviation of vertical acceleration (G) |
| wind_speed_mean | Mean wind speed (knots) |
| wind_speed_std | Standard deviation of wind speed (knots) |
| arrival_airport | Arrival airport associated with the flight |
| arrival_runway | Runway used or assigned for the aircraft's arrival |  

---

## **FLOWCHART OF THE MULTICLASS FLIGHT ANOMALY CLASSIFICATION FRAMEWORK**

<img width="800" height="1100" alt="Flowchart of the Multiclass Anomaly Classification Framework" src="https://github.com/user-attachments/assets/b42fe63e-7929-420a-b7e0-37f929c649ac" />

---

## **RECOMMENDATIONS FOR FUTURE WORK**

In the present study, approximately 160 time-stamped observations per flight were aggregated into flight-level features using the **mean and standard deviation**, potentially obscuring important temporal patterns and short-duration deviations. Future research could explore **unaggregated time-series data** using sequential deep-learning models, such as an **LSTM-based multiclass classifier** or an **LSTM autoencoder with a classification head**. Their performance could then be compared with the models developed here to determine whether capturing temporal dependencies improves the **detection and classification of flight anomalies**.

A second direction for future research is to incorporate additional **airport-specific operational characteristics** into the prediction models, such as airport traffic volume, number of runways, runway configuration, and weather conditions. These factors may influence the likelihood and characteristics of flight anomalies. In addition, incorporating **aircraft-specific characteristics**, such as aircraft model, aircraft age, and engine type, may further improve model performance, as different aircraft types may exhibit different approach dynamics, deceleration behavior, and flap-deployment procedures.

---

## **REFERENCES**

Campesato, O. (2023). Python 3 and Feature Engineering.

Galli, S. (2020). Python feature engineering cookbook. Packt Publishing.

Galli, S. (2021). Feature-engine: A Python package for feature engineering for machine learning. Journal of Open Source Software, 6(65), p.3642.

Lee, H., Li, G., Rai, A. and Chattopadhyay, A., 2020. Real-time anomaly detection framework using a support vector regression for the safety monitoring of commercial aircraft. Advanced Engineering Informatics, 44, p.101071.

Kim, M., Kho, S.Y. and Kim, D.K., 2017. Hierarchical ordered model for injury severity of pedestrian crashes in South Korea. Journal of safety research, 61, pp.33-40.

Masís, S., 2021. Interpretable machine learning with Python (pp. 183-209). Birmingham, UK: Packt Publishing.

Memarzadeh, M., Matthews, B. and Templin, T., 2022. Multiclass anomaly detection in flight data using semi-supervised explainable deep learning model. Journal of Aerospace Information Systems, 19(2), pp.83-97.

Raja, M.N.A., Abdoun, T. and El-Sekelly, W., 2026. Smart Ensemble Modeling for Multiclass Lateral‑Spreading Prediction. Applied Soft Computing, p.115163.

Shaheed, M.S., Gkritza, K., Carriquiry, A.L. and Hallmark, S.L., 2016. Analysis of occupant injury severity in winter weather crashes: A fully Bayesian multivariate approach. Analytic methods in accident research, 11, pp.33-47.

Zhang, T., Ning, Z., Fan, Y., Zidong, Z., Lei, J., Du, S. and He, S., 2026. Interpretable machine learning for multiclass trajectory prediction in cardiovascular-kidney-metabolic syndrome stage: development and external validation. Frontiers in Endocrinology, 17, p.1907821.

---


## **Getting Started: Reproducing the Analysis**

The analysis was conducted using **Python** and the **NASA DASHlink Multi-Class Anomaly Classification Dataset**. The following steps outline how to reproduce the data preprocessing, feature engineering, model training, and evaluation procedures for the four flight classes: **Nominal, Speed High, Path High, and Flaps Late**.

### **1. Requirements**

The project uses Python and the following main libraries:

- NumPy
- Pandas
- Matplotlib
- Seaborn
- Scikit-learn
- Imbalanced-learn
- XGBoost
- LightGBM
- TensorFlow / Keras
- SHAP
- BorutaPy
- Plotly
- Airportsdata

### **2. Dataset**

The dataset is publicly available through the **[NASA DASHlink repository](https://c3.nasa.gov/dashlink/projects/85/)**.

The analysis uses two primary input files:

- `DASHlink_full_fourclass_raw_comp.npz` — Flight trajectory data and anomaly labels.
- `DASHlink_full_fourclass_raw_meta.csv` — Flight metadata, including arrival airport and runway information.

The dataset contains **99,837 flights**, each consisting of approximately **160 time-stamped observations** during the approach-to-landing phase.

Download the required files and place them in the working directory.

### **3. Running the Analysis**

1. Download or clone this GitHub repository.
2. Install the required Python libraries.
3. Download the NASA DASHlink dataset and update the file paths in the Jupyter Notebook.
4. Run the data preprocessing and feature engineering procedures, including:
   - Aggregation of flight trajectories using mean and standard deviation.
   - Encoding of arrival airport and runway information.
   - Feature scaling and Boruta-based feature selection.
5. Split the data into training and testing datasets.
6. Develop and optimize four classification models:
   - **Balanced Random Forest (BRF)**
   - **XGBoost**
   - **Light Gradient Boosting Machine (LightGBM)**
   - **Artificial Neural Network (ANN)**
7. Evaluate class imbalance handling strategies, including **class weighting, balanced sampling, and SMOTE resampling**, as applicable to each model.
8. Evaluate model performance on the held-out test dataset.
9. Conduct **global and class-specific SHAP analyses** to interpret the final XGBoost model.

### **4. Model Evaluation and Interpretation**

Model performance was evaluated using **macro-averaged Precision, Recall, F1-score, and Average Precision (AP)**, with **Macro F1-score** serving as the primary model-selection criterion due to class imbalance.

Among the evaluated models, **XGBoost achieved the highest Macro F1-score (0.811)**, followed closely by **LightGBM and ANN (both 0.808)**. Balanced Random Forest achieved the highest Macro Recall (**0.850**).

The final **XGBoost model** was interpreted using **SHAP (SHapley Additive exPlanations)** to identify influential predictors and examine their class-specific contributions. Particular attention was given to **airspeed characteristics, altitude variability, pitch-angle behavior, flight-path deviations, and arrival airport/runway context** in distinguishing among the four flight classes.
