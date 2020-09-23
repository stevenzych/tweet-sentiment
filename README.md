# *Tweet Sentiment: Assessing User Emotion With Natural Language Processing*
**Steven Zych - September 2020**

# Introduction

This project looks at **user emotion** when tweeting at and about Google, Apple, and their respective products. I have trained a **neural network** on data sourced from [data.world](https://data.world/crowdflower/brands-and-product-emotions) that contains just under **9,000** tweets with three many sentiment classes: **Positive, negative, and no emotion.** 

In short, this is a **ternary** classification problem, that uses cleaned, tokenized, and vectorized words/tweets to make predictions. **The goal is to help the tech companies in question better assess public opinion, and offer insights on how to use this neural network.**

All in all, the following packages were used:
- NLTK
- Keras
- Re
- String
- Collections
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-Learn

# EDA and Cleaning 

## General Cleaning

The cleaning on this data was exceptionally straightforward. Tweets were stripped of the following:
- Usernames following an @
- URL's and web addresses
- Hashtags
- English stopwords as provided by the NLTK corpus
- Punctuation marks

After that, the tweets were tokenized, which is to say that they were split into individual pieces wherever a space occurred. This left us with each tweet having been transformed into a new list of individual word "tokens." As an example, here is one tweet from each sentiment class in both its original and cleaned-tokenized form:

![Tweet Token Example](/images/tweet_examples.PNG)

Beyond this, the only cleaning necessary was to drop a superfluous column, labelled `directed_at` (which was only used in EDA, not modelling), to drop any tweet whose sentiment was labelled as `"I can't tell"` (which is just as good as having *no* label), and to remove one solitary tweet entry that contained no information.

## EDA Insights

Just to get a general idea of the data, I've prepared a countplot showing the distribution of tweets in the dataset based on their labeled sentiment:

![Tweets By Sentiment](/images/count_sentiment.png)

As is evidenced here, tweets with `"No emotion toward brand or product"` are the **majority class** in the data. Among the polar emotional classes, `"Positive"` was the majority by a longshot. It's worth noting that having lots of neutral tweets *isn't necessarily a bad thing,* or bloated data. As a matter of fact, it can provide a helpful **baseline** that more emotionally-charged tweets can be measured against.

We can add another layer of complexity to this plot but including which brands or services these tweets were **directed at:**

![Tweets By Sentiment By Brand](/images/count_brand.png)

We can see hear that while neutral tweets dominate in sheer number, they are actually the least prevalent when it comes to tweets with a clear **direction,** as opposed to just tweets that were pulled and/or more generally part of the digital conversation. I did not have time to explore this fully in this project, but it is an interesting angle to explore in further research.

# Machine Learning Models

## Baseline Model

The first model I made took the following structure:

![Baseline Neural Net](/images/model_1_arch.PNG)

In short, the reasoning behind it is this:
- Sequential model to start, since this is a multi-layer neural network
- Embedding layer with an input size equal to the total vocabulary
- Long Short Term Memory layer to handle sequential data (i.e., words in order from a tweet)
- Alternating Dropout and Dense layers to try not to overfit the model
- Three nodes in the output layer for our three classes

Unfortunately, the model overfit aggressively, as such:

![Baseline Neural Net Score](/images/model_1.PNG)

The **training accuracy** was about **95%,** but it left the **validation accuracy** in the dust at a lowly **67%.** Numerous changes were made to address this. Different amounts and sizes of Dense layers were implemented, as were different rates of Dropout. L2 regularization was applied to some or all of the Dense layers. Different training-validation splits were tried out. All of this--or at least most of it--was to no avail, and I ended up largely unsuccesful until I **switched optimizers.**

But first, here's an example of one of the **interim models:**

![Interim Neural Net](/images/model_6_arch.PNG)

And it's performance, which is visibly quite like the first:

![Interim Neural Net Score](/images/model_6.PNG)

## Final Model

Switching optimizers--from Adam to RMSProp--proved to be the most effective change in modelling. With this change in place, as well as some of the tweaks tested out in the interim models, I ended up with this **final neural network architecture:**

![Final Neural Net](/images/model_7_arch.PNG)

Now, the previous scoring plots showed models with *significantly* better training performances than this one. It was a tradeoff in the end. The only way I could get this model to come remotely close to converging better was to offer up about a 10% change in both accuracies, showing a final **85%** training accuracy and **71%** validation accuracy:

![Final Neural Net Performance](/images/model_7.PNG)

# Conclusions

## Recommendations

While the scope of this project clearly stands to be expanded (Apple and Google need a lot more than 9,000 to go off of), there are some key insights I'd like to bring to the table:
1. **Filter negative tweets out by sentiment and "put out fires" fast.** Negative public opinion is more of a cautious case than positive, obviously, and it's important to have customer service reps use this neural net to find and address key complaints quickly.
1. **Use sentiment filtering for market research** on what product features cause the **most positive** and **most negative** public outcry.
1. **Use the model as a general barometer** of public opinion.
1. **Find positive-sentiment tweets from popular accounts** and reach out about sponsorship, collaboration, and general brand ambassador business.

## Future Research

I come from a **linguistics background,** and there's a few things I would have loved to get into if give the time and/or resources. I certainly will be focusing on these four points in the future:
1. **This model didn't account for emojis, emoticons, or punctuation** as displays of affect/emotion. Emojis and old-school emoticons are obviously emotionally charged, but so are granular displays of punctuation, such as period in/exclusion. Removing punctuation in this case helped streamline the process, but keeping it in *in set cases* could be a key predictor in the future.
1. More generally, I want to investigate the **relationship between tweet sentiment and retweet/reply counts.** Do angry tweets get passed on more often? If so, is there anything that can be done from a consumer outreach angle about this?
1. **Not everyone speaks the same English.** Academic English, like I'm using now, is different than how I'd talk to my friends. And that English (which is really just a change in register) is wildly different than the way people speak English in North Ireland or Singapore. The point here is that having a dataset which just includes "English tweets" does a **disservice** both to the speakers of this variations, as well as to us as language researchers and data scientists. It also **introduces variation,** or noise, that occurs when **inconsistent meanings and word usages rear their heads across cultural boundaries.** Building a model that addresses this (or even just sourcing data in a more controlled manner) would be a dream.

## Thank You

I would like to thank Flatiron School, my class cohort, and the good folks at Apple and Google for extending to me their time and trust.