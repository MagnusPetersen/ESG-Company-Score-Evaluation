# ESG-Company-Score-Evaluation

This is my attempt ot estimated ESG scores, environment, social and governance, using a zero shot multi lable categorization using a pretrained S-BERT Model and see how these change over time for a comany or another entity. The model uses keywords corresponding to E, S and G to categorize articles as pertaining to the three categories. If the article is classified as belonging to one or more of the categories it is assigned a score based on the relative positivity of the article, which is in turn calculated by compairing the score of the lable "negative" and "postive". 

# Disclaimer

This is a personal project that i undertook because i had access to a large dataset of news articles for various companies. The data is private however since my score evalaution program only uses a pretrained model and is therefore applicable for similar data and has not been trained on private data. Feel free to use it as you please. 

# Theory

Sentence-BERT, is a modification of the pretrained BERT network that use siamese and triplet network structures to derive semantically meaningful sentence embeddings that can be compared using cosine-similarity [1]. However since S-BERT has been trained on Sequences is does porely when embedding words, making comparisons between lables and texts suboptimal. The workaround is to embedd the test using S-BERT and to ebedd the word using word2vec and mapping the word2vec vector to the S-BERT embedding using a trained transformation matrix. For more details you can check out this blog post [2].


# Sources

[1] Nils Reimers, Iryna Gurevych. Sentence-BERT: Sentence Embeddings using Siamese BERT-Networks.

[2] Zero-Shot Learning in Modern NLP https://joeddav.github.io/blog/2020/05/29/ZSL.html
