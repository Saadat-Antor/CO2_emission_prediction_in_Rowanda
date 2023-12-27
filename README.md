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

   The "train" dataset is as follows:
   ![Screenshot_3](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/acbd61dc-009a-427e-8c81-32c270d7303a)

**2. Handling Missing & Duplicate Data:**

   The dataset had a lot of missing values, especially in the "UvAerosolLayerHeight" group of columns(almost 99%). So it was imperative to remove all the entries.
   ![Missing Data_bar_chart](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/eb864ce9-6380-47b8-b1be-b67d72c60398)

   There were null values but no duplicate entries. I filled those null values using pandas fillna() function with the strategy "mean". Now there are no missing values to the entire dataset.
   ![imputation_fillna](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/af8f5ef7-9d4c-4fdb-a07f-c0b682647b81)

**3. Handling Outliers:**   

   After plotting a box plot for the `emission` feature, which is the target variable, it seems we can see that there are outliers.
   ![Screenshot_4](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/c076ce87-6117-43db-afad-04da29daeb57)

   After carefully looking at the values, these values were taken on the same location (-2.079, 29.321) and we can see a pattern for all three years. In each week coming, the values starts to increase and usually    crosses the 3000 threshold. Therefore, I believe that we should not consider these values to be outliers. Maybe that location has excessive amount of emissions.

   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/ead7daec-2bae-4a4e-b2e0-2c5726a6e597)

**4. Thorough EDA (on low-level):**

   Plotting a histogram with `emission` feature revealed that it was right-skewed, rather than normally distributed. One can think that using log function or other methods the problem could be solved. But as the competition would use RMSE mehod for scoring, the prediction score could be affected badly.
   ![Screenshot_5](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/e78f19c0-a3f1-453e-a7b0-6d2032fa2ad1)

   The above plot used the mean emission per location and after grouping all the values by location an interesting thing caught my eye. There are several locations where the emission is always zero.
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/f1376508-b82e-4ec8-af27-5b103eca4410)

   Apparently, there are fifteen locations where the emission is zero. It is safe to assume that the values are so near zero that they are written as zeros. Additionally, in theory it is possible for a specific      location to have zero carbon emissions, especially if the activities in that location do not involve the combustion of fossil fuels or other processes that release carbon dioxide (CO2) into the atmosphere.

   Now, to understand better I am going to plot the data in geological coordinates.
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/81a2d12f-5c6a-4d46-b9e4-bcc6dc630be4)

   The emissions are color-coded as `dark-blue = high emission`; `green = low emission`. We see that the emissions are clustered:

      * Points in the west (Democratic Republic of the Congo) all have low emissions.
      * The two red points with the highest emissions are both near Lac Kivu.

   This is an interesting find. Let's investigate this further.
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/e20b5a88-5002-41d4-a04d-dab11699767c)

   Now, we know that the year 2020 affected everything, even the reading of CO2 emission. Let's find out with a plot:
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/0d551dd3-7bad-45e3-8582-cdff9746f18c)

   The figure above displays the CO2 emission by date. We can see that the second quarter of the year 2020 shows some weird pattern which is not similar with 2019 and 2021. Presumably this pattern occured due to     corona outbreak over the year 2020. We should deal with it later because it may introduce overfitting in our model.

   There were strong seasonal patterns could be seen:
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/2174a201-892b-4b4d-b23f-9d9bf7a2e9df)

   We can see that in year 2020 because of the epidemic there is sudden drop in emission (average per year). Moreover, from the perspective of weeks and months noticeable patterns can be seen. In the week number     14 to 16 the emission significantly increased. Same as for the week number 39 to 43. Additionally, the average emission throughout the year has minimal fluctuation except month no 5 and 10 or May and October.

**5. Feature Engineering:**

   To understand the relation between the target variable and non-target variables I plotted a heatmap with top 20 correlated variables:
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/c33e939e-03e1-45ce-94e6-948950c7d933)

   The satellite measurements are not effective due to significant noise and lack of relevance to the target variable. That was why I had omitted all the features except a few.

   Moreover, to reduce the effect of Covid I created a `date` column and plotted emission by date. It looked promising:
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/22a7aa3f-598d-4957-8e66-910b249b6f20)

   I had also created some features that indicated cyclicity because of the plot below:
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/cbd1ec46-8626-4ada-9a59-775e97580e80)

   The plot above shows how the entries for one variable were taken. We can see a pattern i.e. cyclical that shows us that our data has cyclically measured data. Therefore, emmissions have a cyclic pattern - This will be helpful to our model. With more research and domain knowledge generate useful features that can improve your model performance.

   In all the cases, but `year`, the features should be split into two parts: sinus and cosine, to reflect cyclicity, e.g. the 1st January is near the 31st December. Moreover, `cluster` features were also       created. These all would help improve the prediction score.

**6. Preprocessing:**

   In this section, I got all the reliable features and created a data frame that went through a pipeline that used `SimpleImputer()`, `QuantileTransformer()`, and `StandardScaler`.
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/8a9aa64d-d1a2-4485-b383-0dbf1bc640c8)

**7. Model Selection and Implementation:**

   Here we are using three ML models namely `Linear Regression`, `Random Forest`, and `Gradient Boosting`. I compared the performance of each model using a scoring method called "Mean Squared Error".
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/aebbfc44-3908-40d5-9125-74eaa3a95127)

   The `Random Forest` model used here scored better than the other two.

**8. Hyperparameter Tuning:**

   I had used some hyper parameters called `n_estimators`, and `max_depth` and tuned them with different values. But unfortunately, the scores were worse than before:
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/a694ba14-51ac-4cf2-90bf-1760acc9e2e5)

   Therefore, I decided not to use any hyper-parameters.
   
**9. Submission:**

   All the pre-processing I had done before I had done with the "test" dataset and created a data frame:
   ![image](https://github.com/Saadat-Antor/CO2_emission_prediction_in_Rowanda/assets/76962594/3c6f0d18-105a-4c3d-984b-6639fd01122b)

   After that I had trained and fit the `RandomForestRegressor` model again and predicted with the new values. Finally, it was submission-ready. But unfortunately I can't submit the file because the last date of submission was four months ago. But I had fun doing all the data science stuffs and I hope I did pretty good at it.

## Final Thoughts

This project is actually based on a Supervised Machine Learning problem, Regression problem specifically, where I have to create a model predicting the emissions of CO2 emission. Anyone could have thought that `Linear Regression` model can be the best choice but the scores said something different.

My take on this is that no model is perfect for these type of predictions. There could be any number of models good for this project, but eventually I have to chose one of them. Moreover, data pre-processing and analysis are the key here. Without these steps, my model would've done worse. Machine Learning or in broad term Data Science is actually based on two terms, **Handling Data** and **Building Best Model**. My plan for the future is to be better in these two fields.
