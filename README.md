Unstructured Data: Hotel Reviews Information Retrieval
Business Goal:
Our Client runs chained hotels in US and they now are able to collect feedback/review from customers. They wanna ultilize the reviews to improve customer experience by identifying underlying pain points from text, which can be achived by information retrieval

Part 1 Data Exploration
The dataset contains 20,491 entries with 2 features: text reviews and ratings from 1 to 5. By observing the distribution of ratings using a bar plot, we found that: Ratings of 5 have nearly 9,000 entries, making up almost 45% of the total. Ratings of 4 have nearly 6,000 entries, accounting for about 30%. Ratings of 3 are around 2,000 entries, approximately 10%. Ratings of 1 and 2 each have fewer than 2,000 entries, both below 10%, with rating 1 being the least frequent.

A new feature named length was created to record the number of words in the text reviews. A bar plot shows that most reviews have fewer than 3,000 words, with only 297 reviews having 3,000 or more words. These were considered outliers and removed from the dataset.

By using strip plots and distribution plots to observe the relationship between ratings and review lengths, we found that higher ratings are associated with longer reviews.

Without any restrictions on length: Reviews with a rating of 1 are less than 7k words. Reviews with a rating of 2 are less than 8k words. Reviews with a rating of 3 are mostly below 9.5k words, with one outlier close to 14k words. Reviews with a rating of 4 are below 10k words. Reviews with a rating of 5 are below 13k words. Restricting the length to less than 3k words: Regardless of the rating, most review lengths are concentrated between 0-1500 words. It is challenging to predict the rating based solely on the review length, indicating the need for keyword analysis.

Part 2 Text Preprocessing
Change the ratings from 1-5 to three categories: 1-2 as Bad, 3 as Neutral, and 4-5 as Good.
Clean the data in three steps: Remove punctuation from the reviews and convert all characters to lowercase. Remove stopwords from the reviews, e.g., "the," "is," "and." Perform lemmatization on the reviews. Lemmatization links similar meaning words as one word, e.g., "walked," "walk," "walks," "walking" -> "walk."
After cleaning the reviews, we removed 8.7% of redundant data (Length decreased from 13.55 million to 12.364 million)
Part 3 Next Steps
Create word clouds for different rating categories to observe the most frequently occurring words in each type of review, hoping to gain some insights from these words.
Use the LDA (Latent Dirichlet Allocation) model to create topics for different types of reviews. By examining the topics under different types of reviews, we can understand the reasons for low ratings and use this information to improve in the future. At the same time, we can check the topics under good reviews to maintain these positive aspects in the future.

Part 1 WordCloud
Created a word cloud for negative reviews and found some non-English words (nt) among the commonly used words. Further cleaned the data by removing all non-English words.
Created the word cloud again and found that commonly occurring words include hotel, room, night, service, stay...
Part 2 LDA baseline Model & N - Gram
Created a word-level LDA model with three different topics, each composed of five keywords. Each keyword has a certain weight in the topic. Found that commonly occurring words in the topics like "hotel," "resort," and "stay" were not helpful in identifying pain points, so these words were added to the stopword list and further removed.
Reran the word-level LDA model, and the resulting three topics were: "room, tell, book, check, night" "room, good, bathroom, location, small" "food, room, beach, time, service". Among these, there are few qualitative words, with "small" being the only negative one. The insight gained is that consumers frequently mention "room," "bathroom," "location," and "service," which need further attention.
Ran a phrase-level LDA model using 3-Gram on the reviews. The resulting three topics revealed pain points: "poor customer service" and "take long time" were frequently mentioned phrases that can be areas for improvement to enhance the customer experience. Positive aspects include "king size bed," "non-smoking room," "staff friendly helpful," and "harbour view room," which should be maintained in the future.
Part 3 LDA & Keyword Extraction
Rake & LDA: Using the Rake technique, the maximum length of keywords in each Rake review was 4. Some Rake reviews had zero keywords. After running the LDA model, three topics were generated, each with ten one-word terms. Many qualitative words were added, for example:
Positive: nice, better, good, great, best, recommend, beautiful
Negative: disappoint, horrible, avoid, worst, bad
However, it was unclear what specifically was considered good or bad, with only "attitude" being identified. The insight gained is that consumers frequently mentioned "star" and "staff," indicating these areas need attention. 2. KeyBert: The running time was too long, so it was abandoned.

Yake & LDA: Using the Yake technique, keywords in each Yake review were tri-grams (3-word phrases). After running the LDA model, six topics were generated, each with five 3-word phrases. The number of general words increased. Pain points identified included: poor customer service, bedroom bathroom small, door lock break, smell like mold. Positive aspects included: ocean view room, king size bed, minute walk nearest, staff friendly helpful, non-smoke room, good location close, staff polite helpful. Improvement: Pain points and positive aspects were more specific.

TextRank & LDA: Using the TextRank technique, the LDA model generated three topics, each with ten one-word terms. High-frequency words in the three topics included room, service, staff, time, day. The insight gained is the need to focus on room, service, staff, time, and day.

Part 4 TF-IDF & K-Means
Performed TF-IDF on the text and applied a K-Means model to the resulting matrix to identify clusters. By analyzing the SSE plot, the number of clusters (topics) was determined to be 8. The potential pain points found in the topics include:

rude, bad, manager, book, park, terrible, customer, staff, room, service

price, good, location, bathroom, poor, staff, disappoint, small, room, star

dirty, park, place, water, like, bathroom, bed, shower, night, room
