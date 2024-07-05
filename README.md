# Machine Learning for MOF Adsorption Refrigeration Cycle Optimization

This project aims to optimize the performance of Metal-Organic Frameworks (MOFs) used in adsorption refrigeration cycles. The goal is to maximize the Coefficient of Performance (COP) by predicting and selecting the most suitable MOFs using Bayesian Optimization with Gaussian Processes.

## Project Overview

### 1. Data Preparation

The dataset consists of various properties of MOFs, including:
- **Uptake Adsorption (uptake_ads)**: Amount of adsorption in the adsorption phase.
- **Uptake Desorption (uptake_des)**: Amount of adsorption in the desorption phase.
- **Heat Adsorption (heat_ads)**: Heat involved in the adsorption phase.
- **Heat Desorption (heat_des)**: Heat involved in the desorption phase.

The data was preprocessed as follows:
- **Removing Missing Values**: Rows with any missing values (NaNs) were dropped.
- **Calculating Δq**: The difference between uptake adsorption and desorption (Δq = uptake_ads - uptake_des).
- **Filtering Negative Δq**: Rows with negative Δq values were removed to ensure physical consistency.
- **Calculating Average ΔH_ads**: The average of heat adsorption and desorption values was computed.

### 2. COP Calculation

The Coefficient of Performance (COP) was calculated using the following formula:

\[ \text{COP}_R = \frac{\Delta H_{\text{vap, Tev}} \cdot \Delta q}{(M_w \cdot C_{\text{sorbent, p}} \cdot (T_{\text{des}} - T1)) - (\Delta q \cdot \text{avg} \Delta H_{\text{ads}})} \]

where:
- \(\Delta H_{\text{vap, Tev}}\): Heat of vaporization.
- \(M_w\): Molar mass.
- \(C_{\text{sorbent, p}}\): Specific heat capacity of the sorbent.
- \(T_{\text{des}}\): Desorption temperature.
- \(T1\): Initial temperature.
- \(\Delta q\): Difference in uptake adsorption and desorption.
- \(\text{avg} \Delta H_{\text{ads}}\): Average heat adsorption.

### 3. Feature Selection

Using a Random Forest Regressor, feature importance was evaluated. Features with importance above a specified threshold were selected for the model:
- **Random Forest Regressor**: Trained to rank the importance of features.
- **Threshold Filtering**: Important features were filtered based on their calculated importance.

### 4. Bayesian Optimization with Gaussian Processes

Gaussian Processes were used to model the relationship between features and COP. Bayesian Optimization was employed to iteratively select MOFs that maximize the acquisition function:

- **Acquisition Functions**: Different strategies such as Expected Improvement (EI), Maximum Mean (`max y_hat`), and Maximum Uncertainty (`max sigma`) were used.
- **Optimization Loop**: Iteratively selected the most promising MOFs based on the acquisition function and updated the model with new data points.
- **Evaluation Metrics**: Exploration and exploitation terms were calculated to understand the balance between exploring new areas and exploiting known promising regions.

### 5. Training the Random Forest Regressor

The points returned from the Bayesian Optimization process were collected to train a Random Forest Regressor model:
- **Training Data**: The selected points from Bayesian Optimization.
- **Testing Data**: The remaining data points not selected during the optimization process.
- **Model Performance**: Achieved an accuracy of 75% on the test set using only 134 training data points.

### 6. Hyperparameter Tuning

Hyperparameter tuning was performed using Bayesian Optimization to find the best model parameters efficiently:
- **Number of Iterations**: The best parameters were found with only 30 iterations.
- **Parameter Optimization**: Ensured optimal model performance.

## Conclusion

The Bayesian Optimization framework using Gaussian Processes efficiently guides the search for optimal MOFs in the adsorption refrigeration cycle, maximizing the COP. The approach balances exploration of new areas in the feature space with exploitation of known promising regions, leading to a significant improvement in model accuracy and performance. The Random Forest Regressor trained on the selected data points achieved a high accuracy, demonstrating the effectiveness of the optimization process.

## Results

- **Optimized MOFs**: Indices of the selected MOFs during the optimization process.
- **Model Accuracy**: The Random Forest Regressor achieved an accuracy of 75% on the test set.
- **Efficiency**: Used only 134 data points for training and achieved significant accuracy.
- **Hyperparameter Tuning**: Efficiently found the best parameters using only 30 iterations.

COP_R = (ΔH_vap_Tev * Δq) / ((M_w * C_sorbent_p * (T_des - T1)) - (Δq * avg ΔH_ads))

