# Quote correlation in QuoteBank
## Datastory
https://kallebju.github.io/ada-website/
## Abstract
The aim of this project is to investigate similarities between US politicians whose quotes are included in QuoteBank. We will look at two different properties for solving this task: the actual content of the quotes and the time when the politicians are quoted. For each of these types of similarity measures, we will use a clustering algorithm to find potential clusters of politicians with similar charachteristics, and try to analyze these clusters.

<!--
The aim of this project is to investigate this phenomena by analyzing the  correlation among speeches made by different persons within some category (e.g. politicians, movie stars etc.). For instance, a quote of one politician might spread within the community and trigger an action, in the form of a quote, from another politician. Hence, by using the QuoteBank dataset, we will try to see if it is possible to identify speakers whose time of quotings are highly correlated. If time permits, we will refine this study by also including a quote similarity score when calculating the quote correlation. By including this, we aim for being more confident that the response quote actually is related with the preceding quote in terms of content.
-->
## Research Questions
We have chosen to analyze quote similarity among politicians. However, the proposed method can be used for doing the same for other catogories of speakers. Hence we will aim for answering the following research questions:
* Is there a high correlation for the time of quoting for some pairs of politicians?
* Does some politicians make similar quotes in terms of content?
* Are the politicians that are deemed similar by our method, also similar with respect to other charachteristics such as party membership and gender?

## Proposed additional datasets
To be able to extract US politicians from the QuoteBank dataset, we use the provided metadata set, which was collected from Wikidata. This is also necessary for conducting answering our third research question regarding the distribution of party memebership and gender.

## Methods
### Data collection
The first step will be to collect quotes from QuoteBank that were made by people from the selected category, i.e. politicians. This will be done using the provided metadata set which lists, among other speaker attributes, the occupation of the speaker.

### Analyzing correlation between time of quoting
A fairly basic way to study the correlation between quotes from several politicians is to simply study the temporal correlation between their speeches.
We extract the dates of each politician's quotes and we make a signal representing the number of quotes of each politician per week of the year. We can then apply temporal signal analysis tools such as the cross-correlation.

#### K-Shape Clustering
In order to cluster all these time series, we quickly noticed that the use of K-means was inadequate. The algorithm systematically put the vast majority of time series in a single cluster.
We then looked at a clustering method adapted to time series, based on the concept of cross-correlation between signals: K-Shape.
The cross-correlation is itself based on a convolution between signals and allows to analyze the similarity between time signals ( as a distance), despite a temporal shift between them.

### Quote similarity analysis
There exists many well established methods for quantifying the similarity between texts. In this project, we plan to use techniques from vector space retrieval, where the quotes first are transformed into a vector space. These vectors are then compared to each other using the cosine distance; where vectors with a small distance are considered to be similar. One way of transforming the quotes into a vector space is to use a vector of weights for some terms. As weights one can use the TF-IDF score, defined as follows:

<img src="https://render.githubusercontent.com/render/math?math=tf(i,j) \cdot idf(i)">

where `tf(i,j)` is the term frequency of term `i` in document `j`, and `idf(i)` is the inverse document frequency, defined as:

<img src="https://render.githubusercontent.com/render/math?math=idf(i) = log\left(\frac{n}{n_i}\right)">

where `n` is the total number of documents (or quotes in this context) and `n_i` the total number of documents in which term `i` occur. [1] An advantage of using TF-IDF scores as weights instead of only the counts of the term, is that the `idf` part compensates for how common a term is in the collection of documents (quotes). This would make the vector representation more accurately reflect the distinction of quotes. 
### Method applied 
We focused our quote similarity analysis, not on every quotes considered independently, but on the similarity between groups of quotes from a same politician. For doing this, we started by concatenating all quotes from a same politician and then vectorised this applying the TF-IDF method using stopwords. In order to decrease the number of features of our database, we then apply the NMF algorithm with an output in 50-dimensional space. We chose 50 as the number of dimensions since the topics created were rather interpretable; a larger number of dimensions would likely yield very specific topics, while low number of diemensions would yield topics that are less interpretable in the sense that they will include more words that may not be very related to eachother in reality. After considering DBSCAN and K-Means algorithm for clustering our data, we finally pick the second one with 14 clusters. When selecting the number of clusters we considered the silhouette score and Sum of Squared Error curves. We finally get clusters which can be represented by a politician and a set of heavily top words from a same topic.
Repeting the same procees for different years, we can establish some statistics on each cluster and so topic like the ratio female/male or the repartition in parties of politician from this cluster.  
## Proposed timeline
| Week |                        Task                        |
|:----:|----------------------------------------------------|
|  1   |                      Focus on HW2                          |
|  2   |                      Focus on HW2 / Start analyzing correlation between quotes from more than two politicians                          |
|  3   |   Continue analyzing correlation between quotes from more than two politicians     |
|  4   |   Add quote similarity to correlation analysis                           |
|  5   |   Work on the data story (deadline 17/12)          |
## Organization within the team
Kalle Bjurek, Clément Nicolle - Responsible for the quote content similarity part by writing the code and creating visualizations and discussion for the datastory.

Lucas Braz, Pierre-Alain Durand - Responsible for the second similarity measure; time of quoting. This includes writing all code relating to this similarity measure and the clustering of politicians using this measure. Also create visualizations and discussion in datastory.

## References
[1] Karl Aberer, Lecture notes from CS-423 Distributed Information Systems
