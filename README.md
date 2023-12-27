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



