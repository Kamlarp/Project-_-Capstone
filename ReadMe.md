## **TABLE OF CONTENT** 

- Context
- Problem Statement
- Data Roadmap
- Data Proof of Concept 
    - Scope of Work
    - Procedure 
    - Data Source & Dictionary 
    - Deliverables 
    - Limitation
    - Assumption
    - Key Success Metrics
    - Key Finding 
    - Recommendation 
- Possibilities
- References 


## **--- CONTEXT ---**

Alljit (https://alljit.com/) is a Thai mobile application designed to provide mental health support and resources. It offers a variety of features, including:
 - A safe space to talk about anything, including finances, work, family, love, and yourself.
 - A community of people who understand and support each other.
 - The ability to chat with trained administrators who can provide advice and support.
 - A library of articles and podcasts on mental health.
 - A preliminary self-assessment form to help you understand your mental health needs.

Alljit, in collaboration with social media platform, online forum, and mental health hospital. plans to build **the data product which help detect the suicidal tendency** using NLP to analyze th text data, alert, and provide the recommendation for both users and doctors in charges (if available).

## **--- PROBELM STATEMENT ---**

**Real-World Problem Statement**
Mental health issues especially suicide have reached alarming levels globally, as well as in Thailand; we need to proactive detect people who have to tendency of these problems to address these issues.

According to the World Health Organization (WHO), in 2020, suicide was the tenth leading cause of death worldwide, with an estimated 1.39 million deaths. 

According to the Department of Mental Health,In Thailand, suicide rates have increased by 19.6% from 2011 to 2021, with a suicide rate of 15.3 deaths per 100,000 population. 

According to National Institute of Mental Health (NIMH) in the United States, about 90% of people who die by suicide have a diagnosable mental disorder, with depression being the most common.There is notable correlation between suicide and depression.

How can we prevent suicide by proactively detecting those with tendencies and providing them with appropriate clinical solution?

**Product-Level Problem Statement**
How can we develop a data-driven solution that leverages social media interactions to detect signs of suicidal tendencies and offer timely assistance. This involves text tracking, natural language processing (NLP), and the creation of an alert and recommendation system colloborated with online media platform and clinical experts?

**Data-level Problem Statement**
How can we build an accurate model that can proactively identify individuals at suicidal risk in early stage?

## **--- DATA ROADMAP ---**

our data roadmap only focus on data part of the solution  but not cover the alert part and the assistance part for the users and doctors in charges. 

    1) Create the model to classify the post of Pantip online forum
        - we will focus only post with category tag "depression"
        - we will use the key word both in thai and english to manually label "suicide" and "not suicide" 
            - and the post is labeled "suicide", sub label will be added further for "active suicide" and "passive suicide"
    2) Adjust the model to classify all posts and comment in Pantip online forum 
    3) Adjust the model to classify posts & comments on social media that become our partners; the potential partners are such as 
        - twitter post and comments
        - facebook post and comments
        - instragram post and comments
        - tiktok post and comments
        - youtube post and comments
        - line voom post and comments
    4) Adjust the model to classify the distinct people instead of posts and comments.
    5) Regularly update and fine-tune the model based on feedback from mental health professionals and real-world user interactions.

## **--- DATA PROOF OF CONCEPT ---**

### Scope of Work

Our data proof of concept project will focus on the step 1 and part of step 3 in the data roadmap; it include the following steps

    1) Create the model to classify the posts of Pantip online forum
        - we will focus only post with category tag "depression"
        - we will use the key word both in thai and english to manually label "suicide" and "not suicide" 
            - and the post is labeled "suicide", sub label will be added further for "active suicide" and "passive suicide"
    2) use the trained model from Pantip online forum to classify the comments in Alljit Youtube channel.

### Data Proof of Concept Procedures

