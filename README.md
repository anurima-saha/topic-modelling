# Topic Modelling 
## Indentifying major conspiracy theories from reddit text
This project uses unsupervised learning to group reddit text and identify major conspiracy theories using NLP, LDA, spacy, SVD, SBert embedding and HDSCAN.

## Data:
Shape - (17155, 2) <br>
Columns – date (in type datetime[ns]), text (in type object)<br>
Date Range - 01-01-2015 to 17-02-2023 <br>

## Process Flow:

#### Data Cleaning:
* Removing leading spaces
  >![image](https://github.com/user-attachments/assets/5fd5acac-90c4-4969-8956-047094962b1a)
* Removing emojis and any other component that is not a word or a number
  >![image](https://github.com/user-attachments/assets/593a9198-069a-43fc-84c5-24c3e037c6cc)

#### Term Extraction:
**Spacy** model "en_core_web_sm" has been used for term extract along with the Matcher feature it provides.
We are trying to detect a pattern that begins with an adjective or a noun followed by singular/ plural common nouns or proper nouns along with hypen.
From the extracted terms it is noticed that there are terms in text than do not contribute to our analysis like
>![image](https://github.com/user-attachments/assets/9ebc3d12-cbe7-4867-9e3e-bed3c4505236)
<br>So, we invoke a second round of Data Cleaning that removes words like
>![image](https://github.com/user-attachments/assets/dbb611c5-5bcf-45ca-a9ec-5319a9c4fed9)
<br>This is followed by term extraction using C values with theta = 100. The list of ten most common terms and 20 least common terms has been provided below
<br>![image](https://github.com/user-attachments/assets/5a0a3fd9-27cb-4f4b-8c4f-eff09bd2ec2a)
#### Tokenization:
We create tokens from text data using Spacy pipeline incorporating the terms as created above
<br>![image](https://github.com/user-attachments/assets/3f91874e-bbe1-4025-8ee2-e02456a3bff5)

## Topic Modelling:
We have used the LDA model from tomotopy with the following features
<br>![image](https://github.com/user-attachments/assets/2171ad25-90c6-47fc-b90a-50ab5d10ed87)
<br>The twenty topics are plotted on a 2D space as below:
<br>![image](https://github.com/user-attachments/assets/3b3bb731-7c1a-48e4-b0fc-a523a71bb07d)
<br> Looking at top 50 words from each topic we label them as shown in the table below
>![image](https://github.com/user-attachments/assets/aae2159f-8c20-4c93-97fd-e7c9e0ebc6a6)
![image](https://github.com/user-attachments/assets/217843fa-ad8d-44b0-8829-91d42a63d5d7)

## Clustering:
* The “text” data has been cleaned to remove emojis and unnecessary text and punctuation expect “.” as required for sentence tokenization.
* We have removed all data that has less than 5 words.
* We have removed data that begins with "your post" or “please contact” to remove reddit submission messages.

#### Sentence Splitter:
We have used sentence splitter from Spacy to take a batch of 5 sentences at a time from each post
![image](https://github.com/user-attachments/assets/f94c5567-f075-4eea-831d-8c394994b3a7)

#### Embedding:
###### SVD:
We have used TruncatedSVD to transform the data into a *20 dimensional vector*.
![image](https://github.com/user-attachments/assets/205777a5-e3ce-4729-bb75-0fcdecbcc09c)

###### Spacy:
We have used the "en_core_web_lg" from Spacy to transform the cleaned text into a *300-dimension vector*. Further dimension reduction has been done using TruncatedSVD to bring it down to 20 dimension.
![image](https://github.com/user-attachments/assets/1dbfa992-4cf7-4e3b-997f-4d297da55b27)

###### SBERT:
Dynamic embedding was done using SBERT sentence transformer from "all-mpnet-base-v2" to obtain a *768 dimension vector*.Further dimension reduction has been done using TruncatedSVD to bring it down to 20 dimensions.
![image](https://github.com/user-attachments/assets/076c205a-a88f-439a-ace1-ab236b237bbf)

###### Perplexity:
As seen above, with Perplexity=50, the SBERT model clearly produces better results as compared to LSA and Spacy. Thus, we tried to plot the vector projections with perplexity 100, 150 and 200, to compare results and select best model for clustering
![image](https://github.com/user-attachments/assets/3a4c8f26-5c87-4cee-b27e-5db45e0c47a3) 
<br>Perplexity of 200 seems to provide optimal results for the analysis

#### HDBSCAN
We have considered a cluster size of 20 and fitted the SBERT vectors.
![image](https://github.com/user-attachments/assets/42877263-0bb0-4982-8a8a-5cf362b927db)
Highlighted terms in each cluster after incorporating c-values gives us the following topics as focus
<br>![image](https://github.com/user-attachments/assets/02e11cd6-effa-4063-b6ff-31c78b7c4e8b)

## Results:

#### Dynamic Bokeh Plot
![image](https://github.com/user-attachments/assets/5b575bec-009d-4236-9749-a7b61a49fd4a)

For interactive feature and further details please refer to notebook and project reoprt. A few examples have been highlighted below.

1. Presidential Election Fraud
   <br>![image](https://github.com/user-attachments/assets/b2e55eaf-a7fb-403e-8ed5-13cbc2b6f891)
   
2. Evolution Theory vs Religious Beliefs
  <br>![image](https://github.com/user-attachments/assets/27531f4a-bda8-43bf-9e4a-f66a8b780bc7)

3. Zionists – Israel and Palestine Crisis
  <br>![image](https://github.com/user-attachments/assets/d3f4b8ec-b35e-4e41-af87-39153f227579)

4. Flat Earth
   <br>![image](https://github.com/user-attachments/assets/2804e4bd-ae42-4ab1-b083-18ec3e6fa7de)
 
5. Ukraine War
   <br>![image](https://github.com/user-attachments/assets/ca8ffea1-e16e-406c-b55b-4a2f1ebadd9b)

6. Anti – Maskers (COVID 19)
   <br>![image](https://github.com/user-attachments/assets/217dbd76-d953-4979-987e-9032670d74f3)

   

   












