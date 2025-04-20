# 🎭 Sentiment Analysis on IMDb Reviews

This project applies Natural Language Processing (NLP) techniques to classify movie reviews from IMDb into **positive** or **negative** sentiment categories. It follows a two-phase approach:

- 🔍 **Phase 1**: Text preprocessing and exploratory analysis using **R and tidyverse** libraries.
- 🤖 **Phase 2** _(in progress)_: Sentiment classification models in **Python** using Scikit-learn and PyTorch.

## 📁 Dataset

- [IMDb Reviews Dataset (Kaggle)](https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews)
- 50,000 reviews labeled as **positive** or **negative**.

## 📌 Project Phases

### 🧪 Phase 1: Text Processing in R

- Text cleaning: lowercasing, stopword removal, HTML cleanup
- Tokenization and lemmatization
- POS tagging with `udpipe`
- Visualization: word clouds, bar charts, n-gram analysis
- 📈 [Full EDA Notebook on RPubs](LINK_AQUI)

### 🧠 Phase 2: Modeling in Python _(Coming Soon)_

- Data exported as `IMDB-cleaned.csv`
- Model candidates:
  - Logistic Regression
  - Naive Bayes
  - Support Vector Machines
  - PyTorch-based classifier
- Metrics: Accuracy, F1-score, ROC-AUC

## 📦 Deliverables

- `sentiment-analysis.Rmd`: Full EDA notebook in R
- `IMDB-cleaned.csv`: Preprocessed dataset
- `model_sentiment.py`: Sentiment classification model (planned)
- Streamlit / Flask deployment (planned)

## 🚀 Deployment Ideas

- Build an interactive dashboard (Streamlit)
- Deploy a REST API using FastAPI
- (Optional) Real-time batch sentiment processing with Apache Spark

## 📚 Skills & Tools Used

- R: `tidyverse`, `tidytext`, `udpipe`, `ggplot2`, `SnowballC`
- Python: `scikit-learn`, `NLTK`, `PyTorch` (planned)
- EDA & Reporting: R Markdown, RPubs

## 🧠 Author

**Manuel Alejandro Matías Astorga**  
Data Scientist | Physicist | Machine Learning Enthusiast  
📄 [Portfolio Website](https://alexmatiasas.github.io) · [LinkedIn](https://linkedin.com/in/alexmatiasastorga)

---

> ✨ Feel free to fork, contribute or reach out if you're working on similar projects!