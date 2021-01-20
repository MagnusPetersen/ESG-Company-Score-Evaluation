## ESG-Company-Score-Evaluation

This is my attempt of estimating ESG scores, a scoring system for evaluating environment, social and governance impact of an entity. The scoring is done using a zero shot multi label categorization using a pretrained S-BERT Model. The model uses keywords corresponding to E, S and G in order to categorize articles as belonging to the three categories. If the article is classified as belonging to one or more of the categories it is assigned a score based on the relative positivity of the article, which is in turn calculated by comparing the score of the label "negative" and "positive". All scores are standardized based on randomly sampled articles.

![alt text](./example.png)

# Disclaimer

This is a personal project that I undertook because I had access to a large dataset of news articles about various companies. The data is private, however since my score evaluation program only uses a pretrained model and is therefore applicable for similar data and has not been trained on private data it is usable by anyone who want to use it. Feel free to use it as you please, but you will have to use your own data.

# Scoring Principle

For each of the three sub scores, E, S and G, four labels where chosen that encapsulate the sub score. For example, for E the words “Pollution”, “Environment”, “Climate Change” and “Emissions” were chosen. If the sum of the label scores exceeds a chosen threshold the text is classified as “E”. This is then repeated for “S” and “G” with their own labels. Next a positivity score is calculated by comparing the values of the label “negative” and “positive”. The entity is the given a sub score based on the positivity in each sub score category that is has been classified as. This is then repeated for all articles over time to track the ESG scores over time using a rolling average of the individual article scores. 

# Zero shot multi label classification Theory 

Sentence-BERT, is a modification of the pretrained BERT network that use Siamese and triplet network structures to derive semantically meaningful sentence embeddings that can be compared using cosine-similarity [1]. However, since S-BERT has been trained on Sequences is does poorly when embedding words, making comparisons between labels and texts suboptimal. The workaround is to embed the test using S-BERT and to embed the word using word2vec and mapping the word2vec vector to the S-BERT embedding using a trained transformation matrix. For more details you can check out this blog post [2].

# Code Explanation

The initial part of the code is for hyperparameter tuning. In this case the parameters are the cut-off values for the sub scores, E, S and G, for an article to be counted as being about either E, S and/or G. For example, "Evil Inc seen harming baby pandas" might get a sum of all environment keywords ("environment", "pollution", "climate change", "nature") of 0.9 which would lead to a classification as environment, if the cut-off is below 0.9. The confidence finder function is then used to pick cut-offs and evaluate a few articles and see if the chosen cut-offs are appropriate. 

After choosing an appropriate cut-off for all three categories scalers will be calculated to scale the three individual scores to a normal distribution. This is done by sampling several articles at random and calculating ESG scores to find a mean and standard deviation in order to find a transformation to standardize the data. Lastly the ESG Scores will be calculated for the given number of companies.

# Data Explanation

The data I had available, that you will have to replace, consists of a large corpus of articles, the publishing dates of these articles and the company names, that are covered in each article. You will have to replace the following files with your own data.

tiles.csv: A file with all the article headlines.

body.csv: A file with the full articles.

comp_id_list.pkl: A list of lists where each list is filled with the indices of articles that feature a specific company.

comp_name_list.pkl: A list of the company names. 

# Functions

confidence_finder: This function outputs the estimated number of articles classified as "E", "S" or "G" as well as outputting sample articles that were classified, as a file to evaluate the classification.

norm: This function collects the distribution of the esg score in order to standardize them.

esg_calculator: Calculates the ESG Scores of several companies over time or cumulative.

esg_plot: This function plots the ESG Data for timeseries data.

esg_gauss_plot: This function plots the cumulative ESG scores.

# Sources

[1] Nils Reimers, Iryna Gurevych. Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks.

[2] Zero-Shot Learning in Modern NLP https://joeddav.github.io/blog/2020/05/29/ZSL.html
