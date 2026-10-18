# AutoValuate

AutoValuate is a car price prediction application for the Moroccan used-car market. The user enters the characteristics of a vehicle through a web form, and the app returns an estimated price using a machine learning model trained on real listings from Moroccan websites.

The project runs locally and is made of three parts: a scraper that builds the dataset, a notebook pipeline that cleans the data and trains the model, and a web app (React + Flask) that serves predictions.

## Technologies

- Frontend: React, Axios
- Backend: Flask, Flask-CORS
- Machine learning: Scikit-learn, XGBoost, Pandas, NumPy, Joblib
- Data collection: Requests, BeautifulSoup
- Analysis: Jupyter, Matplotlib, Seaborn

## Project structure

```
Pfm-ML/
  app/            Flask API (api.py)
  client/         React frontend (form + result components)
  data/           raw scraped data
  src/            scraper, notebooks, cleaned dataset, trained model
  report/         project report
  requirements.txt
```

## Dataset

The data was scraped from Avito.ma with `src/scraping.py`, which collects used-car listings page by page using multiple threads. The raw file is stored in `data/raw/car_details.csv`, and the cleaned version used for training is `src/cleaned_data.csv`.

Each listing is described by:

- Marque (brand)
- Modèle
- Année
- Type de carburant
- Puissance fiscale
- Kilométrage
- Nombre de portes
- Première main
- État
- Boîte à vitesses
- Origine

The target is the listing price.

## Model

The cleaning and modeling work is in `src/preprocessing.ipynb` and `src/clean_code.ipynb`. It covers missing values, outliers, encoding of categorical variables and scaling, all wrapped in Scikit-learn pipelines. Three regressors were compared: Linear Regression, Random Forest and XGBoost, with hyperparameters tuned using GridSearchCV and evaluated with R², MAE and RMSE. The final model is saved with Joblib as `src/car_price_model.pkl`.

## Installation

Clone the repository:

```
git clone https://github.com/Mouad-Rad/Pfm-ML.git
cd Pfm-ML
```

Backend:

```
python -m venv venv
venv\Scripts\activate          (Linux/Mac: source venv/bin/activate)
pip install -r requirements.txt
```

In `app/api.py`, the model is loaded with an absolute path. Update it so it points to `src/car_price_model.pkl` on your machine, then start the API:

```
cd app
python api.py
```

The API runs on `http://127.0.0.1:5000`.

Frontend:

```
cd client
npm install
npm start
```

The app opens on `http://localhost:3000`.

## Usage

Open the app, fill in the vehicle details (brand, model, year, fuel type, mileage, and so on) and submit the form. The estimated price is displayed right below.

The API can also be called directly:

```
POST /predict
Content-Type: application/json
```

The body must contain all eleven features listed in the dataset section. The response looks like this:

```
{ "predicted_price": 95000.0 }
```

If a feature is missing, the API answers with a 400 error listing the missing fields.

## Author

Mouad Rad
