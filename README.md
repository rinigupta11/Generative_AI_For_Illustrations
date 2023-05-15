# Generative AI For Illustrations
## Abstract: 
This section provides a brief summary of the project, highlighting the main research question, methodology, results, and conclusions. It should be concise and clear, usually limited to 250-300 words.

This project is aimed at generating illustrations using the text from novels provided. 

## Introduction: 
The introduction provides background information on the topic being studied, and explains the motivation behind the research question. This section also highlights the novelty of the research, its importance, and its potential impact. [NOTE: 1-2 paragraphs]

What if you could view illustrations of any novel you chose? I know I love to see visual representations of stories because they help me dive right into the plot. Many books, particularly novels, do not have embedded illustrations. Illustrations are a useful way of understanding the setting and characters of a book. Illustrations help us to imagine the world the novel creates in an effective and immersive way. Using natural language processing and generative neural networks, I aim to take the text of a couple books and generate illustrations using Stable Diffusion. The books I chose to showcase for this project are Harry Potter and the Sorceror's Stone and the Great Gatsby. 

From this project, I hope to understand how to best extract meaning from a large body of text that is relevant for illustration. An analysis would be necessary here to determine which parts of speech and other context clues could be important for words to illustrate. Stringing these words together in a meaningful way will also prove to be a challenge. The generative modeling is not the main scope of my project and will simply demonstrate the relative success or failure of my NLP methodology in a visual way. 

![image](https://user-images.githubusercontent.com/76021844/219227363-710759a9-3d0e-43ad-9ec6-c74375b19ff9.png)

Research Questions:
1. What parts of a grouping of text are important for illustration?
2. How should the text be grouped (i.e. by pages or chapter or something else)
3. What NLP methods are useful for extracting meaning from the text relevant to illustration?
4. Which generative model works better for this task?
5. How do we judge the "accuracy" of the results? Is visual inspection sufficient for determining merit of this modeling approach?
6. Where can book data be sourced?
7. What cleaning has to be performed on the data gathered?
8. How can this be put into a container for easy inference?

## Literature review: 
This section provides a comprehensive review of the relevant literature on the topic being studied. It highlights the strengths and weaknesses of previous research and identifies gaps in the current understanding of the topic. [NOTE: Not required but will make an impression. 1-2 paragraphs]

As far as I could find, there has not been any research done specifically to illustrate novels using NLP techniques. NLP applications using novels are a common field of research, so I drew insights from how to extract information in text based on other work done about NLP techniques. Specifically, named-entity recognition and summarization were a key part of my pipeline, so I referenced the documentation for these two areas of NLP frequently. 

## Methodology/Dataset: 
This section explains the research design, including the data sources, data collection methods, and analysis techniques used. It also discusses any assumptions made and the rationale behind the chosen methods. [NOTE: 2-4 paragraphs]
<img width="846" alt="Screen Shot 2023-05-14 at 11 15 10 PM" src="https://github.com/rinigupta11/Generative_AI_For_Illustrations/assets/76021844/3e3db4a1-4717-4bc6-8e51-15163bc61d95">

## Results: 
This section presents the findings of the research, including descriptive statistics, tables, and graphs. It should provide a clear and concise summary of the main results, highlighting any patterns or trends observed. [NOTE: 2-4 paragraphs]

## Discussion: 
The discussion section interprets the results of the study in light of the research question and literature review. It should explain how the findings relate to previous research and provide a critical analysis of their implications. [NOTE: 6-10 paragraphs]

## Conclusion: 
This section summarizes the main findings of the study, restates the research question, and discusses the implications of the research for future research and practice. [NOTE: 1-2 paragraphs]

## References: 
Bert-extractive-summarizer. PyPI. (n.d.). Retrieved April 27, 2023, from https://pypi.org/project/bert-extractive-summarizer/ 

ChatGPT, personal communication, February 11, 2023

EntityRecognizer · spaCy API Documentation. EntityRecognizer. (n.d.). Retrieved April 27, 2023, from https://spacy.io/api/entityrecognizer 

Harry Potter and the Philosopher's Stone. GitHub. (n.d.). Retrieved April 27, 2023, from https://github.com/formcept/whiteboard/edit/master/nbviewer/notebooks/data/harrypotter/Book%201%20-%20The%20Philosopher's%20Stone.txt 

Run Stable Diffusion with an API. Replicate. (n.d.). Retrieved April 27, 2023, from https://replicate.com/blog/run-stable-diffusion-with-an-api 

The Great Gatsby. Project Gutenberg Australia. (n.d.). Retrieved April 27, 2023, from http://gutenberg.net.au/ebooks02/0200041.txt 








