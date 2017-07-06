# Gender Classification using SIFT Encoded Vector Dataset

### Problem statement
Given the training dataset of SIFT encoded images of people, build a classifier to predict the gender. This was a part of one of the offline (in person) hackathons I participated. The scoring metric was AUC.

### Dataset
Training, Validation and Test datasets were provided. Predictions were to be done on the test set and submitted for evaluation. The Training and Validation dataset is a csv file with Id, SIFT encoded vector representation of images (256 columns) and Label. The dataset has about 24K rows and 258 columns. The dataset was skewed with almost 80% of the rows for one class (Label 1) and 20% for the other (Label 0).

### Approach
* Did train split into train and cross validation for evaluation. Didn't use Validation set right in the begining. 
* Balance out the dataset:
    * This can be done in one of the two ways...
        * Remove class 1 rows to bring down the row count close to that of class 2. This approach can be chosen if training size/hardware limitation is a problem and unless this won't reduce dataset drastically.
        * Oversample for class 2 rows by adding copy of the randomly chosen rows repeatedly to bring the count close to that of class 1. Shuffle records in class 2, to prevent any kind of uniformity introduced while adding the rows repeatedly.
* Dimensionality Reduction based on Feature importances:
    * In any dataset, even before thinking of feature engineering, we should first identify the importance of all the features. Not all of them are useful. And limiting to the most important features always makes a lot of difference. 
    * I used PCA and SelectKBest. Both gave almost similar results.
    * I did this by choosing _n_ best features using SelectKBest() iteratively for _n_ ranging between 30 to 256, with stepping interval of 5. This gave me the best _n_ based on the highest AUC score.

#### Possible Improvements
* Use stratified sampling for cross validation
* Use PCA and SelectKBest together to find the best features, stepping at every feature instead of having bigger steps
* Perform grid search to tune hyper parameters better
* Can be ensembled with other types of classifiers like SVM, ANN and use voting to improve prediction accuracy

### Quick info about SIFT
SIFT stands for Scale-invariant feature transform. SIFT is used to encode images into vectors. This representation is different from raw flattened out pixels that are widely used. The main shortcoming of the raw pixel representation of images is that, the pixel data doesn't preserve/represent similarities across images. For instance, even the slight change in the image's aspect ratio or angle of view changes the pixels drastically, even when the image is same.

#### References
* [Scale-invariant feature transform - Wikipedia](https://en.wikipedia.org/wiki/Scale-invariant_feature_transform) 
