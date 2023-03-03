# Medical-Transcript-Classification


# Business Understanding
**Stakeholders:** Doctors, Hospitals and Research Institutions

In today's healthcare environment, doctors are required to spend more time than ever on administrative tasks such as documentation and record-keeping. This can be a significant drain on their time and energy, taking away from their ability to focus on providing high-quality care to their patients. Doctors and hospitals are still adapting to Electronic Health Records (EHRs) which have helped immensely, although the burden of administrative work and documentation are still significant. Having an NLP (Natural Language Processing) model that can accurately classify medical documents by the type of specialist seen would be immensely helpful for doctors and hospitals for several reasons. First and foremost, it would streamline the process of organizing patient records and help medical professionals easily access the information they need. When a patient visits a specialist, they often receive specific tests, procedures, or medications that are unique to their condition. An NLP model that can classify medical documents by specialist type can make it easier for doctors to quickly access relevant information about a patient's diagnosis, treatment, and progress. This would save doctors and hospital staff time and effort, while also ensuring that patients receive the most appropriate care, more time with their doctor and save significant spending on administrative tasks. Additionally, having an NLP model to classify medical documents would also help with research efforts by making it easier to collect and analyze data on patients with specific medical conditions, which could ultimately lead to improved treatment and outcomes. 

<img width="1016" alt="Time_Spent" src="https://user-images.githubusercontent.com/113449546/222605507-416ab1ee-4b5f-4f75-9318-3b4014ae2760.png">



# Dataset
The dataset was downloaded from __[Kaggle](https://www.kaggle.com/datasets/tboyle10/medicaltranscriptions)__. Some categories in the dataset were combined to create a more effective model. For instance, Neurosurgery and Neurology were combined into one category, as well as Consult - History and Physical, and General Medicine. The categories of Discharge Summary and SOAP/Chart/Progress Notes were dropped from the dataset as they were deemed too broad to classify. The other specialties that were dropped were those that had fewer than 100 transcripts, as it was not enough data to create an effective classification model for them. 33 rows of the dataset contained no transcript at all and those were removed.

This ultimately left the dataset with 3,714 transcripts and the following counts for each.
| Specialty                                        | Number of Documents |
| -------------------------------------------------| --------------------|
| Surgery                                          | 1088                |
| Consult - History and Phy. / General Medicine    | 775                 |
| Cardiovascular / Pulmonary                       | 371                 |
| Orthopedic                                       | 355                 |
| Neurology / Neurosurgery                         | 317                 |
| Radiology                                        | 273                 |
| Gastroenterology                                 | 224                 |
| Urology                                          | 156                 |
| Obstetrics / Gynecology                          | 155                 |


# Preprocessing and Modeling
Four different approaches were used for creating the classification algorithm.
1. Tokenizing and lemmatizing the text with NLTK and using GridSearhCV along with 5 different types of models
2. Tokenizing and lemmatizing the text with Gensim and SpaCy, implementing an NMF topic model to create topic weights for each document, and using GridSearhCV along with 5 different types of models
3. Creating a custom Word2Vec model and using an LSTM Neural Network with the text preprocessed from the second approach
4. Using GloVe embeddings and an LSTM Neural Network with the text preprocessed from the second approach

The last two approaches were unsuccessful as there was not enough data for the LSTM model to distinguish the classes.

While the first two approaches displayed similar results, the second approach improved the accuracy, precision, recall and F1-scores by roughly 3-4%. The second approach involved both vectorizing the text as well as using the topic weights created from the NMF model as inputs into each model. 

# Evaluation and Conclusion
Ultimately, using Gensim and SpaCy for preprocessing, combined with NMF topic distributions and then a Logistic Regression ended up achieving the best results. 

### The NMF Topic Visualization
<img width="1195" alt="Screen Shot 2023-03-02 at 8 16 43 PM" src="https://user-images.githubusercontent.com/113449546/222607200-0e554a8f-3f63-48e1-903b-55193ca702fb.png">

### Pipeline Architecture
<img width="325" alt="Screen Shot 2023-03-02 at 8 17 50 PM" src="https://user-images.githubusercontent.com/113449546/222607298-f125afb6-6918-4aac-aef2-4667b5d3351f.png">

| Metric          | Value       |
| ----------------|-------------|
| Accuracy        |  56.80%     |
| Precision       |  59.45%     |
| Recall          |  56.80%     |
| F1-Score        |  55.48%     |

### Testing Confusion Matrix
![confusion_matrix](https://user-images.githubusercontent.com/113449546/222606911-a9515095-3a72-40fa-977e-e18dd9213569.png)

### Top 20 Features by Specialty
![model_features](https://user-images.githubusercontent.com/113449546/222605170-85e8b898-180c-4346-9e12-03adb7af408b.png)

While there is significant value in the idea behind the model, the results would need significant improvement to be practically implemented. The model had a clear issue in distinguishing Surgery from other categories, as surgery clearly could have overlap with many disciplines. It often predicted that a transcript was for a certain specialty, when it was actually a surgical visit. For instance, Orthopedics and Surgery was difficult for the model to differentiate, but this was not surprising as one usually does not see an Orthopedist unless they are getting some sort of surgery. Although, the model did do very well at determining when something was a general visit compared to other specialties. 

The model was able to pick up on many of the important features that one would think would distinguish these specialties from one another. In some cases, there were multiple different topics from the NMF model coming up for a certain specialty which could be one of the issues in the models predictions. For instance, the Cardiovascular category only had one topic in its important features, although the Surgery category had 3 different topics. One issue with the NMF model was that the 4th topic (nmf__topic3) appeared in over half of the specialties. In the end, the topic weights still improved the classifications overall.

# Next Steps
  > Acquire more data to not only include more specialties but to create a useful LSTM model
  
  > Find a dataset with better topic differentiation

  > Look into pre-trained vectors that contain specific medical terminology
