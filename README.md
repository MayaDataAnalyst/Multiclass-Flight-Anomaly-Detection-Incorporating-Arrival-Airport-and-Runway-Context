# Multiclass-Flight-Anomaly-Detection-Incorporating-Arrival-Airport-and-Runway-Context
## **PROJECT SUMMARY**

**PROJECT BACKGROUND AND OBJECTIVES**

Despite the historically high safety performance of commercial aviation, the continued reduction of aviation accidents and safety-related incidents requires **proactive identification of flight anomalies and their associated risk factors**. Machine learning (ML) techniques have increasingly been applied to aviation safety analysis to identify complex patterns in flight data, improve anomaly detection, and support data-driven safety risk assessment.
However, many existing studies have primarily employed **binary classification approaches**, distinguishing between nominal (Normal Operation) and anomalous flights without explicitly differentiating among anomaly types. Although some studies, such as **Memarzadeh et al. (2021)**, have explored multiclass anomaly classification, relatively limited attention has been devoted for identifying distinct types of flight anomalies.

Accordingly, this project pursued two primary objectives. The first was to **develop and compare multiple machine learning models for multiclass flight anomaly classification** in commercial aviation, distinguishing among **Nominal, Speed High, Path High, and Flaps Late** flight conditions. The second objective was to **identify and assess the key flight characteristics and operational factors associated with different types of flight anomalies**, providing insights that could support proactive aviation safety management and risk identification.



**FLIGHT DATA:**

This project used the **NASA DASHlink Multi-Class Anomaly Classification Dataset**, publicly available through **[NASA DASHlink repository](https://c3.nasa.gov/dashlink/projects/85/)**. The dataset contains time-series flight trajectory data collected during the approach-to-landing phase, with flights categorized into four classes: **Nominal, Speed High, Path High, and Flaps Late**. Each flight consists of **160 time-step observations**, with each time step containing measurements of multiple flight parameters, such as airspeed, altitude, and other aircraft operational characteristics.

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

**XGBoost** was subsequently selected for **SHAP (SHapley Additive exPlanations) analysis** to evaluate the relative importance of the selected predictors and examine their contributions to model predictions across the four flight classes. Both global and class-specific SHAP analyses were conducted to assess the magnitude and direction of each feature's influence on the model output.

The **SHAP analysis** identified `airspeed_mean` as the most influential predictor, followed by `airspeed_std`, `tot_pressure_std`, and `altitude_std`. However, the effect of each predictor on class probability varied by class. For example, the **class-specific SHAP analysis** revealed higher mean airspeed (`airspeed_mean`) generally contributed positively to **Speed High** and **Flaps Late** predictions but negatively to **Nominal** predictions. Greater altitude variability (`altitude_std`), pitch-angle variability (`pitch_angle_std`), and flight-path deviations (`loc_dev_std` and `gl_slp_dev_mean`) were particularly influential in identifying **Path High** anomalies. For **Flaps Late**, higher mean airspeed and mean pitch angle (`pitch_angle_mean`) generally increased the class prediction scores, whereas higher airspeed variability (`airspeed_std`) contributed negatively.

---

**PROJECT CONTRIBUTIONS/MERIT:**

The primary contribution of this project is the incorporation of **arrival airport and runway information as contextual predictors** in a multiclass flight anomaly classification framework. Unlike previous similar projects that have primarily relied on flight-specific parameters, this study has integrated operational context to distinguish among four flight categories: **Nominal, Speed High, Path High, and Flaps Late**. The inclusion of these contextual features also improved predictive performance by approximately **??%**, highlighting their potential value for anomaly detection. **A similar improvement was observed in my earlier binary classification project**, where incorporating arrival airport and runway information enhanced the predictive performance of the classification models.

Another key contribution of this project is the development of a **multiclass flight anomaly classification framework**, extending beyond the binary classification approaches commonly employed in previous studies, where different anomaly types have often been grouped into a single category. Rather than grouping the target variable into nominal and anomalous flights, this study enables the identification of **three distinct anomaly types: Speed High, Path High, and Flaps Late**, alongside the Nominal class. Furthermore, the project provides a **systematic comparative evaluation of four machine learning algorithms and class imbalance-handling strategies** within a multiclass classification framework.

Collectively, these contributions support the development of a more **interpretable, context-aware multiclass flight anomaly classification framework**, capable of distinguishing among different anomalous flight behaviors rather than treating all anomalies as a single category.


---
---
---
---

## **Dataset Description**

### **Target Variable Distribution and Binary Classification**

| Class Index | Original Class | Count | Binary Outcome |
|:-----------:|--------------------|------:|----------------|
| 0 | Nominal | 89,663 | Normal Class |
| 1 | Speed High | 7,013 | Anomaly Class |
| 2 | Path High | 2,207 | Anomaly Class |
| 3 | Flaps Late Setting | 954 | Anomaly Class |
| **Total** | | **99,837** | |

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

