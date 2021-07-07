# Automatic extraction and comparison of literary characters

Author: Beatriz Borges

Supervisor: Professor Frédéric Kaplan

Digital Humanities Lab, Spring Semester 2021

<br/><br/>

## Introduction
As readers of a literary work, we possess an incredibly vast collection of background knowledge and common sense facts that help us understand both the action taking place and the characters involved in it, regardless of how exactly they are described or addressed. Current Named Entity Recognition (NER) models aren't able to match this performance, often forcing scholars to rely on preexisting databases or manually label the texts they are studying, limiting the range of their analysis and the amount of works they can examine. 

This exploratory project studies several approaches to address this problem, suggesting a final pipeline that automatically extracts character references, merges them into cohesive entities and then uses embeddings to represent them. This representation appears to capture several interesting characteristics of its entities, like their gender, level of participation in the narrative (often a proxy of their role importance) and even non-human nature.

<br/>

## Research Summary
By developing a pipeline that goes further than simple Person entities' extraction, complemented with external information we subconsciously employ when comprehending events -- honorifics and their associated genres, lists of common male and female names, a dictionary of nicknames or otherwise diminutive name forms, and creating matching algorithms that are able to match simple name abbreviations when a writer tries to capture a name's sonority (like David Copperfield's \emph{Emily} and \emph{Em'ly} example) -- we have been able to improve the performance in terms of entities' extraction, merging, and representation.

Though this is also a generic pipeline, it has shown it is consistently able to extract useful characteristics, particularly evident when analyzing either gender, non-human characteristics, or role importance in the narrative (characters in the top half of the PCA-reduced space were very frequently, if not always, secondary characters with minor roles), and creates a novel way to visualize literary works and their characters, possibly opening new avenues for their study.

<br/>

## Installation and Usage
Code developed on Jupyter Notebooks, in an Anaconda environment, with Python 3.8. To reproduce the used conda environment, first run:

```bash
conda create -y -n ENV_NAME python=3.8 scipy pandas numpy matplotlib
conda activate ENV_NAME
conda install jupyterlab bokeh seaborn nb_conda_kernels
pip install jupyter_nbextensions_configurator

conda install pytorch torchvision -c pytorch
conda install -c conda-forge ipywidgets 
conda install -c conda-forge spacy 
conda install -c conda-forge nlp 
conda install -c anaconda scikit-learn 
conda install -c anaconda gensim 
conda install -c anaconda nltk 
conda install -c plotly plotly 

pip install tqdm
pip install Fuzzy
pip install stop-words
pip install nameparser
pip install editdistance
pip install pyjarowinkler
pip install libindic-utils
pip install libindic-soundex
python -m spacy download en_core_web_sm

conda install -c conda-forge keras
conda install -c conda-forge textblob
conda install -c conda-forge nameparser 

pip install git+https://github.com/huggingface/transformers.git@refs/pull/11223/head

curl https://raw.githubusercontent.com/carltonnorthern/nickname-and-diminutive-names-lookup/master/names.csv -o names.csv
curl https://raw.githubusercontent.com/carltonnorthern/nickname-and-diminutive-names-lookup/master/python-parser.py -o python_parser.py
curl https://media.geeksforgeeks.org/wp-content/uploads/male.txt -o ./male_names.txt
curl https://media.geeksforgeeks.org/wp-content/uploads/female.txt -o ./female_names.txt
```

Then, in a Python shell or notebook, run the following code once:

```python
import nltk
nltk.download('punkt')
nltk.download('wordnet')
nltk.download('stopwords')
nltk.download('conll2000')
nltk.download('averaged_perceptron_tagger')
```

**Usage:** 

The pipeline is composed of two main steps: 
1. Deploying LUKE model for NER, in order to extract Person entities from the work in question. This is done via the `get_LUKE_person_entities()` function.
2. Create a list of BookEntity instances and then generating their embeddings. This is done via two functions, `get_book_entities()` and `get_averaged_entity_based_BERT_embeddings()`.

Section 8 of the notebook contains several usage examples, for different books, authors and styles. It features two different lists of BookEntity, matched at different strictness levels (textually-close and more-laxly matched BookEntities), as well as the visualization of the PCA-reduced embeddings for all three embedding creation styles (`MASK`, `CLS` and `context`).


<br/>

## License
Automatic extraction and comparison of literary characters - Beatriz Borges

Copyright (c) 2021 EPFL

This program is licensed under the terms of the LGPL v2.1.
