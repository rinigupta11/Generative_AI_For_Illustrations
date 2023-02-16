# Generative AI For Illustrations

What if you could view illustrations of any novel you chose? I know I love to see visual representations of stories because they help me dive right into the plot. Many books, particularly novels, do not have embedded illustrations. Illustrations are a useful way of understanding the setting and characters of a book. Illustrations help us to imagine the world the novel creates in an effective and immersive way. Using natural language processing and generative neural networks, I aim to take the text of a book (for this project, it will be Harry Potter), and generate illustrations using DALL-E or Stable Diffusion. 

![image](https://user-images.githubusercontent.com/76021844/219227363-710759a9-3d0e-43ad-9ec6-c74375b19ff9.png)


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