The data modeling procedure includes the following tasks.

    --- Prepare Pantip Data  ---

    1) Webscrape data from pantip online forum with tag "depression" - 24,000 post
    2) Filter the post with key words related to suicidal 
    3) label the post 
        - 0 : not have suicidal idea 
        - 1 : have sucide active suicidal idea 
        - 2 : have passive death wish, but not have active suicial idea
    4) create the dataframe including columns id, profile, label, url, title, text, tag, time
    5) preprocess data
            - create the categorical columns from date column:
                    "day_week" - day of week
                    "day_month" - day of month
                    "time_day" - time of day
                    "month_year" - month of year
                    "year" - year
            - create the categorical columns from text column and title column
                    "title_emoji" - emoji in title (yes/no)
                    "text_emoji" - emoij in text (yes/no)
            - create the categorical column from column tag
            - combine the column "post title" and "post text" to "all_text"
            - create the numercial columns from text column and title column:
                    "title_len" - length of title
                    "text_len" - length of text 
            - combine title columna and text column into all_text column
            - remove the following data from all_text column
                all english words
                all numerics
                all punctuations
                all urls
                all null, blank, and whitespace text
                all new line text
                all words which are specific to Pantip post:
            - use PyThaiNLP to do tokenization and to do stopword in "all_text" columns

    --- Analyse Pantip Data  ---

    6) Do the EDA 
        - Analyse proportion among 
            - 0 : not have suicidal idea 
            - 1 : have sucide active suicidal idea 
            - 2 : have passive death wish, but not have active suicial idea
        - Analyse histogram of time features including time of day, day of week, day of month, month of year, year
        - Analyse histogram of text numeric features including title length, text length
        - Analyse bar chart of text category features including title emoji, text emoji check 
        - Analyse histogram of tag features 

    7) Do the Topical Modeling
        - categorize the post into 4 groups including 
            - depression, including all observations
            - suicide ideation, including active and passive suicide ideation
            - active suicide ideation 
            - passive suicide ideation 
        - do the LDA model by determining 3 topics for each group
        - analyze the topics of each group

    --- Feature Engineer & Model Pantip Data: suicide vs non suicide ---

    8) Do the feature engineering
        - Define variable X (features) & variable y (target)
        - Split train and test data
        - Do one hot encoding categorical features
        - Vectorize text features
        - Scale of Numerical features
        - Concatenate all features
        - Do the SMOTE (Only Training Data).
    9) Do the  modeling & evalutaing 
        - logistic regression (use 30 sec)
        - decision tree (use 10 mins)
        - select the model best recall score on suicaidal 
    10) do the analysis from the model result
        - coefficient analysis 
        - error analysis

    --- Feature Engineer & Model Pantip Data: "active vs passive" ---

    11) repeat the step 8)-10) by focusing on label active suicide ideation vs passive suicide ideation

    --- Simplify Features & Models on Pantip Data: "suicide vs non suicide" for Youtube ---

    12) repeat the step 8)-10) by removing some features not available in Youtube comment data

    --- Simplify Features & Models on Pantip Data: "active vs passive" for Youtube ---

    13) repeat the step 11) by removing some features not available in Youtube comment data
    
    --- Feature Engineering Youtube data & Use the train model "suicide vs non suicide" on Youtube data ---

    14) use youtube data and use trained model from step 12

    --- Feature Engineering Youtube data & Use the train model "active vs passive" on Youtube data ---

    15) use youtube data and use trained model from step 13


### Data Sources and Dictionary

**"df_pantip_posts_suicide_labeled_processed.csv" is the data sets which we scraped from Pantip post and preprocessed them to be ready for one hot encoding, tokenization, and word vectorization before the modeling**


|name|explanation|example| 
|---|---|---|
|id| post index number generated for this project | 24057 |
|profile|user id| 7860906|
|label| labeled as active suicide ideation (1) vs none (0)| 1 |
|url| url of the post | https://pantip.com/topic/42367932 |
|title| title of the post | อยากจบชีวิตตัวเอง เพราะชีวิตเหมือนตายทั้งเป็น |
|text| detail of the post | ผิดไหม? ถ้าเราอยากจากโลกนี้ไปตลอดกาล... |
|tag| type of the post | ['ปัญหาชีวิต', 'สุขภาพจิต', 'โรคซึมเศร้า'] |
|time| publish time of the post | 11/27/2023  1:47:44 PM |
|len_title| number of characters of the post title | 10 |
|len_text| number of characters of the post detail | 100 |
|emoji_title| check yes (1) vs no (1) whether there are emojis in the post title | 0 |
|emoji_text| check yes (1) vs no (1) whether there are emojis in the post detail | 1 |
|day_week| the day of week from sunday to saturday| Saturday |
|day_month| the day of the month from 1st to 31st| 11 |
|month_year| the month of the year from January to December| November |
|year| the year in AD | 2023 | 
|time_day| the time bracket within a day from 0-3 to 21-24 | 12-15 |


