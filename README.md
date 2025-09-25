# Detecting the anomalous activity of a ship's engine
**Darya Yarparvar** | 18 September 2025
______________________________________________________________

## Introduction
### Problem statement
Engine breakdowns cause costly downtime, safety risks, excess fuel consumption, and missed deliveries, all of which reduce profits and harm customer satisfaction. The business needs a reliable way to detect anomalies early to prevent these breakdowns.
### Data
The dataset (Devabrat, 2022) contains 19535 rows with six features: engine rpm, lubrication oil pressure, fuel pressure, coolant pressure, lubrication oil temperature, and coolant temperature.
## Methods
Stakeholders expect anomalies to make up 1–5% of the data. This range will serve as the key measure for evaluating and accepting the results of any anomaly detection method. Figure 1 shows an overview of the approach used. 

<img width="660" height="423" alt="image" src="https://github.com/user-attachments/assets/f791a3ec-9eef-4379-a835-40e80ec93a2c" />

Figure 1. An overview of the approach

### Data preparation
The data was checked for missing values and duplicated rows. None existed.
The descriptive statistics of the data was investigated during exploratory analysis. The clean data was used for further steps including IQR and Isolation Forest anomaly detection methods.
### Anomaly detection
The three methods used for anomaly detection are summarised in Table 1.

<img width="703" height="407" alt="image" src="https://github.com/user-attachments/assets/3df2c9ad-9f7a-487c-8e0e-5dfb8bdaa4a6" />

Table 1. The methods used for anomaly detection

### Dimensionality reduction with PCA
Using PCA and reducing the dimensionality of the dataset to 2 principal components makes it possible to visualise the separation between normal points and anomalies in a 2D space. 

### Feature scaling
Data were standardised for OCSVM and PCA, which are sensitive to feature scales.


## Results
Each method identified 2.16% of the data as anomalies, satisfying the stakeholder’s expected range of 1–5% (Table 2).

<img width="709" height="751" alt="image" src="https://github.com/user-attachments/assets/b7dac6ac-5432-4646-a01e-69d3470ecf80" />

Table 2. The results of each anomaly detection method. 

### IQR method
Initial anomaly detection using the standard IQR method identified 23.73% of the data as outliers which was significantly higher than the expected 1-5% range. To refine this, a stricter definition was adopted: “solid outliers” were defined as data points where at least two features were simultaneously outside their normal ranges. With this rule, 2.16% of data points were identified as anomalies. Most anomalies (75.36%) came from just three feature pairs, while tri-feature anomalies were rare and none involved more than three features.
### One-class SVM and Isolation forest
Since we are working in an unsupervised setting with no true labels, we cannot measure model performance using recall or precision. Instead, we rely on the stakeholder’s criterion: if the proportion of detected anomalies falls within the expected range (1–5%), the model is considered acceptable.
### Comparing all used methods:
PCA projections into 2D show anomalies scattered inside and around the main cluster, with no clear separation (Fig. 2). Hyperparameter tuning did not improve separation. This suggests that anomalies are embedded within the normal operational range. OCSVM and IF anomalies appear more spread out, while IQR anomalies are more compact in the centre (Fig. 3). Most anomalies detected only by one method are also concentrated in the centre (Fig. 4).

<img width="788" height="699" alt="image" src="https://github.com/user-attachments/assets/43baad6f-65c6-4e21-8238-1ab2854d4da2" />

Figure 2. Detected anomalies projected into a 2D space after applying PCA (turquoise: normal data). The lack of a distinct cluster for anomalies suggests they are embedded within the normal operational range.

<img width="689" height="699" alt="image" src="https://github.com/user-attachments/assets/596cdeb5-0acd-4b1c-a292-f3d30d780a4f" />

Figure 3. IQR, OCSVM, and IF anomalies projected into a 2D space after applying PCA (Normal data is not shown)

<img width="788" height="699" alt="image" src="https://github.com/user-attachments/assets/97dad91d-7fb9-4c5d-932b-0d51d87641fd" />

Figure 4. Anomalies detected by one, two or three methods projected into a 2D space after applying PCA (Normal data is not shown)

By checking overlaps between different approaches, we can focus on samples that are more likely to be true anomalies. Only 113 (26.8% of detected anomalies) are common across all methods (Fig. 5).

<img width="510" height="425" alt="image" src="https://github.com/user-attachments/assets/d453d967-1e6b-4be7-97f6-ebdb78503ac2" />

