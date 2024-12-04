## Pipeline Overview
This project follows a structured pipeline for data and model processing:

1. **Data Ingestion:** Data is imported from `data/IMDB Dataset.csv`.
2. **Transformation:** Text reviews are tokenized and cleaned using spaCy.
3. **Training:** A BERT model is trained in `src/model_training.py` to classify sentiments.