**"df_youtube_comment_suicide_labeled_processed.csv" is the data sets which we scraped from comment of Alljit Youtube channel and preprocessed them to be ready for one hot encoding, tokenization, and word vectorization before the modeling.**

|name|explanation|example| 
|---|---|---|
|vdo id| vdo index number generated for this project | 24057 |
|title| title of the vdo | อยากจบชีวิตตัวเอง เพราะชีวิตเหมือนตายทั้งเป็น |
|comment| comment of vdo| ??? |
|label| labeled as active suicide ideation (1) vs none (0)| 1 |
|time| publish time of the comment | 11/27/2023  1:47:44 PM |
|len_comment| number of characters of the post title | 10 |
|emoji_comment| check yes (1) vs no (1) whether there are emojis in the comment | 0 |
|day_week| the day of week from sunday to saturday| Saturday |
|day_month| the day of the month from 1st to 31st| 11 |
|month_year| the month of the year from January to December| November |
|year| the year in AD | 2023 | 
|time_day| the time bracket within a day from 0-3 to 21-24 | 12-15 |

### Assumptions
    - People who wrote about their desire to kill themselves or leave the world on the online forum or social media have active suicide ideation
    - Modeling from Pantip online forum can be applied for other social media text data
    - We can contact those who wrote their desire to kill themselves or leave the world on the online forum or social media 
    - We can prevent the suicide attempt by detecting them earlier and offer the timely assistance 


### Deliverables

**Document**
- ReadMe.md 
- suicide ideation detection.ppt

**Code**
- pantip_post_id_scraping.jpynb
- pantip_post_detail_scraping.jpynb
- pantip_post_preprocessing_&_eda.jpynb
- pantip_post_model_train_label.jpynb
- pantip_post_model_train_sublabel.jpynb"
- pantip_post_model_train_simplified_label.jpynb
- pantip_post_model_train_simplified_sublabel.jpynb
- youtube_vdo_id_&_comment_scraping.jpynb
- youtube_vdo_comment_combining_&_labeling.jpynb
- youtube_vdo_comment_preprocessing_&_eda.jpynb
- youtube_vdo_comment_model_test_label.jpynb
- youtube_vdo_comment_model_test_sublabel.jpynb
- youtube_vdo_comment_scraping folder

**Model**
- pantip_post_model_train_simplified_label_lr.pkl
- pantip_post_model_train_simplified_label_vectorizer.pkl
- pantip_post_model_train_simplified_label_scaler.pkl
- pantip_post_model_train_simplified_sublabel_lr.pkl
- pantip_post_model_train_simplified_sublabel_vectorizer.pkl
- pantip_post_model_train_simplified_sublabel_scaler.pkl
- X_train_sm_label_columns.json
- X_train_sm_sublabel_columns.json

**Result**
- pantip_model_simplifeid_label_lr_classification_report.csv
- pantip_model_simplifeid_label_lr_confusion_matrix.csv
- pantip_model_simplifeid_label_dt_classification_report.csv
- pantip_model_simplifeid_label_dt_confusion_matrix.csv
- pantip_model_simplifeid_sublabel_lr_classification_report.csv
- pantip_model_simplifeid_sublabel_lr_confusion_matrix.csv
- pantip_model_simplifeid_sublabel_dt_classification_report.csv
- pantip_model_simplifeid_sublabel_dt_confusion_matrix.csv
 
