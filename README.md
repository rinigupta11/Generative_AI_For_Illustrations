# Generative AI For Illustrations
## Abstract: 

This project is aimed at generating illustrations using text from novels. The main questions I will answer involve what sorts of NlP techniques are most useful at extracting information from a large amount of text that would be useful for creating a text prompt to feed into a downstream generative AI model. Deciding the chunking of the text was an initial challenge as datasets usually include page and chapter delineations. In order to accomplish the NLP portion of this task, I summarized the appropriate chunking of text using BERT and then combined those results with named-entity recognition, specifically sPaCy's implementation, to get the entities out of the distilled text relevant for illustration. The entities I focused on were location, person, and organization. Using this methodology allowed me to have full sentences to feed into the Stable Diffusion API. I was able to generate reasonable illustrations, but limitations of the generative AI models made the results worse in some situations, particularly for drawing faces. Furthermore, more fine tuning in terms of adding more NLP steps like incorporating dependency parsing and creating a character dictionary should definitely be incorporated in future work to improve these illustrations. .

## Introduction: 

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

As far as I could find, there has not been any research done specifically to illustrate novels using NLP techniques. NLP applications using novels are a common field of research, so I drew insights from how to extract information in text based on other work done about NLP techniques. Specifically, named-entity recognition and summarization were a key part of my pipeline, so I referenced the documentation for these two areas of NLP frequently. 

## Methodology/Dataset: 

I used two novels for this project -- Harry Potter and the Sorceror’s Stone and The Great Gatsby. I sourced my Harry Potter dataset from Glozman and The Great Gatsby from Project Gutenberg. Specifically,  I webscraped the Project Gutenberg .txt file and Harry Potter using BeautifulSoup. I initially started by only using Harry Potter, but I wanted to see illustrations from two very different types of novels so I added the Great Gatsby to compare how the modeling performed on differing scenery types. 


Here is my process for loading in the text for The Great Gatsby. 
```
import requests
from bs4 import BeautifulSoup
import pandas as pd
import numpy as np

url = "http://gutenberg.net.au/ebooks02/0200041.txt"
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')
text = soup.get_text()
chapters = text.split("Chapter")
df = pd.DataFrame()
df['chapter_text'] = chapters
df.drop(0, inplace = True)
df['chapter_text'] = df["chapter_text"].replace("\r\n", "", regex=True).str.lower() 
```

Here is my process for loading in the text for Harry Potter.

```
import requests
from bs4 import BeautifulSoup
import pandas as pd
import numpy as np

url = "http://www.glozman.com/TextPages/Harry%20Potter%201%20-%20Sorcerer's%20Stone.txt"
response = requests.get(url)
soup = BeautifulSoup(response.content, 'html.parser')
text = soup.get_text()
chapters = text.split("CHAPTER")
df = pd.DataFrame()
df['chapter_text'] = chapters
df['chapter_text'] = df["chapter_text"].replace("\r\n", "", regex=True).str.lower()
df.drop(0, inplace = True)
```
I did not do much in terms of preprocessing because the downstream tasks worked better without a lot of preprocessing. I really only removed excess white space and made everything lowercase. Preserving the sentence structure was important for the NER and because I needed to extract full sentences for the text prompt in the end. These steps are visible in the code snippets above.

