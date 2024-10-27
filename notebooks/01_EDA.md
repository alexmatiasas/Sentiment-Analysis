---
title: "Exploratory Data Analysis (EDA) Sentiment Analysis"
subtitle: "Data science portfolio"
author: "by: Manuel Alejandro Matías Astorga"
date: "2024"
output: 
  # prettydoc::html_pretty:
  html_document:  
    theme: yeti
    highlight: tango
    toc: true
    toc_depth: 3
    number_sections: true
    toc_float: true
    df_print: paged
    keep_md: true
---

# Preparation of the work environment

In this section, we will prepare our working environment by installing and loading the necessary libraries. Proper setup is essential for efficient data handling, visualization, and analysis in R. This step ensures that all the tools and libraries we need are readily available for our exploration and modeling tasks.

## Installing libraries

In this step, we install the necessary libraries for our analysis, including packages for data manipulation, visualization, text mining, and word clouds. Each package serves a unique purpose in the data science workflow, as detailed below:

-   **tidyverse**: For data manipulation and visualization.
-   **data.table**: Optimized for handling large datasets efficiently.
-   **ggplot2**: Essential for creating a wide variety of visualizations.
-   **dplyr**: Used for data wrangling and filtering.
-   **tm**: Text mining for natural language processing (NLP).
-   **wordcloud**: Generates word clouds for text data visualization.
-   **readr**: Efficient data reading.
-   **knitr**: Supports notebook creation and reporting.


``` r
# Install necessary libraries (only first time)
# install.packages(c("tidyverse", "data.table", "ggplot2", "dplyr", "tm", "wordcloud", "knitr", "tm"))
```

## Loading libraries

With the libraries installed, we now load them into our R environment. Each library plays a crucial role in the EDA and text mining process, as outlined below. Some libraries may output messages upon loading, so we suppress these to keep the notebook clean and readable.


``` r
# Load libraries for EDA and text mining
library(tidyverse)   # Data manipulation and visualization
library(data.table)  # Efficient data handling for large datasets
library(ggplot2)     # Data visualization
library(dplyr)       # Data wrangling
library(tm)          # Text mining for NLP tasks
library(wordcloud)   # Word cloud visualization
library(readr)       # Data reading
library(knitr)       # For creating notebooks     
```

After loading the libraries, we confirm that they have loaded successfully by displaying the versions of key libraries. This ensures compatibility and reproducibility of the analysis.


``` r
# Print version of main libraries
cat("Loaded tidyverse version:", as.character(packageVersion("tidyverse")), "\n")
```

```
Loaded tidyverse version: 2.0.0 
```

``` r
cat("Loaded data.table version:", as.character(packageVersion("data.table")), "\n")
```

```
Loaded data.table version: 1.16.2 
```

``` r
cat("Loaded ggplot2 version:", as.character(packageVersion("ggplot2")), "\n")
```

```
Loaded ggplot2 version: 3.5.1 
```

## Loading Data {.tabset .tabset-fade .tabset-pills}

### IMDB Dataset

The **IMDB Dataset** contains 50,000 movie reviews labeled with binary sentiments (positive or negative). This dataset is useful for natural language processing (NLP) tasks, particularly sentiment analysis. It includes: - **25,000 reviews for training** and **25,000 reviews for testing**, making it a benchmark dataset for sentiment classification tasks. - Each review is labeled with a **sentiment** value (positive or negative) in order to train and evaluate machine learning models.

The dataset was downloaded from the [Kaggle repository](https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews).

<dl>

<dt>

<h4>Dataset Structure</h4>

</dt>

<dd>

Each row in the dataset represents a movie review with two main columns:

-   **review**: The text of the movie review.
-   **sentiment**: The label for the sentiment, either "positive" or "negative".

**Example**:

| review | sentiment |
|------------------------------------|------------------------------------|
| "One of the other reviewers has mentioned that after watching just 1 Oz episode you'll be hooked." | positive |
| "A wonderful little production. The filming technique is very unassuming- very old-time-B..." | positive |

</dd>

</dl>

We will start by loading the dataset using the `read_csv` function from `readr`.


``` r
df <- read_csv("../data/IMDB Dataset.csv",)
```

<h3>Initial Inspection of Data</h3>

