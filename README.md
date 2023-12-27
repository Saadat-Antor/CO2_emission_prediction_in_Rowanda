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

The train dataset had a lot of missing values, especially in the "UvAerosolLayerHeight" group of columns.
![Missing Data_bar_chart](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/eb864ce9-6380-47b8-b1be-b67d72c60398)