Here is the code for the summarization portion. I used the BERT extractive summarizer to distill the text into the important parts for its meaning. The num_sentences parameter was something I played with, especially depending on the chunk size. I usually selected between 3 and 5 sentences for this portion of the pipeline. For chapters, I selected more sentences and for sentences, I selected less.
```
! pip install bert-extractive-summarizer
from summarizer import Summarizer

chapter_2 = df['chapter_text'][2]
len(chapter_2)
model = Summarizer()
summarized_chapter_2 = model(chapter_2, num_sentences = 3)
```
Sometimes, the results of the summarization were already short enough to use as a text prompt. In that case, I have a conditional in the create_text_prompt function to just return the summarized text. Here is the code for creating the text prompt.
```
# Load the necessary libraries
import spacy


def create_text_prompt(text, prompt_length = 150): 
  if len(text)<200:
    return text
  # Load the English language model
  nlp = spacy.load('en_core_web_sm')
  # Process the chapter text with spaCy
  doc = nlp(text)

  # Get the sentences that contain entities
  sentences_with_entities = []
  for sent in doc.sents:
      if any(ent.label_ in ['PERSON', 'ORG', 'GPE', 'LOC'] for ent in sent.ents):
          sentences_with_entities.append(sent)

  # Create the prompt
  prompt = ''
  for i, sent in enumerate(sentences_with_entities):
      # Add the sentence text
      prompt += sent.text.strip()

      # Stop if the prompt is long enough
      if len(prompt) >= prompt_length:
          break
  return prompt.replace('  ', ' ')
```
Here, I extract the named entities from the sentences with the label PERSON, ORG, GPE, and LOC and then append those sentences to a list. Out of the entities that sPaCy recognizes, these seemed to be the most relevant for the task at hand. In order to create the prompt, I then add each of those sentences until the prompt length is reached. Sentences with those entities are most likely to include information relevant for illustration. I tended to use the prompt_length of 150 characters. 

I then fed the results of that function, the textual prompt, into the stable diffusion API. Here is the code for how I used the API to generate illustrations in an external url.
```
import replicate
import webbrowser
model = replicate.models.get("stability-ai/stable-diffusion")
version = model.versions.get("db21e45d3f7023abc2a46ee38a23973f6dce16bb082a930b0c49861f96d1e5bf")
output_url = version.predict(prompt=prompt_text)[0]
print(output_url)
webbrowser.open(output_url)
```

The figure below illustrates my pipeline for this project.
<img width="846" alt="Screen Shot 2023-05-14 at 11 15 10 PM" src="https://github.com/rinigupta11/Generative_AI_For_Illustrations/assets/76021844/3e3db4a1-4717-4bc6-8e51-15163bc61d95">



## Results: 

Given that this project included generative AI as the final result, it was difficult to calculate metrics to assess performance. At this stage, I assessed the performance of the model by visually inspecting the images and I am attaching some of them below for others to validate as well. I noticed that the model had some preconceptions of popular characters like Harry Potter. That is, when the text prompt included just the name Harry, the illustration did not include aspects of Harry Potter like glasses and other character attributes. In contrast, if the prompt included the term Harry Potter, the generative AI drew a much more realistic illustration including common character attributes for Harry. Furthermore, I noticed that Stable Diffusion was not the best at drawing faces, like in the illustration I got below: 

<img width="265" alt="Screen Shot 2023-05-15 at 6 51 48 PM" src="https://github.com/rinigupta11/Generative_AI_For_Illustrations/assets/76021844/c9ce14ca-0d77-48ba-9a78-66bf4c72be74">


In terms of the prompts, the parameters I found to be the best were 3 sentences and a prompt length of 150 characters. These hyperparameters could change depending on the novel being illustrated, but these were very influential in the final result. The NER performed well with the given entities but I would like to include dependency parsing in future iterations of this project. The BERT summarizer was the most effective summarizer I tried out of about 4 or 5 prepackaged summarization tools in Python. Extractive summarization made the most sense for this use case since I wanted to use the sentences of the book to aid illustration downstream. Overall, I was satisfied with the results, but Stable Diffusion did make some results worse than I would like despite the prompts being informative. The main scope of this project was my NLP methodology though, and the prompts generated tended to encompass items to illustrate. 

For the prompt, “About half way between West Egg and New York the motor-road hastily joins the railroad and runs beside it for a quarter of a mile, so as to shrink away from a certain desolate area of land,” I obtained this illustration: 

<img width="322" alt="Screen Shot 2023-05-15 at 6 22 47 PM" src="https://github.com/rinigupta11/Generative_AI_For_Illustrations/assets/76021844/d15ab6e7-abde-4bab-99a1-00563e5127f2">


For the prompt, "On Sunday morning while church bells rang in the villages along shore the world and its mistress returned to Gatsby's house and twinkled hilariously on his lawn.", I got this illustration:


<img width="376" alt="Screen Shot 2023-05-15 at 6 23 18 PM" src="https://github.com/rinigupta11/Generative_AI_For_Illustrations/assets/76021844/d980be74-5b62-4600-948e-05d19b6b341c">