To better understand the dataset, we start by displaying the first few rows using `head(df)`. This gives us a quick overview of the text data format in the `review` column and the labels in the `sentiment` column.


``` r
# Display the first few rows of the dataset
head(df)
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["review"],"name":[1],"type":["chr"],"align":["left"]},{"label":["sentiment"],"name":[2],"type":["chr"],"align":["left"]}],"data":[{"1":"One of the other reviewers has mentioned that after watching just 1 Oz episode you'll be hooked. They are right, as this is exactly what happened with me.<br /><br />The first thing that struck me about Oz was its brutality and unflinching scenes of violence, which set in right from the word GO. Trust me, this is not a show for the faint hearted or timid. This show pulls no punches with regards to drugs, sex or violence. Its is hardcore, in the classic use of the word.<br /><br />It is called OZ as that is the nickname given to the Oswald Maximum Security State Penitentary. It focuses mainly on Emerald City, an experimental section of the prison where all the cells have glass fronts and face inwards, so privacy is not high on the agenda. Em City is home to many..Aryans, Muslims, gangstas, Latinos, Christians, Italians, Irish and more....so scuffles, death stares, dodgy dealings and shady agreements are never far away.<br /><br />I would say the main appeal of the show is due to the fact that it goes where other shows wouldn't dare. Forget pretty pictures painted for mainstream audiences, forget charm, forget romance...OZ doesn't mess around. The first episode I ever saw struck me as so nasty it was surreal, I couldn't say I was ready for it, but as I watched more, I developed a taste for Oz, and got accustomed to the high levels of graphic violence. Not just violence, but injustice (crooked guards who'll be sold out for a nickel, inmates who'll kill on order and get away with it, well mannered, middle class inmates being turned into prison bitches due to their lack of street skills or prison experience) Watching Oz, you may become comfortable with what is uncomfortable viewing....thats if you can get in touch with your darker side.","2":"positive"},{"1":"A wonderful little production. <br /><br />The filming technique is very unassuming- very old-time-BBC fashion and gives a comforting, and sometimes discomforting, sense of realism to the entire piece. <br /><br />The actors are extremely well chosen- Michael Sheen not only \"has got all the polari\" but he has all the voices down pat too! You can truly see the seamless editing guided by the references to Williams' diary entries, not only is it well worth the watching but it is a terrificly written and performed piece. A masterful production about one of the great master's of comedy and his life. <br /><br />The realism really comes home with the little things: the fantasy of the guard which, rather than use the traditional 'dream' techniques remains solid then disappears. It plays on our knowledge and our senses, particularly with the scenes concerning Orton and Halliwell and the sets (particularly of their flat with Halliwell's murals decorating every surface) are terribly well done.","2":"positive"},{"1":"I thought this was a wonderful way to spend time on a too hot summer weekend, sitting in the air conditioned theater and watching a light-hearted comedy. The plot is simplistic, but the dialogue is witty and the characters are likable (even the well bread suspected serial killer). While some may be disappointed when they realize this is not Match Point 2: Risk Addiction, I thought it was proof that Woody Allen is still fully in control of the style many of us have grown to love.<br /><br />This was the most I'd laughed at one of Woody's comedies in years (dare I say a decade?). While I've never been impressed with Scarlet Johanson, in this she managed to tone down her \"sexy\" image and jumped right into a average, but spirited young woman.<br /><br />This may not be the crown jewel of his career, but it was wittier than \"Devil Wears Prada\" and more interesting than \"Superman\" a great comedy to go see with friends.","2":"positive"},{"1":"Basically there's a family where a little boy (Jake) thinks there's a zombie in his closet & his parents are fighting all the time.<br /><br />This movie is slower than a soap opera... and suddenly, Jake decides to become Rambo and kill the zombie.<br /><br />OK, first of all when you're going to make a film you must Decide if its a thriller or a drama! As a drama the movie is watchable. Parents are divorcing & arguing like in real life. And then we have Jake with his closet which totally ruins all the film! I expected to see a BOOGEYMAN similar movie, and instead i watched a drama with some meaningless thriller spots.<br /><br />3 out of 10 just for the well playing parents & descent dialogs. As for the shots with Jake: just ignore them.","2":"negative"},{"1":"Petter Mattei's \"Love in the Time of Money\" is a visually stunning film to watch. Mr. Mattei offers us a vivid portrait about human relations. This is a movie that seems to be telling us what money, power and success do to people in the different situations we encounter. <br /><br />This being a variation on the Arthur Schnitzler's play about the same theme, the director transfers the action to the present time New York where all these different characters meet and connect. Each one is connected in one way, or another to the next person, but no one seems to know the previous point of contact. Stylishly, the film has a sophisticated luxurious look. We are taken to see how these people live and the world they live in their own habitat.<br /><br />The only thing one gets out of all these souls in the picture is the different stages of loneliness each one inhabits. A big city is not exactly the best place in which human relations find sincere fulfillment, as one discerns is the case with most of the people we encounter.<br /><br />The acting is good under Mr. Mattei's direction. Steve Buscemi, Rosario Dawson, Carol Kane, Michael Imperioli, Adrian Grenier, and the rest of the talented cast, make these characters come alive.<br /><br />We wish Mr. Mattei good luck and await anxiously for his next work.","2":"positive"},{"1":"Probably my all-time favorite movie, a story of selflessness, sacrifice and dedication to a noble cause, but it's not preachy or boring. It just never gets old, despite my having seen it some 15 or more times in the last 25 years. Paul Lukas' performance brings tears to my eyes, and Bette Davis, in one of her very few truly sympathetic roles, is a delight. The kids are, as grandma says, more like \"dressed-up midgets\" than children, but that only makes them more fun to watch. And the mother's slow awakening to what's happening in the world and under her own roof is believable and startling. If I had a dozen thumbs, they'd all be \"up\" for this movie.","2":"positive"}],"options":{"columns":{"min":{},"max":[2]},"rows":{"min":[6],"max":[6]},"pages":{}}}
  </script>
</div>

1. **Data Format**: 
  - The dataset is presented as a `tibble` with **6 rows** and **2 columns**: `review` and `sentiment.` Each row represents a single movie review and its corresponding sentiment label. 
2. **Columns**:
  - `review`: This column contains text data, where each entry is a movie review. Observations from these initial rows reveal: 
    - Reviews are written in **natural language**, exhibiting varied lengths and formats.
    - There is some **HTML-like syntax** present, such as <br />, which may require preprocessing to ensure text uniformity.
    - Reviews contain **punctuation** and **special characters** (e.g., "), which also suggests that further text processing steps, like removing or standardizing punctuation, could be beneficial. 
  - `sentiment`: This column contains categorical labels indicating the sentiment associated with each review:
    - “**positive**”: Suggests the review expresses a favorable opinion about the movie.
    - “**negative**”: Suggests the review expresses an unfavorable opinion. 
    - In this preview, 5 of the 6 reviews are labeled as “positive” and 1 as “negative”. However, the actual distribution will need to be confirmed in the full dataset.
3. **Observations on Sample Reviews**: 
  - From this small sample, we can observe the following patterns: 
    - Reviews labeled “positive” often use expressive language that conveys enjoyment or admiration. 
    - The presence of both “positive” and “negative” labels suggests a balanced dataset, although this balance will need to be validated in the following sentiment distribution analysis.
  - Based on these initial findings, preprocessing steps such as HTML tag removal, punctuation handling, and standardization of text may be necessary before proceeding with sentiment analysis.

This preliminary examination confirms that the dataset is suitable for a binary sentiment classification task, with review as the text input and sentiment as the target variable. Additionally, it highlights the need for cleaning and standardizing the text data as part of the preprocessing pipeline.

<h3>Structural Overview and Basic Descriptive Statistics</h3>

Let's take a look at the structure of the dataset, which provides an overview of the column types and a summary of the dataset's dimensions. This initial check also helps confirm that the `sentiment` column is a character variable, which we will later convert to binary format for statistical modeling.


``` r
# Check structure of the dataset
str(df)
```

```
spc_tbl_ [50,000 × 2] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ review   : chr [1:50000] "One of the other reviewers has mentioned that after watching just 1 Oz episode you'll be hooked. They are right"| __truncated__ "A wonderful little production. <br /><br />The filming technique is very unassuming- very old-time-BBC fashion "| __truncated__ "I thought this was a wonderful way to spend time on a too hot summer weekend, sitting in the air conditioned th"| __truncated__ "Basically there's a family where a little boy (Jake) thinks there's a zombie in his closet & his parents are fi"| __truncated__ ...
 $ sentiment: chr [1:50000] "positive" "positive" "positive" "negative" ...
 - attr(*, "spec")=
  .. cols(
  ..   review = col_character(),
  ..   sentiment = col_character()
  .. )
 - attr(*, "problems")=<externalptr> 
