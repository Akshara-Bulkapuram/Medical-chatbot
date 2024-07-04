# Medical-FAQ-chatbot

# Building a Medical FAQ chatbot using SBERT model. Dataset used was MEDQUAD

## short description
We have built this as our NLP course project. The Medquad dataset contains 47k Question-Context-Answer Mapping. we utilised an Sentence transformer BERT model and used it on our data to fetch the Most Accurate answer from the database given a Query. It works by creating sentence embedding for the Query question and checks the similarity for All the Questions in the database and get the answer of the most similar question. Elasticsearch based indexing is used for optimizing the database fetching. later we extended our project by utiliseing ROBERTA and MEDBERT sentence transformer models. Elasticsearch was used for indexing and faster retrieval.

For Questions that aren't present in the database (similarity<30%) we used BERT to pass the context obtained from webscraping and getting the answer and displaying it to the user. This QA pair is then stored in a seperate database, it can be added to main database after manual check.

## detailed methodology

A. Data Collection and Preprocessing

For training purpose we are using MedQuAD which comprises a dataset containing 47,457 pairs of medical questions and answers, sourced from 12 National Institutes of Health (NIH) websites, including but not limited to MedlinePlus Health Topics, and cancer.gov, niddk.nih.gov. This extensive compilation encompasses a diverse range of 37 question types, spanning categories such as diseases , diagnostic tests, Treatments and all linked to various medical entities including drugs and Side Effect. The testing phase involves the meticulous collection of 2479 judged Question-Expected Answer pairs.This dataset is curated to ensure its representativeness across various medical domains.

B. Models

1) BERT (Bidirectional Encoder Representations from Transformers): BERT’s specialty lies in its bidirectional approach, allowing it to capture context from both preceding and following words in a sequence. This bidirectional contextuality is integral for understanding the nuanced language often found in medical queries, where the correct interpretation often depends on the entire context. Its architecture includes multiple layers of attention mechanisms and feedforward networks.

2) SBERT (Sentence-BERT): Sentence-BERT (SBERT) is an innovative extension of the BERT architecture, designed specifically to generate semantically meaningful sentence embeddings. In the fig-1,the SBERT model takes two sentences(Entity A and Entity B) as input and passes them through a BERT encoder.The BERT encoder outputs a vector representation for each sentence. The SBERT model then computes the cosine similarity between the two vector representations. The cosine similarity is a measure of how similar two vectors are. A higher cosine similarity indicates that the two vectors are more similar. The cosine similarity score is then used to train the SBERT model using a Siamese network architecture wherein two identical neural networks share the same parameters. The Siamese network is trained to utilizes a contrastive loss function, such as the triplet loss, to minimize the distance between the two vector representations for similar sentences and maximize the distance between the two vector representations for dissimilar sentences. Once the SBERT model is trained, it can be used to generate sentence embeddings for new sentences by passing them through the BERT encoder.

  ![image](https://github.com/Akshara-Bulkapuram/Medical-FAQ-chatbot/assets/94600166/d9144277-63b1-463a-b0e2-fcec714645ae)

   
3) RoBERTa (Robustly optimized BERT approach): RoBERTa builds upon BERT’s foundation with a focus on robust optimization. Its specialty lies in addressing some of BERT’s limitations through modifications such as removing the next sentence prediction objective and training with larger mini-batches. This optimization makes RoBERTa particularly effective for tasks where robust language understanding is paramount, as is the case in medical question-answering. Its architecture retains the transformer model’s strengths but incorporates enhancements for improved overall performance. RoBERTa’s performance often surpasses that of BERT, especially in scenarios where thorough language comprehension is crucial.

4) MedBERT: MedBERT’s specialty lies in its domainspecific adaptations, specifically designed or fine-tuned for medical applications. Its pretrained on medical domain dataset and hence this model was chosen.

C. Similarity Thresholding

The QA MedBot incorporates a similarity metric to assess the relevance of user queries to the dataset. This metric calculates the similarity between the user’s input and questions in the dataset. we chose a threshold of 0.7 is set to filter questions, ensuring that only sufficiently similar questions proceed for further analysis. This step is crucial for maintaining the precision of the QA system.

D. Web Scraping

In scenarios where the calculated similarity falls below the established threshold, the QA MedBot initiates web scraping. The top 10 search results related to the user’s query are scraped to extract additional information. This external information serves to enrich the dataset and enhance the MedBot’s knowledge base. The scraped data undergoes preprocessing to align with the QA system’s requirements.

E. Answer Retrieval
for faster retrieval we have used elasticsearch engine. elasticsearch creates indexes of questions in the database and their embeddings. when a query question is input the engine searches for the most similar sentence embedding in the indexes to the sentence embedding of query question. after it finds such an index it retrieves the answer present at that index as the response to user’s query.

## RESULTS

![image](https://github.com/Akshara-Bulkapuram/Medical-FAQ-chatbot/assets/94600166/9229dc56-2543-4525-b373-f4f8fc115f11)
![image](https://github.com/Akshara-Bulkapuram/Medical-FAQ-chatbot/assets/94600166/72a22ce2-914b-4f87-883c-f6ceb456d8e2)
![image](https://github.com/Akshara-Bulkapuram/Medical-FAQ-chatbot/assets/94600166/1cd1b562-bcbe-4ef7-9f49-d49180ebcb7b)
![image](https://github.com/Akshara-Bulkapuram/Medical-FAQ-chatbot/assets/94600166/a577ce75-bfcb-4e51-b50f-00d552c63751)

## Note
The most important part about this approach of chatbot based on data retrieval is that its only as good as its data. so if this was to be made as a general all purpose chatbot in medical field, then vast amount of data would be needed and there might be inconstency in the data. but instead I would suggest an approach like hospital based. where the users can get general medical information through the faq database generated by the hospital like why causes a disease? how to prevent etc. in many cases we all have experienced googling about a medical condition and causes for it after a doctor appointment. This is because not all information can be properly obtained through a doctor appointment. so a simple faq chatbot like this made from a trusted set of question- answer information can help people visiting that hospital

