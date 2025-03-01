# Quanteda Code Through 
### <i>Module 7</i>
**Michelle Hsu**  
*2025-02-20*

# Introduction: 

<p> In this code-through, we are going to go more in depth about a package we 
used in Lab 04 called <code>quanteda</code>. <code>quanteda</code> is an R package 
that was made to help with textual analysis, data transformation, 
and organization. </p>

<p> To demonstrate how to use quanteda, we will be using the same data set from 
Lab 04. The data set has details about different non-profit organizations and 
initiatives as well as what communities they are helping or representing. From 
mission statements to organization names, the data set is mostly comprised of 
text or character strings which is a perfect candidate when it comes to 
demonstrating the uses of <code>quanteda</code>. </p>

---

### Loading the Packages and Data Set:

<p> As with most packages, <code>quanteda</code> is often used alongside other packages 
such as  or  to help with the data processing and analysis. In this code-through, 
we will be using <code>dplyr</code>, <code>pander</code>, and <code>tidyverse</code> 
to help with formatting certain data sets and cleaning up the results of the 
coding chunks. </p>

```r

library (quanteda)
library (dplyr)
library (tidyverse)
library (pander)

```

<p> </p>

```r

URL <- "https://github.com/DS4PS/cpp-527-spr-2020/blob/master/labs/data/IRS-1023-EZ-MISSIONS.rds?raw=true"
dat <- readRDS(gzcon(url( URL )))

```

### Creating a Corpus and Using Tokens:

<p> When working with the quanteda package, we need to create a series of called corpuses. 
A corpus allows </p>

<p> Alongside corpuses, we also use tokens. Tokens help with </p>

#### Example: 

In Lab 04 Part II, Question 1, we were asked to find the ten most frequently-
used words in the missions statements after stemming the data. In order to do so, 
we had to organize the data such that 

First we need to load in the mission statements of the different non-profit organizations.

```r

dat$mission <- tolower( dat$mission )

```

Here is when we finally get to see how corpuses and tokens are used. Since we are
Looking for 

```r

corp <- corpus(dat, text_field = "mission")
corp <- corpus_trim(corp, what = "sentences", min_ntoken = 3)
tokens <- tokens(corp, what = "word", remove_punct = TRUE)
tokens <- tokens_remove(tokens, c(stopwords("english"), "nbsp"), padding = FALSE)

```

 

```r

my_dictionary <- dictionary(list(
    five01_c_3 = c("501 c 3", "section 501 c 3"),
    united_states = c("united states"),
    high_school = c("high school"),
    non_profit = c("non-profit", "non profit"),
    stem = c("science technology engineering math", "science technology engineering mathematics"),
    los_angeles = c("los angeles"),
    ny_state = c("new york state"),
    ny = c("new york"),
    healthcare = c("health care", "healthcare"),
    community_service = c("community service"),
    charitable_giving = c("charitable giving")
))

```
Here we are using the <code>tokens_compound()</code> function to take the tokens we
created in the dictionary above and further filter the <code>dat$mission</code> data set 
to fit the criteria we are looking for. 

```r

tokens <- tokens_compound(tokens, pattern = my_dictionary)

```

Additionally, we are using the <code>dfm()</code> function in order to create a data frame of
our organized data we just filtered through and <code>tokens_wordstem()</code> to find words that
start with similar roots or phrases and add them to the count of how many instances of that word exist.

The function <code>topfeatures()</code> highlight the top ten instances of a variable. In this case,
it is highlighting the top ten instances of different stemmed words.

<i> An Aside: We might have the words purpose, purposed, purposing, etc. but they all start with the same
stem of <b>purpos</b>. Thus, instead of categorizing them into their own individual category, they are categorized
under <b>purpos</b>.</i>

```r

tokens_stemmed <- tokens_wordstem(tokens)

dfm_stemmed <- dfm(tokens_stemmed)

word_counts <- topfeatures(dfm_stemmed)

```
Now that everything is organized and filtered properly, once we use <code>pander</code>
to make the table look more organized: 

```r

pander(word_counts)

```
And thus, our final results should look something like this: 

![]({{site.url}}/assets/img/final-table.png)  

# Conclusion:

<p> <code>quanteda</code> can be used for a variety of different text related contexts,
whether for textual analysis or data organization. When used alongside different packages, 
we can create organized and clean data sets to present to different audiences, whether
it be fellow coders or to the general public. Thus, <code>quanteda</code> is a wonderfully
helpful package in regards to work flow, presentation, and organization. </p>

---

# References

This code-through references the following sources:

* Unknown (Unknown). RDocumentation. 
[RDocumentation](https://www.rdocumentation.org/packages/quanteda/versions/0.9.2-0/topics/corpus)

* Kenneth Benoit and Kohei Watanabe and Haiyan Wang and Paul Nulty and Adam Obeng a
nd Stefan Müller and Akitaka Matsuo (2018) quanteda. 
[Quanteda](https://quanteda.io/articles/quickstart.html)