For the prompt, “Harry Potter sighed and stretched out on the bed. He privately agreed, though the thought didn't cheer him up at all.”, I got this illustration. 

<img width="367" alt="Screen Shot 2023-05-15 at 6 40 10 PM" src="https://github.com/rinigupta11/Generative_AI_For_Illustrations/assets/76021844/b357764a-43ec-4757-b895-1bb5a0ae6f0f">


## Discussion: 

The first item that I had to make decisions about was the optimal way to chunk the text. There are a few obvious delineations that are possible, namely paragraph, page, or chapter. I did not work with paragraphs for the purpose of this research because the novels I worked with included too much text to get an illustration for each paragraph. Furthermore, paragraphs might have some information for illustration in the end of the paragraph that continues into the next paragraph. This issue could be the case with pages as well, but should not be an issue with chapters. For the purposes of this project, I found chapters to be the best delineating method because the summarization algorithm was nicely able to distill only the important information in the chapter for the NER to work on. Another way to figure out optimal chunking would be to use a clustering approach in the future. 

The summarization worked just as I would want it to, but perhaps seeing how abstractive summarization works in comparison to extractive summarization would be a useful avenue to explore in the future. Furthermore, doing some more extensive and robust testing about the optimal number of sentences would add validity to the results I obtained. 

It would also be useful in the future to do some more intricate experimentation with named-entity recognition. I just used the sPaCy implementation for NER and got decent results, but depending on the complexity of the text, more advanced NER might be useful. The novels I used for this project were written in fairly easy to understand modern English, but certain novels, such as those written by Shakespeare for example, might require more advanced NER. Furthermore, in order to get accurate depictions of characters, creating a dictionary of all the character names and adding adjectives/traits about the characters into that dictionary would be useful for non-popular novels. 

Another step that would likely improve the results (meaning the quality of the text prompt) would be to include dependency parsing. Doing so would improve the richness of the prompts as one could extract descriptive information. What I found difficulty with in incorporating these new steps was figuring out how to preserve the full sentence prompt structure in the end. Including dependency parsing would require one to figure out how to sew the relevant text back together into a form that Stable Diffusion can understand. 

In general, one major area of improvement is that it is very difficult to use metrics to assess the performance of my modeling. Given that (1) there is no good way to judge the quality of a text prompt and (2) the generative AI can lead to worse or better results than the quality of the text prompt, some creativity would be necessary to judge these results. If I had more time, I would have likely had others input some text and give ratings for the results in order to assess how well the models are doing. I plan to create a front-end in the future, so hopefully I can collect more data with user input. 

The implications of this research are diverse. Illustrations are useful for having a better experience reading a novel, but they could also be useful in numerous other ways. Illustrations open up more difficult novels to less advanced readers and aid in comprehension. Furthermore, illustrations can be a useful tool when someone is learning a new language for the first time. Finally, illustrations can be useful when brainstorming as they can help bring ideas to life. 

## Conclusion: 
This section summarizes the main findings of the study, restates the research question, and discusses the implications of the research for future research and practice. [NOTE: 1-2 paragraphs]

## References: 
Bert-extractive-summarizer. PyPI. (n.d.). Retrieved April 27, 2023, from https://pypi.org/project/bert-extractive-summarizer/ 

ChatGPT, personal communication, February 11, 2023

EntityRecognizer · spaCy API Documentation. EntityRecognizer. (n.d.). Retrieved April 27, 2023, from https://spacy.io/api/entityrecognizer 

Harry Potter and the Philosopher's Stone. GitHub. (n.d.). Retrieved April 27, 2023, from https://github.com/formcept/whiteboard/edit/master/nbviewer/notebooks/data/harrypotter/Book%201%20-%20The%20Philosopher's%20Stone.txt 

Run Stable Diffusion with an API. Replicate. (n.d.). Retrieved April 27, 2023, from https://replicate.com/blog/run-stable-diffusion-with-an-api 

The Great Gatsby. Project Gutenberg Australia. (n.d.). Retrieved April 27, 2023, from http://gutenberg.net.au/ebooks02/0200041.txt 