Figure 5. The overlap in IQR, OCSVM and IF anomalies. Of the total 422 anomalies detected by each method, only 26.8% are common among all of the methods. The highest agreement: OCSVM and IF (58.1%) The lowest agreement: OCSVM and IQR (30.6%) The method with the lowest number of anomalies that were not agreed by any other methods: IF (26.8%) The highest number of anomalies that were not agreed by any other methods: IQR (54.3%). These findings were expected as the IQR is a univariate method while OCSVM and IF are multivariate methods.

Common anomalies detected by all three methods account for only 0.58% of the data, which is below the specified threshold. However, anomalies identified by two methods constitute 1.09% of the data. PCA visualisation again showed no clear pattern (Fig. 6). 

<img width="788" height="699" alt="image" src="https://github.com/user-attachments/assets/0b231033-dadc-4f4a-91d6-c6e094267f99" />

Figure 6. Anomalies detected by one, two or three methods projected into a 2D space after applying PCA (Normal data is not shown)

Figure 7 shows co-occurring features in anomalies detected by more than one method (1.09% of data).  These feature combinations should be experimentally investigated further, as they are more likely to contribute to anomalous behaviour. For example, high fuel pressure, low coolant pressure, and high lubrication oil temperature. These combinations largely overlap with the insights obtained from the IQR method (Table 3; Fig. 8). One advantage of the IQR approach is that it allows us to focus on practical combinations of features and assess whether specific combinations are meaningful, drawing on domain knowledge from ship engineering. However, a limitation is that this method cannot capture the overall multivariate effect of features. No anomalies were detected with more than three co-occurring features, which further highlights this restriction.

<img width="724" height="452" alt="image" src="https://github.com/user-attachments/assets/e2a282dd-30f0-4fe4-b046-a834ab042ad0" />

Figure 7. Anomalies detected by more than one method. blue: common among all three methods (0.58% of data); orange: common among two methods (1.09% of data)

<img width="570" height="534" alt="image" src="https://github.com/user-attachments/assets/a154211a-951f-4260-a8c5-a5ef92050305" />

Table 3. Thirteen unique feature combinations in IQR anomalies. Together, the top three pairs account for 75.36% of IQR anomalies. Tri-feature anomalies are rare; only 11 cases in total.


<img width="783" height="991" alt="image" src="https://github.com/user-attachments/assets/07684675-518e-474d-ba69-33992683de76" />

Figure 8. Thirteen unique feature combinations were involved in the 422 IQR anomalies. The three most frequent co-occurring features in anomalies are: [fuel pressure, oil temperature] (143 cases); [coolant pressure, oil temperature] (107 cases); [engine rpm, oil temperature] (68 cases). Together, these three pairs account for 75.36% of IQR anomalies. Tri-feature anomalies are rare. Only 7 cases of [fuel pressure, coolant pressure, oil temperature], and 2 cases of each of [engine rpm, oil temperature, coolant pressure] and [engine rpm, oil temperature, fuel pressure] combinations.

OCSVM and IF anomalies show a similar spread across each feature, while IQR anomalies are less similar (Fig. 9).
This is expected since OCSVM and IF are multivariate anomaly detection methods. They take into account the relationships between all features simultaneously. 
However, although we define solid outliers as points with more than one feature outside the normal range, IQR is fundamentally univariate. The “solid outliers” rule combines features, but the initial detection is still feature by feature. As a result, anomalies detected by IQR can differ from those found by multivariate methods and a point flagged in one or two features may not stand out in the combined feature space.

<img width="790" height="991" alt="image" src="https://github.com/user-attachments/assets/3ec92c71-3aa7-4bff-ade3-17f03f3c305a" />

Figure 9. The spread of IQR, OCSVM and IFanomalies  across each feature.

## Conclusions
All methods detected about 2.16% of anomalies, meeting the expected 1–5% range. The best approach for anomaly detection on this dataset is a hybrid strategy:  IQR highlighted key feature combinations, while OCSVM and Isolation Forest captured multivariate patterns across all features. Overlapping anomalies detected by multiple methods (1.09% of data) are most likely true anomalies. Focusing on these key features and combining statistical and multivariate approaches can help the company prevent engine breakdowns, reduce downtime, save fuel, and improve safety and customer satisfaction. 

## References:
Devabrat, M., 2022. _Predictive Maintenance on Ship's Main Engine using AI._ Available at: https://dx.doi.org/10.21227/g3za-v415. [Accessed 10 September 2025]




















          aWord count (main text only, excluding cover, tables, figures, captions, and references): 835 words
