# An NLP Project on "The Silmarillion" by J.R.R. Tolkien
<img src="Images/Valinor.jpg" width="900" height="400">

# 1 Introduction
## 1.1 Natural Language Processing (NLP) 
Natural Language Processing, or NLP, is within the field of linguistics and refers to a set of techniques for manipulation of natural language, such as speech and text, using software. NLP is a sub-branch of data science and has many applications, though it has been particularly useful in the healthcare industry, with the increased use of Electronic Health Records (EHR). For example, we can now predict risk for suicide and suicidal ideation by applying NLP to EHR's ([1](https://www.nature.com/articles/s41598-018-25773-2)). Companies can apply NLP to social media or product reviews, in order to understand their customers better and ultimately offer a better product or service ([2](https://www.researchgate.net/publication/309691845_A_Review_of_Natural_Language_Processing_Techniques_for_Opinion_Mining_Systems)). Since I don't have ready access to EHR's, I've decided to do this NLP project on one of my favorite books, *The Silmarillion*, by J.R.R. Tolkien. 

## 1.2 The Silmarillion
When people hear of J.R.R. Tolkien, his book *The Silmarillion* is typically not the first that comes to mind. After all, it's difficult to compete with books like *The Hobbit* and *The Lord of the Rings*, as both are far easier to read and have big budget Hollywood films tied to them. Still, *The Silmarillion* holds a special place in my heart. It lays the foundation for Middle-earth and sets the stage for the more popular books in Tolkien's catalogue. The book is incredibly dense, content-wise, and written in a formal and archaic style, more similar to the Bible than *The Lord of the Rings*. It's challenging and some find it impenetrable. Though, I can't recommend it more to fans of fantasy and for Tolkien fans, it's simply essential reading. 