```

The output of `str(df)` reveals the following information about the dataset:

-   **Data Type**: The dataset is a `spec_tbl_df`, a specialized type of tibble provided by the readr package, which allows for efficient handling and display of large data.
-   **Dimensions**: The dataset contains **50,000** rows and **2 columns**.
-   **Columns**:
    -   `review`: This column is of type `character`, meaning it contains text data. Each entry in this column represents a movie review.
    -   `sentiment`: This column is also of type `character.` It contains the sentiment label for each review, either “positive” or “negative”.
-   **Attributes**:
    -   `spec`: This attribute indicates that `review` and `sentiment` are recognized as `character` columns, which aligns with our expectations.
    -   `problems`: An attribute that detects any potential parsing issues when loading the data. Since it’s empty, no parsing issues were encountered.

This structure confirms that the dataset is suitable for a sentiment analysis task, where `review` serves as the input text and sentiment as the binary target variable.


``` r
# Check summary of the dataset
summary(df)
```

```
    review           sentiment        
 Length:50000       Length:50000      
 Class :character   Class :character  
 Mode  :character   Mode  :character  
```

Since both `review` and `sentiment` are character columns, `summary(df)` provides basic information on their lengths and classes. Here’s a breakdown:

-   `review`: Contains 50,000 entries of type `character`, as expected for text data. This confirms that each entry represents an individual review without any missing values.
-   `sentiment`: Also contains 50,000 entries of type `character.` Since it aligns with the dataset documentation, this suggests there are no missing labels for sentiment.

The absence of missing values indicates that the dataset is complete and does not require any initial imputation.

### 2nd Dataset

### 3rd dataset

# Sentiment distribution visualization

In this section, we will analyze the distribution of the `sentiment` variable in the IMDB dataset. Understanding the distribution of sentiment is essential for assessing class balance, which can impact model performance. Additionally, we will examine basic descriptive statistics and structural information to ensure that the data aligns with our modeling requirements.

## Exploration of the structure and descriptive statistics

We start by examining the structure of the dataset using `str()`, which provides an overview of column names, types, and data samples. Additionally, we use `summary()` to obtain descriptive statistics, allowing us to identify any potential issues, such as missing values or anomalies.


``` r
str(df)
```

```
## spc_tbl_ [50,000 × 2] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
##  $ review   : chr [1:50000] "One of the other reviewers has mentioned that after watching just 1 Oz episode you'll be hooked. They are right"| __truncated__ "A wonderful little production. <br /><br />The filming technique is very unassuming- very old-time-BBC fashion "| __truncated__ "I thought this was a wonderful way to spend time on a too hot summer weekend, sitting in the air conditioned th"| __truncated__ "Basically there's a family where a little boy (Jake) thinks there's a zombie in his closet & his parents are fi"| __truncated__ ...
##  $ sentiment: chr [1:50000] "positive" "positive" "positive" "negative" ...
##  - attr(*, "spec")=
##   .. cols(
##   ..   review = col_character(),
##   ..   sentiment = col_character()
##   .. )
##  - attr(*, "problems")=<externalptr>
```

``` r
summary(df)
```

```
##     review           sentiment        
##  Length:50000       Length:50000      
##  Class :character   Class :character  
##  Mode  :character   Mode  :character
```

The structure output confirms that the dataset consists of 50,000 rows and 2 columns: review and sentiment. The review column is of type character, containing text data, while the sentiment column is a factor, with levels “positive” and “negative”. This aligns with our expectation for binary sentiment classification.

## Visualization of the sentiment distribution

To understand the distribution of sentiments, we use a bar plot to visualize the count of each sentiment class. This visualization allows us to quickly assess whether the dataset is balanced, which is important for building an effective classification model.


``` r
library(ggplot2)
ggplot(df, aes(x = sentiment)) +
  geom_bar(fill = "skyblue", color = "black") +
  geom_text(stat='count', aes(label=..count..), vjust=2) +
  ggtitle("Sentiment Distribution") +
  xlab("Sentiment") +
  ylab("") +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, size = 15, face = "bold"),
    axis.title.x = element_text(size = 12),
    axis.title.y = element_text(size = 12)
  )
