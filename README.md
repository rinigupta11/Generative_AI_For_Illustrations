# Generative AI For Illustrations
## Abstract: 
This section provides a brief summary of the project, highlighting the main research question, methodology, results, and conclusions. It should be concise and clear, usually limited to 250-300 words.

## Introduction: 
The introduction provides background information on the topic being studied, and explains the motivation behind the research question. This section also highlights the novelty of the research, its importance, and its potential impact. [NOTE: 1-2 paragraphs]

What if you could view illustrations of any novel you chose? I know I love to see visual representations of stories because they help me dive right into the plot. Many books, particularly novels, do not have embedded illustrations. Illustrations are a useful way of understanding the setting and characters of a book. Illustrations help us to imagine the world the novel creates in an effective and immersive way. Using natural language processing and generative neural networks, I aim to take the text of a couple books and generate illustrations using Stable Diffusion. The books I chose to showcase for this project are Harry Potter and the Sorceror's Stone and the Great Gatsby. 

![image](https://user-images.githubusercontent.com/76021844/219227363-710759a9-3d0e-43ad-9ec6-c74375b19ff9.png)


## Literature review: 
This section provides a comprehensive review of the relevant literature on the topic being studied. It highlights the strengths and weaknesses of previous research and identifies gaps in the current understanding of the topic. [NOTE: Not required but will make an impression. 1-2 paragraphs]

## Methodology/Dataset: 
This section explains the research design, including the data sources, data collection methods, and analysis techniques used. It also discusses any assumptions made and the rationale behind the chosen methods. [NOTE: 2-4 paragraphs]

## Results: 
This section presents the findings of the research, including descriptive statistics, tables, and graphs. It should provide a clear and concise summary of the main results, highlighting any patterns or trends observed. [NOTE: 2-4 paragraphs]

## Discussion: 
The discussion section interprets the results of the study in light of the research question and literature review. It should explain how the findings relate to previous research and provide a critical analysis of their implications. [NOTE: 6-10 paragraphs]

## Conclusion: 
This section summarizes the main findings of the study, restates the research question, and discusses the implications of the research for future research and practice. [NOTE: 1-2 paragraphs]

## References: 
This section provides a list of all the sources cited in the paper, following a specific citation style (e.g., APA, MLA).



From this project, I hope to understand how to best extract meaning from a large body of text that is relevant for illustration. An analysis would be necessary here to determine which parts of speech and other context clues could be important for words to illustrate. Stringing these words together in a meaningful way will also prove to be a challenge. The generative modeling is not the main scope of my project and will simply demonstrate the relative success or failure of my NLP methodology in a visual way. 

Research Questions:
1. What parts of a grouping of text are important for illustration?
2. How should the text be grouped (i.e. by pages or chapter or something else)
3. What NLP methods are useful for extracting meaning from the text relevant to illustration?
4. Which generative model works better for this task?
5. How do we judge the "accuracy" of the results? Is visual inspection sufficient for determining merit of this modeling approach?
6. Where can book data be sourced?
7. What cleaning has to be performed on the data gathered?
8. How can this be put into a container for easy inference?


TODO:
1. Get Harry Potter book data
2. Remove punctuation, lowercase, and perform other preprocessing steps that seem valuable.
3. Experiment with preprocessing to preserve capturing the meaning for illustration.
4. Extract words that are important for illustration.
5. String those extracted words together into a "summary" for the generative model
6. Understand how to use the generative model and input text in for illustration.
7. Code will be needed to pipe the different parts of this process together including scraping, preprocessing, extracting, feeding into generative model, and getting an image out in the end. 