**output - final**
this folder include labeled and processed data used for feature engineering 
- **df_pantip_posts_suicide_labeled.csv** : file contain the detail of each post and label (1 = suicide idea, 0 = no suicide idea) 
- **df_pantip_posts_suicide_labeled_processed.csv** : file contain the detail of each post, and label, and processed data including len of title and text, emoji of title and text, time of day, day of week, day of month, month, year; this file is zip. so you need to unzip and put the unzipped file in this folder
- **df_youtube_comment_suicide_labeled.csv** : file containing comments of each vdo of youtube alljit channel with label
- **df_youtube_comment_suicide_labeled_processed.csv** : file containing comments of each vdo of youtube alljit channel with label and processed data including len of text, emoji check of text, time of day, day of week, day of month, month, year

**output - wip**
This folder include work in process document which we use to analyse and label data for output - final folder. i do the work in this folde with excel only. no python code related to all these file.
- alljit_youtube_vdo_document_labeling.csv
- Pantip_Post_Suicie_Labeling.csv
- df_pantip_depression_post_url.csv
- df_pantip_depression_post.csv
- df_pantip_suical_post_text_only.csv
- df_pantip_suical_post_text.csv
- df_pantip_suical_post_title.csv
- df_youtube_vdo.csv
- df_youtube_vdo_comment.csv

### Key Success Metrics

When building a classification model for sensitive topics like suicide ideation, it is critical to prioritize metrics that reflect the cost of misclassification. In this context, the most severe consequence would be failing to identify an individual who is at risk. Therefore, the following metrics are typically prioritized:

**Recall (Sensitivity)**: This is the most important metric for your use case because it measures the proportion of actual positives (cases with suicide ideation) that were correctly identified by the model. A high recall indicates that the model is effective at identifying most individuals who are at risk, which is essential for providing timely intervention.

### Finding & Model Result 

**Pantip Post Finding used for training model**
- for pantip post with depression tagging which we use to develop trained model 
        - total 24,000 posts
        - proportion between suicidal (95%) and non suicidal (5%)is very imbalanced
        - proportion between active suicidal (55%) and passive suicidal (45%)is quite balanced
- tag features, text features, and time feature has notably impact on the classification model
- barchart analysis of tag features reveal that the issue of family and teen problem is more for suicidal vs non suicidal
- topical modeling on text feature reveal that the negative relatiionship with family, friend, fiancd', especially with mom and dad, lead to negative feeling about selft which lead suicidal ideation 
 - barchart analysis of time feature also reveal that suicaidal post is notably more 
        - during 18.00 - 21.00 for time of the day 
        - during tuesday for time of the week 
        - during the recent year of data collection which is 2023 
 - barchart analysis of time feature also reveal that suicaidal post is notably less 
        - during 6.00 - 9.00 for time of the day 
        - during friday for time of the week **
        - during the earliest year of data collection which is 2016 and 2017 

**Pantip Post Modeling modeling Result**
- we use 2 model including logistic regression and decision tree
- logistic regression has better recall score for both case of 
    1) suicial ideation (1) vs non suicidal ideation model (0) - 90%
    2) active suicidal (1)vs passive suicidal model (0) - 100%
- however the score is quite low for macro average score across areas as the figure shown below 

suicial ideation (1) vs non suicidal ideation model
                precision  recall    accuracy
 macro avg       0.56      0.79      0.52

active suicidal (1)vs passive suicidal model (0)
                precision  recall    accuracy
macro avg       0.82      0.55      0.47

- these score contradiction reveal that the model predict very well on positive case (label 1) but predict very poorly on the negative class (0)

**Alljit YouTube Comment Finding used for testing model**
- for pantip post with depression tagging which we use to develop trained model 
        - total 12,000 posts
        - proportion between suicidal (95%) and non suicidal (5%)is very imbalanced
        - proportion between active suicidal (40) and passive suicidal (60%)is quite balanced

**Alljit YouTube Comment modeling Result**
- we use 2 model including logistic regression and decision tree
- logistic regression has better recall score for both case of 
    1) suicial ideation (1) vs non suicidal ideation model (0) - 76%
    2) active suicidal (1)vs passive suicidal model (0) - 75%
- from the score difference between trained model and tested model, we can see high level of overfitting

-  compared to the macro average score across areas as the figure shown below, the score is quite close

suicial ideation (1) vs non suicidal ideation model
                precision  recall    accuracy
 macro avg       0.96      0.94      0.95

