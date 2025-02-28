# Quanteda Code Through 
**Michelle Hsu**  
*2025-02-20*

# Introduction: 

<p>
In this code-through, we are going to go more in depth about a package we used in Lab 04 called <code>quanteda</code>. 
<code>quanteda</code> is an R package that was made to help with textual analysis, data transformation, and organization.
</p>

<p>
To demonstrate how to use quanteda, we will be using the same data set from Lab 04. The data set has details about different non-profit organizations and initiatives as well as what communities they are helping or representing. From mission statements to organization names, the data set is mostly comprised of text or character strings which is a perfect candidate when it comes to demonstrating the uses of <code>quanteda</code>.
</p>

<p>
As with most packages, <code>quanteda</code> is often used alongside other packages such as  or  to help with the data processing and analysis. In this code-through, we will be using <code>dplyr</code>, <code>pander</code>, and <code>tidyverse</code> to help with formatting certain data sets and cleaning up the results of the coding chunks. 
</p>

---

### Loading the Packages and Data Set:

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

# Creating a Corpus and Using Tokens:

When working with the quanteda package, we need to create a series of called corpuses. A corpus allows 

### Example: 

In Lab 04 Part II, Question 1, we were asked to find the ten most frequently-used words in the missions statements after stemming the data. In order to do so, we had to organize the data such that 

First we need to load in the mission statements of the different non-profit organizations.

```r

dat$mission <- tolower( dat$mission )

```

Here is when we finally get to see how corpuses and tokens are used. Since we are using 

```r

corp <- corpus(dat, text_field = "mission")
corp <- corpus_trim(corp, what = "sentences", min_ntoken = 3)
tokens <- tokens(corp, what = "word", remove_punct = TRUE)
tokens <- tokens_remove(tokens, c(stopwords("english"), "nbsp"), padding = FALSE)

```

Organizing 

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

```r

tokens <- tokens_compound(tokens, pattern = my_dictionary)

```

```r

tokens_stemmed <- tokens_wordstem(tokens)

dfm_stemmed <- dfm(tokens_stemmed)

word_counts <- topfeatures(dfm_stemmed)

```

```r

pander(word_counts)

```

# Conclusion:

---

# References

This code-through references the following sources:

* Unknown (Unknown). RDocumentation. [RDocumentation](https://www.rdocumentation.org/packages/quanteda/versions/0.9.2-0/topics/corpus)

* Kenneth Benoit and Kohei Watanabe and Haiyan Wang and Paul Nulty and Adam Obeng and Stefan Müller and Akitaka Matsuo (2018) quanteda. [Data Imaginist](https://quanteda.io/articles/quickstart.html)