```

![](01_EDA_files/figure-html/plotting sentiment distribution-1.png)<!-- -->

From the plot, we observe that the dataset has equal number of positive and negative reviews. This balance is ideal for our sentiment analysis model, as it reduces the risk of bias toward one class during training.

## Analysis of the review length

Adding a new column of each review and visualizing their distribution


``` r
library(dplyr)
df <- df %>%
  mutate(review_length = nchar(review))

ggplot(df, aes(x = review_length)) +
  geom_histogram(binwidth = 10, fill = "steelblue") +
  ggtitle("Lenght Distribution of reviews") +
  xlab("Lenght of the reviews") +
  ylab("") +
  theme_minimal() +
  theme(
    plot.title = element_text(hjust = 0.5, size = 15, face = "bold"),
    axis.title.x = element_text(size = 12),
    axis.title.y = element_text(size = 12)
  )
```

![](01_EDA_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

## Frecuent words and wordcloud

Using the library `tm` for the preprocessing of the text and generate a wordcloud.


``` r
library(tm)
library(wordcloud)
library(RColorBrewer)

# Crear un Corpus
corpus <- Corpus(VectorSource(df$review))
corpus <- tm_map(corpus, content_transformer(tolower))
corpus <- tm_map(corpus, removePunctuation)
corpus <- tm_map(corpus, removeWords, stopwords("english"))

