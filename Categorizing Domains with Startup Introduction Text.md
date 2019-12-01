# Categorizing Domains with Startup Introduction Text
## There's a spy between us!

* The original article was published in https://brunch.co.kr/@cloud09/321. This is a translated version.

I was very curious when I saw the data two to three months ago. Startup is categorized by industry, but there is something that is not understandable. Smile, cleaning laboratory, WAHOME, and bucket plates are tied in the name of home service. Home is home, but the first three companies are related to home, and the second one is related to interior design and home furnishings. I was wondering what would happen if the machines were divided if they had been grouped differently (although it was for some reason, of course). So I tried.

![Korean Startup over $10M](/korean_startup.png)

This is how the process works.

> 1. Planning the analysis method
> 2. Collecting data
> 3. Text pre-treatment
> 4. Categorizing
> 5. Check results
> 6. Visualization

1. Planning the analysis method

The intention was to have learning data grouped without giving it to them. And we found out how to collect the data. I thought about what data each startup can use, but I thought I could use it if I have an explanation. 

2. Collecting data

A single startup landing site has collected a list of companies, brief descriptions, and detailed explanations. They used selenium and beautifulsoup. A total of 70,539 cases were obtained as a result, but there were only 1,328 cases where there were only 22 % of the total. Even so, the simplest explanation averaged 32 characters, less than about 1.5 sentences, assuming that one sentence was about 25 years old. On the other hand, the state was in an average of 501 characters when detailed explanations were available, but the figure was very low at 5,653. But what I wanted was not to classify every startup in the country, and I decided to continue because companies with detailed explanations were likely to continue to operate and become meaningful.



3. Text pre-treatment

The process of refining collected text.

3.1 Deleted useless punctuation and url pattern. The reason for deleting the url pattern is that it often has links to the company's site in detail.

3.2 I have preregistered the words that fit the domain. The elegant brothers were not allowed to be separated into two brothers, so they went through the process of registering themselves as a proper noun.

3.3 Non-using words (meaningless words) have been excluded. As it is also a business introduction column and it is a red flag, words such as ' Startup ', ' Investment ', ' Innovation ', ' Cumulative ', ' Competition ' and so on are really numerous and have been deleted because they are not helpful in identifying the domain.

3.4 Only nouns were left based on the process of 1) to 3). This is because nouns alone were not difficult to grasp, and verbs made by combining with nouns were well understood because nouns remain.

I think it's very clear to write it like this, but in reality, we've clustered it, and if the results aren't good enough, we've gone back and forth through the process of n times (let's put in the numbers you want to imagine).

4. Clustering
   
The method of classification has two attempts, K-means clustering and LDA. However, LDA did not produce a clearer result than expected. 

![lda](/lda.png)

On the other hand, when trying K-means, the words were well bound up with much more relevant things.

![kmeans](/kmeans.png)

So I ended up with K-means. The difference between LDA and K-means is the assumption about the number of topics in a document. Below is a description of the difference between LDA and K-means.

> K-means can also be used as a topic modeling in fact. In fact, the difference between topic modeling such as LDA and K-means is the assumption about the number of topics in a document. LDA can have many kinds of topics in a document. Assumes, but K-means assumes that a single document is a topic. This assumption is rather a useful prior in some sets of documents. There is a topic in one article mainly in the news. On the other hand, a review of a movie from multiple perspectives can be a mixture of topics. If we assume that there is a single topic in a document, we don't necessarily need to use the over-specific LDA.

Source : https://lovit.github.io/nlp/2018/09/27/pyldavis_kmeans/


The results of K-means seem to have come out more neatly because the data I gathered were better suited to assume a single topic than to assume multiple topics. The corporate profile itself is not written from multiple perspectives.



1. Check results

So 14 classifications have been given as follows : The title below the cluster number is what I named after the bundle of words per cluster.


![cluster_result1](/cluster1.png)

![cluster_result2](/cluster2.png)

![cluster_result3](/cluster3.png)

For your information, Bucket Place, which was classified as home service in Startup Alliance, was classified as an interior cluster. The companies in the interior cluster outside of Bucket Place included the house floor, interior gentleman, space leven and so on. On the other hand, there are no clusters of domestic services such as smiles and cleaning laboratories, which do not appear to be formed because there are not many domestic service companies to form a cluster and because the introduction of the company does not reveal the domestic service. (There were not nearly 5600 periods of data - driven or matching algorithms.) To avoid increasing the number of clusters, the silhouettes index was the highest but smaller, but we did not check by increasing the number of clusters. When checking the results one by one, they are not completely classified, but if it is really important to categorize them well, it would be better to define the candidate group based on the results (after human modification).

6. Visualization

Visualizations were carried out in both cluster and document units. Usually, Pyldavis seems to be used a lot when you model topics, but the reason why I did both is because I wondered how it would appear by document. Pyldavis is the visualization of cluster units by comparing the results of LDA and K-means above. To be exact, I used kmeans_to_piLDAvis, but it was already convenient to use because it was well built. (Thank you.)
Visualization of the document unit was reduced to t-sne, creating plots as a board. If each point represents a company and has the same color, it belongs to the same category. Hover over to see which entities fall into which category. If you want to check it out for yourself, I have a file [here](https://github.com/dearcloud09/startup_clustuering/blob/master/t-sne%20vis.html).


![bokeh](/bokeh.png)

To conclude, I thought about this toy project again and I had fun. And I want to cherish the feeling of wanting to know what I am (out of the blue. I think it's a good idea to take care of things because they often shrink after they look like their own. I'll end with a meme that I had a lot of empathy with doing the Toy project.

![meme](/just_do_it.jpeg)

> Why the hell do you do that?
> 
>  Damn it, I don't need a reason to play the game! Just do it!