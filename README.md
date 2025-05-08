# SMA_final_project

JUSTE SOME EXAMPLES OF USING :

Final project SMA : Issue based articles and Horse race articles in the 2024 legislative elections


Here we can find our main results 

![DAILY_12](https://github.com/user-attachments/assets/bd789bd7-a914-44f7-a847-68e62befff58)


# Titre 1

# Introduction




France's 2024 snap legislative elections garnered significant national and worldwide attention due to the rarity of the event, the risks at play and it's sudden 
announcement that resulted in heightened political polarisation. The elections, which were prompted by President Emmanuel Macron's decision to dissolve the National Assembly, became the primary topic of contention in the nation's political climate, with competing perspectives erupting across all ideological parties (Chabal & Behrent, 2024).The election was a crucial turning point in modern French politics, a the election mainly revealed the increasing prominence of both far-left and far-right groups, particularly the newly formed Nouveau Front Populaire created to combat the extremism and dominance of the Rassemblement National (Ivaldi, 2024).  Given the topic's importance to both political science and media studies, our group has decided to study how French national media outlets from an array of political orientations framed the 2024 snap elections, paying particular attention to the popular use of strategic game framing.
The June 9 to July 7, 2024, timeframe was chosen for a number of reasons: it encompasses the entire duration of the snap election campaign, from the dissolution of the French National Assembly to the last round of voting; it is a period of high political importance, enabling the identification of framing trends during highly consequential electoral pressure; and it offers a targeted timeframe where media narratives are likely to be most concentrated and politically consequential.
The media outlets included for this study span a broad range of ideologies and were picked because of their national significance, their ability to shape political discourse, and the variety of content types they provide.
Libération, Le Nouvel Observateur, L'Humanité, Le Monde Diplomatique, and Alternatives Économiques were picked to represent the left-leaning media outlets.  Le Monde, Les Échos, 20 Minutes, La Tribune, and La Croix were the examples for center-leaning media. And finally  Valeurs Actuelles, Le Figaro, L'Express, and Le Point for right-leaning.  These publications offer a thorough and representative cross-section of French political journalism during the election season through publishing frequently, and in a variety of formats, including news reports, analysis pieces, interviews, editorials, features, and profiles, in addition to having varying editorial stances.  They are essential to our inquiry because of their impact on public debate and because of their large audience, which guarantees that the framing techniques used represent the mainstream media landscape (Lecheler & De Vreese, 2012).
Our study seeks to provide understanding on the connection between ideological polarisation and political media framing, specifically in the modern French environment.  We aim to analyse how strategic game framing, which prioritises focus on competition and electoral manoeuvring over substantive policy focus, may have influenced voter perceptions and the larger political narrative by concentrating on well-known national media outlets with distinct editorial identities.  Through doing this, we can identify the potential biases present in these outlets' reporting approaches as well as their persuasive power.
# Literature Review


# II) Methodology

## II.A) Extraction and pre-processing

First of all, we collected on Factiva all the articles from 09/06/2024 to 07/07/2024 containing at least once the word "legislative election".
These articles come from newspapers with three political orientations:
  
  •	From the center: Le Monde (daily), La Tribune du Dimanche (weekly), La Croix, (daily), 20 Minutes (daily): **a total of 679 articles**
  
  •	From left: Libération (daily), Le Nouvel Obs (weekly), L'Humanité (daily), Marianne (weekly): **127 articles in total**
  
  •	From the right: Le Figaro (daily), Le Point (weekly), Valeurs Actuelles (weekly), L'Express (weekly): **202 articles in total**

From Factiva we extract PDF files of all the articles, a first notebook codes_data_ formating.ipynb allows us to convert these PDFs of several hundred articles into CSV files, by political orientation and by round (T1 for first round of elections, T2 for second round). They are available in the **SMA_final_project /data/ data_articles** folder and are named: articles_extraits_T1_centre.csv, articles_extraits_T2_centre.csv, articles_extraits_T1_droite.csv, articles_extraits_T2_droite.csv, articles_extraits_T1_gauche.csv, articles_extraits_T2_gauche.csv.

**Part A) Data import and small pre-processing** of the notebook **article_analysis.ipynb** allows you to bring these files together in a single dataframe while adding two columns, the political orientation and the type of newspaper (daily/weekly). This is also an opportunity to do a pre-processing to remove the few parasitic lines (name of the newspaper, date of extraction, article identifier) due to the presentation format of the articles on Factiva)

## II.B) TOP 500 adjectives and nouns and classification

For each political orientation, in part **B) Word Count**, we use *spaCy* and the *fr_core_news_sm model* to extract the list of most used nouns and adjectives. For each political orientation, we create a csv with the list of the 500 most common nouns and 500 adjectives. We then manually assign each word the classification **'Issue Based ' (IB)** or **'Horse based ' (HR)**, we put nothing if it does not correspond to any category, which is regular.
The classified tables can be viewed on GitHub in the **data/TOP 500** repertory
In total we have: **142 IB nouns** against **101 HR nouns**, and **222 IB adjectives** against **97 HR adjectives**. The rest are classified as neutral.

## II.C) Topic modeling

*gensim.models* library that we run on the corpus of articles from left-leaning newspapers, articles from right-leaning newspapers, and centrist newspapers. We also run the algorithm on all articles from daily newspapers and all articles from weeklies. The results will then be analyzed.
The code for this step is in part **D) Topic analysis** . Here are the parameters we use for our LDA:

![image](https://github.com/user-attachments/assets/0d440703-b7e1-4ed6-9779-cd3355270720)

 
This subject analysis is an interesting interlude in our work

## II.D) Classifying articles

Next, we seek to assign each article a score of either "Horse Race" (HR) or "Issue- Based " (IB) . To do this, we analyze all the words in the article to identify those that appear in our lists of nouns or adjectives previously classified as HR or IB. For example, if the word "survey ," classified as HR, appears three times in an article, the article is awarded three points for this term.
Each article thus obtains four distinct scores:
 
  •	A score named IB ,
  
  •	A score named HR ,
  
  •	adjective score IB ,
  
  •	HR adjective score .
  
In order to compensate for the imbalance between the number of terms classified in each category (142 IB nouns against 101 HR nouns, and 222 IB adjectives against 97 HR adjectives), we apply correction factors :
  
  •	The score of HR names is multiplied by 142/101 ,
  
  •	That of HR adjectives by 222/97 .
  
By summing the corrected noun and adjective scores, we obtain the overall HR and IB scores for each article. These scores are then normalized by dividing them by the total number of words in the article, to avoid a long article automatically having a higher score than a short one.

Finally, we calculate the HR/IB ratio . A ratio greater than 1 indicates that the article is more Horse Race oriented , while a ratio less than 1 suggests an Issue- Based orientation .

For each political orientation, we generate an intermediate CSV in which, per article, we have the list of IB words, the list of HR words, the list of IB adjectives and the list of HR adjectives. These files are named: **df_centre_IB_HR_ranked.csv** , **df_centre_IB_HR_ranked.csv** , **df_centre_IB_HR_ranked.csv** and are accessible on GitHub in the **data/ IB_HR_rank** directory

The code for this processing can be found in section **E.1 – Calculating IB and HR scores** of the notebook.

## II.E) Plotting curves

We can derive some statistics from the score given to each article.
 
However, we prefer visualization in the form of curves, which allows us to more intuitively follow the evolution of scores over the electoral period.
this , we use the pandas groupby function to group the data by day and political orientation. This reduces the number of rows in the dataframe by calculating the average of the scores for each group. For example, the row corresponding to 06/18/2024 - left will represent the average of the scores of all articles published that day in left-leaning newspapers. The code used to perform this temporal and political grouping, as well as to generate the associated curves, can be found in section **E.2 – Graphs of IB/HR scores** of the notebook .

In a second step, we analyze the scores according to the rhythm of publication of the newspapers, this time grouping the articles according to whether they come from daily or weekly newspapers , while maintaining the same averaging logic. The code corresponding to this processing can be found in section **E.3 – Graphs by rhythm of publication** .

We plotted a week where each point represents a week to achieve a smoothing effect. A daily plot would be too sensitive to variations in the number of publications each day, making the data too unstable. Furthermore, while a daily frequency can be considered for articles from the center, which has several thousand, it is not suitable for the extreme left or right, where there are only a few hundred articles over the entire period.

All results and figures produced are available in the results repertory **SMA_final_project/Results/**


# III) Results
As previously explained, in our analysis we decided to extract the most common 500 adjectives and nouns, divided between HR (horse race) and IB (issue based) terms. The most common HR nouns were "pourcent", "élection" and "juin". The most common IB nours were "euro", "programme" and "marché". 
The most common HR adjectives were "politique", "gauche" and "éxecutif", with "politique" being by large the most common one (around 10 times more used than "gauche"). As per IB adjectives, the most used were "européen", "public" and "social".
A much greater variety in HR nouns and adjectives can be noted, given the much higher usage of such terms in media during the month of June 2024. 

![top_300_adjectifs_HR](https://github.com/user-attachments/assets/fb67b7e3-8e51-4910-8257-7008f67dc6f8)

![top_300_adjectifs_IB](https://github.com/user-attachments/assets/f40cdc8b-3107-4837-8b23-d876a22c1b4f)

![top_300_nouns_HR](https://github.com/user-attachments/assets/d84e00a8-bc90-40c7-b69e-5f7fb472b2ad)

![top_300_nouns_IB](https://github.com/user-attachments/assets/545dc5ea-a234-4e4f-8d98-9810ab80f745)

After having calculated the corrected HR and IB scores of the nouns and adjectives, we normalize them by dividing them by the total number of words in the article, to avoid a long article automatically having a higher score than a short one. With the data obtained, we calculate the HR/IB ratio and we trace the evolution of the score in the election period visualized as curves for left wing, right wing and centre-aligned media. 

![evolution_IB_HR](https://github.com/user-attachments/assets/a677c682-4955-46f7-91c1-59b43dd09213)

![Average IB HR by political orientation](https://github.com/user-attachments/assets/03bf0dfa-73c5-4a14-a625-c78cc69d5f7a)

From the obtained representation we notice a consistent predilection for HR terms in the case of left wing, right wing and centre-aligned media. Centre-aligned media lead the table for what concerns the usage of both HR and IB terms. The average HR/IR ratio indicates a prevalence of HR oriented articles. 

However, great differences between newspapers can be noticed. It could be possible to argue that the HR/IB usage difference depends more on the newspapers themselves and their style of reporting than on the media alignment. Daily newspapers favour HR terms, as opposed to weekly releases.  


# Bibliography

**gras**

*italic*