Having read both *The Hobbit* and *The Lord of the Rings* at a very early age, I admit to having avoided *The Silmarillion* until about 10 years ago. I was part of a seminar of about 30 other geeks who met online bi-weekly to discuss each chapter in detail. The seminar was later released as a podcast ([3](https://tolkienprofessor.com/lectures/courses/silmarillion-seminar/))([4](https://itunes.apple.com/us/course/the-silmarillion-seminar/id599723153)), and for anyone planning to read the book....maybe it will help!

# 2 Data
The full text of *The Silmarillion* used in this project is accessible here ([5](https://archive.org/stream/fegmcfeggerson_gmail_4731/473%20%281%29_djvu.txt)). After scraping the webpage and cleaning the text, I extracted the entire text and the text for each seperate chapter and created a PANDAS dataframe. I then used the modules NLTK and Text Blob to extract some basic information from the text and then the modules Text Blob, VADER, and the NRC Emotion Lexicon to perform sentiment analyses by chapter. All values for sentiment analyses and descriptive text analyses were added to the dataframe, pictured below.

<img src="Images/Data.jpg" width="900" height="190">

# 3 Text Analysis
## 3.1 Word Cloud 
Using the Natural Language Toolkit module (NLTK)([6](https://www.nltk.org/)), I first "tokenized" each word from the book, extracting each "token" and removing all whitespace, punctuation, etc. Then, I made all tokens lower-case for consistency and removed "stop-words." Stopwords are refered to as the words in a text that don't particularly convey significant meaning, such as "the", "in", and "a". Finally, I got the frequency of each word, or toke, and then created a "word cloud" of the most frequently used words superimposed over a shield (much easier than to superimpose over a sword!).

<img src="Images/Silmarillion_wordcloud.png" width="350" height="450">

## 3.2 Most Frequent Nouns
Again, using NLTK, I conducted the same preliminary text manipulation as above (toekize, lower-case, and stopwords) on each separate chapter. Following this, I used NLTK to tag the different parts of speech for each word. This tags each word in the text with its part of speech, for example "NN" for noun and "VB" for verb. The top 10 most frequent nouns were extracted for each chapter and appear on the table below. This helps give a sense of what each chapter is about. For example, the *Ainulindalë* is a Creation story of the origin of the universe. The Ainur are spiritual beings, akin to angels, who sing a vision of the universe into existence based on a theme given to them by Iluvatar (God). Melkor is one of the Ainur and the most powerful and knowledgable of them all. He broke from the harmony and developed his own song, causing discord in the overall theme. After some struggle, Iluvatar stopped the music and showed them the world they had created which was before just a vision. As you can see, the top 10 nouns for this chapter strongly reflect the summary just given.

<img src="Images/Most_freq_nouns.jpg" width="800" height="1200">

## 3.3 Lexical Density
Lexical density is formally defined as the number of lexical words (or content words) divided by the total number of words in a text. Lexical words give a text its meaning and provide information regarding what the text is about (nouns, adjectives, verbs, and adverbs). Other words such as articles (a, the), prepositions (on, at, in), conjunctions (and, or, but), and so forth are more grammatical in nature and, by themselves, give little or no information about what a text is about. With the above in mind, lexical density is simply the percentage of words in a text that offer information about what is being communicated. Lexical density is simply a measure of how informative a text is. 

Because this score is sensitive to total word count, I included both the normal scores for each chapter in addition to the "normalized" scores. The "normalized" scores were attained by only using the first 3440 words in each chapter, as this is the word count for the shortest chapter. Becuase of this sensitivity to word count, it is difficult to compare one text to another, but by using "normalized" scores, we can compare chapters to a reasonable degree. The range of scores is 0 to 1 (0-100%), with higher scores indictating higher lexical density. The scores did not change much between the two methods and the average across all chapters was around 0.25 (*SD* = 0.06) for the normal method and 0.27 (*SD* = 0.04) for the "normalized" method. 

<img src="Images/Lexical_Density.png" width="700" height="600">

# 4 Sentiment Analyses
Sentiment analysis is a method of contextual mining of text, or unstructured data, that identifies and extracts subjective information from that source, whether it be a book, tweets, movie and product reviews, or electronic health records. Sentiment analysis is the process of determining the attitude or emotion of the writer. 

## 4.1 TextBlob
[TextBlob](https://textblob.readthedocs.io/en/dev/quickstart.html) is a Python library for processing textual data and helps with tasks such as part-of-speech tagging, noun phrase extraction, sentiment analysis, and more. The sentiment function of TextBlob produces two properties: *polarity* and *subjectivity*. Polarity is a number [-1,1] where 1 refers to a positive statement and -1 refers to a negative statement. Subjectivity is a number [0,1] where a 0 refers to an objective statement and 1 refers to a subjective statement. Subjective sentences generally refer to personal opinions, emotions or judgments and objective refers to factual information. In the graph below, we see that the Subjectivity score is typically around 0.4, give or take, indicating the text is generally more objective than subjective. This makes sense as the book is closer to a historical account, rather than a subjective narrative. The Polarity score across chapters is pretty low between 0.0-0.2, but never drops into the negative range. This is surprising for anyone who has read chapters like *Of The Darkening of Valinor* and *Of Turin Turambar*, though it is reassuring to see that those are some of the lowest points on the line. 

<img src="Images/TextBlob_polarity.png" width="900" height="700">

## 4.2 VADER
[VADER](https://github.com/cjhutto/vaderSentiment) (Valence Aware Dictionary and sEntiment Reasoner) is a sentiment analysis tool that is specifically designed to examine sentiments expressed in social media. This lexicon is sensitive to both the *polarity* and the *intensity* of sentiments expressed in social media contexts, and is also generally applicable to other forms of text. Because VADER was specifically designed for use with short pieces of text on social media, I decided to loop the analyses through each sentence of a chapter and then to get the average over all sentences for a total sentiment score for that particular chapter. 

Seen below are the full results from the VADER analysis. You can see right away that the *Neutral* sentiment score is quite high across all chapters. This is likely due to the style of writing in *The Silmarillion*, which is quite formal and impartial. As an example, consider this passage from the chapter with the highest *Negative* rating (*Of the Darkening of Valinor*): *"In a ravine she lived, and took shape as a spider of monstrous form, weaving her black webs in a cleft of the mountains. There she sucked up all light that she could find, and spun it forth again in dark nets of strangling gloom, until no light more could come to her abode; and she was famished*." This passage paints quite a gloomy scene, but it's written in such a formal and matter of fact way, as is the book in general. Now imagine an account of the same monstrous light-eating spider in the form of a personal narrative by a character witnessing it all...I expect the *Neutral* score would be far lower!

<img src="Images/VADER_full.png" width="900" height="700">

Now, let's look at the same graph without the *Neutral* line and zoomed in to the rest. 

<img src="Images/VADER_zoom.png" width="900" height="700">

<img src="Images/VADER_compound.png" width="900" height="700">