# Generar la Nube de Palabras
set.seed(1234) # Para reproducibilidad
wordcloud(corpus, 
          max.words = 100, 
          random.order = FALSE, 
          colors = brewer.pal(8, "Paired"),
          scale = c(3, 0.5),
          rot.per = 0.35, 
          use.r.layout = FALSE)

# Añadir título y fondo claro
title("Word Cloud of Sentiments", col.main = "black", font.main = 4)
```

![](01_EDA_files/figure-html/frecuent words and wordcloud-1.png)<!-- -->

``` r
# Mostrar leyenda (opcional)
# legend("topright", legend = "Most frequent words", col = "blue", cex = 0.8, text.col = "blue")
```

# Statistical analysis and modeling

## Statistical tests

Perdorming statistical tests


``` r
t.test(review_length ~ sentiment, data = df)
```

```
## 
## 	Welch Two Sample t-test
## 
## data:  review_length by sentiment
## t = -3.4721, df = 49627, p-value = 0.0005168
## alternative hypothesis: true difference in means between group negative and group positive is not equal to 0
## 95 percent confidence interval:
##  -48.08220 -13.38444
## sample estimates:
## mean in group negative mean in group positive 
##               1294.064               1324.798
```

## Basic statistical model

Exploring some initial models.


``` r
# Verificar los valores únicos en sentiment
unique(df$sentiment)
```

```
## [1] "positive" "negative"
```

``` r
# Convertir sentiment a una variable binaria
df$sentiment_binary <- ifelse(df$sentiment == "positive", 1, 0)

# Crear la columna review_length con la longitud de cada reseña
df$review_length <- nchar(df$review)

# Ajustar el modelo logístico
glm_model <- glm(sentiment_binary ~ review_length, data = df, family = "binomial")
summary(glm_model)
```

```
## 
## Call:
## glm(formula = sentiment_binary ~ review_length, family = "binomial", 
##     data = df)
## 
## Coefficients:
##                 Estimate Std. Error z value Pr(>|z|)    
## (Intercept)   -4.111e-02  1.484e-02   -2.77  0.00561 ** 
## review_length  3.140e-05  9.048e-06    3.47  0.00052 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## (Dispersion parameter for binomial family taken to be 1)
## 
##     Null deviance: 69315  on 49999  degrees of freedom
## Residual deviance: 69303  on 49998  degrees of freedom
## AIC: 69307
## 
## Number of Fisher Scoring iterations: 3
```

``` r
#summary(glm_model)
```