active suicidal (1)vs passive suicidal model (0)
                precision  recall    accuracy
macro avg       0.76      0.76      0.76

 - the closed score reveal that the model predicttion  on positive case (label 1) and n the negative class (0) are quite close 



### Data, Model, Follow-up Action Limitations
    - Even though, we can detect the people with active suicide ideation earlier, we may not be able to contact them because there is no contact data; the project will be more possible if we have support from government, mental health hospital, telecom operators, online forum, and social media which have contact data and can provide legal permission for us to contact suicide attempt person in Thailand. 
    - There will be people undetected because they may not express their active suicide ideation on online forum or social media.
    - Data modeling from Pantip online forum might not be applicable for other social media text data; Therefore, the model need to be finetuned from platform to platform
    - Thai conversational language is fluid and evolving fast. the term related to active suicide ideation may changes over time. Therefore, the model need to be updated on routine basis
    - We only use 2 models which are logistic regression and decision tree
    - We have not tuned the model with grid search yet.
    - The data is label not by certified psychiatrist.
    - We only use the data with depression tagging. We can use more data from all tagging related mental health.

### Recommendation 

**for Pantip**
- Alljit can improve the model and support Pantip to classify of its subscribers, and detect those with mental health issues and suicidal tendency by 
    - incresae the trained data by including all post related to mental health
    - be more detailed in labeling with key words and getting advice from certified psychiatrist
    - use more models including deep learning model
    - try grid search to tune model
    - change the post classification to people classification 
- After that we can scale up the project to increase all post within Pantip
- After we have acceptable model for using with all pantip subscribers, We can also add the questionnaire to make the classification more accurate.
- Then we can provide mental health services to its subscribers by partnering with hospital to target and personalize the services to those with mental health issues

**for other social media partner**
 - Alljit and social media partners can do the same things as with pantips but it is highly recommended to finetune the model based on the data of social media to prevent overfitting issues if we use the pure model from pantip

**for government**
Alljit, social media partners, and government can do the nationwide report, detection, alert, and recommendation system for mental health and suicidal risk 

Just like what we did with Covid, we can proactively do the same for mental health and suicidal risk

And ee will be able to allocate the resources to province that need more mental health professional and support 


## **--- REFERENCE ---**

**Active Suicide Ideation vs Passive Suicide Ideation (or Passive Death Wish)**

The machine learning model breakdown the labeling into Active Suicide Ideation vs Passive Suicide Ideation (or Passive Death Wish)

Passive suicidal ideation is when someone thinks about or wishes for death without actually planning to end their life. Passive suicidal ideation is distinctly different from active suicidal ideation. Active suicidal ideation is when you think about specific ways in which to end your own life. Put simply, active suicidal ideation involves a plan to harm yourself, whereas passive suicidal ideation does not come with any plan for suicide.

For more detailed information, you can refer to the original publications below.
https://ridgeviewhospital.net/passively-suicidal-a-warning-sign-you-should-never-ignore/

**The Integrated Motivational-Volitional (IMV) model of suicidal behavior**

The finding from machine learning model is analyzed with reference to the The Integrated Motivational-Volitional (IMV) model of suicidal behavior which is a comprehensive framework that explains the development of suicidal ideation and the progression to suicidal attempts. It was developed by Rory O'Connor and colleagues and integrates key factors influencing suicidal behavior.

The IMV model is structured around three distinct phases:

    Pre-Motivational Phase: This phase involves biopsychosocial vulnerability factors and triggering events that set the stage for the development of suicidal thoughts. These factors can include early life adversity, poverty, and traits like socially prescribed perfectionism.

    Motivational Phase: This phase describes how and why suicidal ideation may emerge, with a focus on the concepts of defeat and entrapment as primary drivers of suicidal thoughts.

    Volitional Phase: This marks the transition from suicidal thoughts to behaviors. This phase is characterized by volitional moderators, which include factors like access to means of suicide, past suicidal behavior, and physical pain sensitivity.

For more detailed information, you can refer to the original publications below.
https://pubmed.ncbi.nlm.nih.gov/30012735/


