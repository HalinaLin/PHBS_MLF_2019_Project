# Detection of the Malicious Website's URL
## Group Members
Name | Github ID | Student ID 
:-: | :-------------------------------: | :-:
[Lei HU](https://github.com/huleipku)     |     huleipku     |     1901212585    
[Jinze HE](https://github.com/Hejinzefinance)     |     Hejinzefinance     |     1901212582    
[Yixin ZHAO](https://github.com/Zhaoyixin9705)     |     Zhaoyixin9705     |     1901212681    
[Aiyu CAO](https://github.com/caoxiaolong0521)     |     caoxiaolong0521     |     1801212821    

##  The Goal of Our Project
In order to **detect the security of a URL** (i.e. whether the website is dangerous to visit), we try to construct some reasonable features from the `malicious_urls.csv` and `benign_urls.csv` data sets, and use this to train our machine learning detection model.

##  Description of Raw Dataset
* `malicious_urls.csv` and `benign_urls.csv` are malicious and benign data sets, which containing 5000 data respectively.
* `top1m_rank.csv` contains the top 1 million URLs which are often used in people's daily life.
* `data_ulrs.csv` is the training data set after we construct and extract features from the raw datasets

* malicious urls examples：
<br> http://www.businesspage.ecuaradionline.com/9feaf7f8354ad68ba40e29d70cd05405
<br> http://businesspage.ecuaradionline.com
<br> http://www.lotto109.com/follow-up/eb6e30faeebb16feaf07ae96e6e2e821

* benign urls examples：
<br>www.api-global.netflix.com
<br>www.google.com
<br>www.microsoft.com

* top1 million urls examples：
<br> www.apple.com
<br> www.live.com
<br> www.googleapis.com

* training dataset examples:
<br> http://www.businesspage.ecuaradionline.com/9feaf7f8354ad68ba40e29d70cd05405/|4.603980434631428|5|0|69|0|0|19|0|1|1
<br> http://businesspage.ecuaradionline.com/|3.9056390622295662|3|0|32|0|0|0|0|1|1
<br> http://www.lotto109.com/follow-up/eb6e30faeebb16feaf07ae96e6e2e821/|4.229085753935212|6|0|60|0|0|17|0|1|1

* Data source:
<br> https://openphish.com/feed.txt
<br> https://ransomwaretracker.abuse.ch/blocklist/
<br> https://www.phishtank.com/

## Preprocessing
* In this part, the preprocessing means that we want to **remove the prefix** like `http://` or `https://` from the URLs. The reason is that the prefix is not helpful to judge the website, or even affect the calculation of the features.
* In the code, we define a function `parse_url` to remove the prefix. If you want to see the source code, please turn to [Part-1 Create_dataset_for_training_model.ipynb](https://github.com/caoxiaolong0521/PHBS_MLF_2019_Project/blob/master/Part-1%20Create_dataset_for_training_model.ipynb).

## Feature Selection
Feature name | Explanations about the feature 
:-: | -
`Entropy` | Entropy was originally a concept proposed in the field of physics, which is used to *measure the degree of chaos in a system*. Then, Shannon borrowed this concept and proposed the information entropy. And many researches have shown that **malicious URLs often have a higher information entropy**. And the class `Entropy` is to calculate the information entropy.
`bag_of_words` | Research also shows that **malicious URLs usually contains more words of different categories**, so *how many different kinds of words* that have appeared in the URL can also be an effective feature. And `bag_of_words` is to calculate this feature. 
`contains_IP` | *Whether a URL contains an IP address* is also a powerful indicator. So we need to check the occurence of an IP address within a URL since the **benign URL will not contain IP** in most cases.
`url_length` | It is intuitive that **malicious URLs can often be very long** in comparison to benign URLs. For example, the official website of [Baidu](https://www.baidu.com/), [GitHub](https://github.com/), [Google](www.google.com) are all relatively short.
`special_chars` | Sometimes **malicious URLs contain a larger number of special characters**, like ';','%','!','&',':', etc. So we introduce the feature `special_chars` to reflect. 
`num_digits` | Researches also show that **malicious URLs usually contain more numbers**. So the number of digits is also a good feature.
`suspicious_strings` |  A higher number of suspicious strings would more possibly indicate a malicious URL. So we introduce `suspicious_strings` to describe the feature.
`popularity` | If a website is more popular, it means more people are willing to visit, which reflects the low chance or possibility to be malicious. So the websites contained within the top 1 million URLs dataset are not likely to be malicious.

## Feature Calculation
* After selecting the feature to use, we established a class called `URLFeatures` to calculate the value of the features. 
* The picture below shows the structure of the class. The class `URLFeatures` contains 9 function members (8 functions for calculating, 1 final function for incorporating the URL and its corresponding features into a list). 
![image](https://raw.githubusercontent.com/caoxiaolong0521/PHBS_MLF_2019_Project/master/images/Structure.jpg)
* Based on `parse_url` and `URLFeatures`, we defined the function `extract_features` to combine the preprocessing part and the calculation of features into one step.
* At last, we define the function `create_dataset` to calculate the 8 features of our selected data and save them into `data_urls.csv` (*it will take approximately 2-3 hours*).
* The source code is in [Part-1 Create_dataset_for_training_model.ipynb](https://github.com/caoxiaolong0521/PHBS_MLF_2019_Project/blob/master/Part-1%20Create_dataset_for_training_model.ipynb).

## Model Training
* After we obtain the training data (`data_urls.csv`), we need to train the model next.
* The model we chose is the **random forest**. And we define the function `train_model` to train the classifier on the dataset we obtained before.
* The source code of training the model is in [Part-2 Train_model.ipynb](https://github.com/caoxiaolong0521/PHBS_MLF_2019_Project/blob/master/Part-2%20Train_model.ipynb).

## Model Evaluation
* In order to evaluate the performance of the model, we define three functions `plot_ROC_CURVE`, `plot_confusion_matrix`, `get_learning_curve` to plot the ROC curve, confusion matrix and learning curve.
* The source code is in [Part-2 Train_model.ipynb](https://github.com/caoxiaolong0521/PHBS_MLF_2019_Project/blob/master/Part-2%20Train_model.ipynb). And the figures of the results are shown below.
![image](https://github.com/caoxiaolong0521/PHBS_MLF_2019_Project/blob/master/images/ROC.jpg)
![image](https://github.com/caoxiaolong0521/PHBS_MLF_2019_Project/blob/master/images/confusion_matrix.jpg)
![image](https://github.com/caoxiaolong0521/PHBS_MLF_2019_Project/blob/master/images/learning_curve.jpg)

<br> `The following sections can be used to determine whether a newly entered URL is malicious or not:`
<br> get_url_info(): Get URL information function extracts features from a user supplied URL. The function extracts all features similarly to extract_features() and saves the extracted features in the form of a dictionary. 
<br> check_valid_url(): Check valid URL function checks whether or not the input URL to classify is in a valid format.
<br> classify_url(): Classify URL function passes in the input URL and classifies it as malicious or benign.

we have try some other algorithms we learned in class,like svm or lr,however,the result is not acceptable for us, we finally choose to use the RandomForest to train the classifier in the train_model() function.

## Model evaluation 
Due to the suitable feature selection,our URL prediction model has obtained relatively good indicators with Cross Validation Score:  92.6 % and F1 Score:  92.72 %




## appendix
In the process of the project, we actually found another very powerful feature, named Safebrowsing, which has greatly improved the performance of our results.
<br>The Google Safebrowse API will check if Google classes the URLs as safe. Any URLs classed as not safe may be malicious. Although blacklists and resources such as Google safebrowsing cannot predict malicious URLs, the appearance of a URL in these lists is indeed a very powerful feature. However, because of some network failures, we finally found that we could not call this API of Google Cloud Platform when running again.what is really a pity!
