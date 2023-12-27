This project was posted in Kaggle(Playground Series - Season 3, Episode 20) and required to predict CO2 emission prediction in Rwanda. The dataset was provided by Sentinel-5P satellite observations and was already split into train and test files. The data was taken from 497 locations and have 79023 entries with 76 columns. The project includes the steps below:
- Exploring the dataset on the high-level
- Handling Missing & Duplicate Data
- Handling Outliers
- Thorough EDA (on low-level)
- Feature Engineering
- Preprocessing
- Model Selection and implementation
- Hyperparameter tuning
- Submission

Now, I will summarize all the steps and findings below:

**1. Exploring the dataset on the high-level:**
   
The "train" dataset had a lot of missing values, especially in the "UvAerosolLayerHeight" group of columns(almost 99%). So it was imperative to remove all the entries.
![Missing Data_bar_chart](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/eb864ce9-6380-47b8-b1be-b67d72c60398)

There were null values but no duplicate entries. I filled those null values using pandas fillna() function with the strategy "mean". Now there are no missing values to the entire dataset.
![imputation_fillna](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/af8f5ef7-9d4c-4fdb-a07f-c0b682647b81)

