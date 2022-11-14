# Overview
The RPG Stack Exchange website offers a place for game masters and players of tabletop role-playing games to ask and answer questions. It is a great 
resource for those that enjoy tabletop RPGs and the website supports questions about many different RPG systems such as Dungeons \& Dragons, World of Darkness, 
Pathfinder and Apocalypse World. We decided to use this website for our search system because we are both familiar with the many aspects of tabletop gaming 
and it’s generally a topic we enjoy reading about. The RPG world is filled with many different types of players, from players that enjoy more roleplay-based 
games, to players that focus on minimizing and maximizing their character’s skills and attributes. Regardless of playstyle, the RPG Stack Exchange offers 
many interesting and engaging discussions for all types of players to read and enjoy.

For this project, we used the PyTerrier library to run several different models as retrieval systems.

# Usage
The large file consisting of the posts of the RPG Stack Exchange website is not included. You have to manually download it and drop the Posts.xml file into a Google Drive folder named IRCourse_Project_Files. Once you have done this, the Colab file should be functional.

Unlike our first project, these search systems do not take manually inputted queries. Instead, the search functions take a Dataframe of queries with a unique query id for each. There are five different search models:
* TF-IDF
* BM25
* DirichletLM
* Fusion RRF
* Query Expansion

# TF-IDF
One of the first models we used was the TF-IDF model, or the term frequency-inverse document frequency model. The TF-IDF model focuses on giving terms a weight depending 
on how important that term is to a document in the collection. TF-IDF is very popular in information retrieval. The TF-IDF value of a term will increase proportionally to the number of times a word appears in the document, whilst the value will decrease as more documents with the word exist in the collection. This is because as a term becomes more common among documents, that means it's more likely to be a common word like "the", which does not have much meaning to it when compared to something like "dragon".

# BM25
Another model we used for our query retrievals was the BM25 model, short for Best Matching 25. BM25 is a bag-of-words retrieval method. That is, bag-of-words implies that the ordering of words does not matter; instead, documents are reduced to unique terms and the number of times that they appear. BM25 is similar to TF-IDF in the sense that it considers both term frequency and inverse document frequency, however BM25 applies both document and query length normalizations. Thus, if say a document has the word "dragon" appear 200 times, it is not 100 times more relevant than a document that has "dragon" twice; normalizations will limit the contribution of term frequency to the score. To do this, we use numerical constants that are different from collection to collection, and are used as tuning parameters.

# DirichletLM
Our third and self-chosen model to use was the Dirichlet Language Model. As per the project guidelines, we used the Dirichlet model to rerank the top 100 results of BM25 for each query. The DirichletLM model focuses on smoothing by making more frequent and common words have less probability, and instead giving it to less likely terms. Thus, we are more likely to see documents we would not have before. irichletLM will generate a language model for each document, giving each unique term a probability. To create a score for a document given a query, DirichletLM will take each term's probability and multiply them together, thus giving a probability score of seeing those terms together.

# Rank Fusion
To get the rank fusion of TF-IDF and BM25 models we used RRF(Reciprocal Rank Fusion). RRF takes the reciprocal for the ranking of a certain document for each system and sums them up. This means that it will rank based on solely the rank that a document got in the systems. If the document ranked highly in the input system, then it will rank highly in the fusion.

# Sequential Dependence (Query Expansion)
Sequential dependence is a method of query expansion that takes into the account pairs of terms in your query instead of the terms individually. Pyterriers version of this takes your query, splits the terms up into sequential pairs, takes the whole query, and uses those sets of terms as the query. For example:

Original query: how to cook potatoes

Resulting query after sequential dependence: (how to) (to cook) (cook potatoes) (how to cook potatoes)

These sets of terms are then used in a ranking system. We used dirichlet as ours.
