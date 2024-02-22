---
layout: post
title:  "Prototyping the Political Bias A.I"
date:   2024-01-07 15:08:06 +0000
categories: Prototyping
author: Jordan Tallon
---


# Introduction
When coming up with the idea for our third year project, we decided to create an app that aggregates news feeds from multiples sources into a one stop location. To increase the scope of the project and the self learning aspect, I proposed the idea of integrating machine learning features into the platform.

We decided to go with a feature that will allow users to run a political bias detection model that classifies any detected political bias in a given article. We were drawn to this concept as it addresses a real world concern (the increasing polarization in online media) and it provides an excellent opportunity to bootstrap our understanding of Machine Learning, a field that is rapidly gaining demand and importance in today's technological landscape.

The 'Political Bias Detection' has two main components to it:
1. A web scraper that will retrieve the contents of user's article to feed into a model for analysis
2. The machine learning model itself that will analyse the given input and output a political bias label associated (if any) to the given input. e.g. ('Left', 'Center', 'Right')
To tackle this challenge, we decided to split up the work for the two main components. The first component, the web scraper, was to be completed by my partner Hephzibah Victor Bode-Favours. The second component, the machine learning model, was to be completed by myself, Jordan Tallon.

In this blog I will outline my initial dive into machine learning. My goal at this stage wasn't to develop a fully realized 'complete' model, only to dip my toes into the 'ocean' of machine learning. I work best when I get some practical experience alongside my theoretical learning, so starting with a prototype really helped me develop a sense of direction for how I am going to tackle the challenge.

# Initial Goals
* Understand Basic Concepts.
* Get practical experience with machine learning.
* Familiarize myself with datasets and data handling.
* Understand model training and evaluation.
* Gain confidence with fulfilling the machine learning requirements of the UniFeed project.

# The Machine Learning Prototype

## Dataset
I made use of the [AllSides](https://huggingface.co/datasets/valurank/PoliticalBias_AllSides_Txt) dataset which encompasses ~20,000 articles labelled: left, right, or center. The articles were scraped from the AllSides website, which presents curated articles from each political alignment for different topics.

## Methods
In developing the prototype for the Political Bias Detection model, I learned a combination of Python libraries and machine learning techniques. Most of my learning was through the HuggingFace community and YouTube. I did not follow any particular 'tutorials', I learned through finding solutions to individual problems I encountered and researching what steps I need to take for a complete training process. Here's a breakdown of the methods I employed:
1. **Environment and Setup**
   - I used **Google Colab** for development. The platform is a standard for both hobbyist and professional ML projects and granted me cheap access to GPUS/TPUS for training.
   - I Installed essential libraries into my Colab file, particularly from **HuggingFace** (datasets, transformers) and **PyTorch** (accelerate, torch, evaluate).
2. **Dataset Preparation**
   - I used HuggingFace's `datasets` library to load in the dataset from "JordanTallon/political_bias" (a fork I created of the original AllSides dataset where I can freely modify it). 
   - I converted the dataset to a **Pandas** DataFrame and split it into training (80%) and testing (20%) sets, I made sure that the stratification was based on the 'label' column so each class was equally represented.
3. **Data Processing and Tokenization**
   - I converted the string  labels to integer indices (0, 1, 2) for compatibility with the machine learning framework.
   - I utilized the `AutoTokenizer` from Hugging Face's `transformers` library with the pre-trained "distilbert-base-uncased" model for tokenization.
   - I created  a tokenization function and applied it to the dataset in batches and padded or truncated the articles to a maximum length of 512 tokens.
4. **Model Evaluation and Metrics**
   - Conducted model evaluation to obtain metrics such as accuracy, precision, recall, and F1 score.
   - `accuracy_score` and `precision_recall_fscore_support` from Scikit-learn were particularly important here for metric computation.
5. **Model Saving**
   - I saved the trained model and tokenizer to a directory. This is so that I can use the prototype as a test for integrating a PyTorch model into the Django Project

# Supervisor Feedback
I took my prototype and findings to a supervisor meeting where I received great feedback and direction. Of note are: my lack of dataset analysis, and unjustified conclusions. These two particular pieces of feedback were important because they were relevant to one of the main issues I had during the creation of the prototype. 

DistilBERT has a token limit of 512, which is roughly ~350 words using the OpenAI Tokenizer for conversion. There are articles in the dataset that have more words than the 350 limit. Initially I truncated the input data during tokenization so that only the first 512 tokens will be considered and the rest discarded. I did not like this approach because so much data was being lost. So I decided to break up each input into chunks of 512 tokens where an article with roughly ~1400 words would become 4 chunks of 512 tokens. This did not necessarily work bad, but the decision I made was based on an unjustified conclusion that all 'chunks' of the article represent the overall bias of the article equally. I do not necessarily know if this is true, nor do I know if I took the correct approach for handling the chunks if it is true.

It was also requested that I analyse the data further, I reflected on the meeting with the following questions, both from myself and my supervisor: What is the mean, median, range of the article text contents? What if only ~5% of my articles require chunking (over 512 tokens long), do I even need to address this issue? What other chunking strategies are there (from quick Google searches I have already came across Sliding Window which looks promising for this problem). 

Sliding Window essentially keeps the context of its neighbours inside each chunk through overlapping. So the start of the second chunk of text, would start with the ending of the first chunk of text. This sounds like an interesting solution to look into, and will be my first step after analysing the data.

At this point, I am very happy now with a sense of direction to further improve the Political Bias Detection model.

# Conclusion
I was quite post-hoc and unstructured whilst learning how to create the prototype model. This lead to mistakes that I feel I can easily avoid now with the knowledge gained from creating the prototype. I did not realize going in just how important and defining the dataset selection and pre-processing stages are. I am very happy with how much I learned from the prototype and feel much more confident going forward with the project.

For the final model at a later date, I plan to be much more structured, and go much further into detail than this blog for the prototype. I will most likely include the breakdown as a README in the GitLab repo folder containing the final model. I plan to break down all the different steps I took, how I managed the dataset, reproducibility, training, evaluation, etc.

