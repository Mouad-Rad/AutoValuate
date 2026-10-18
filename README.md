# AutoValuate

Car price prediction for the Moroccan used-car market. You fill in a car's details (brand, model, year, mileage, etc.) and the app returns an estimated price based on a model trained on listings scraped from Avito.ma.

Live version: https://autovaluate.vercel.app

## Stack

- Frontend: React, Axios
- Backend: Flask, Flask-CORS
- ML: Scikit-learn, XGBoost, Pandas, NumPy, Joblib
- Scraping: Requests, BeautifulSoup

## Project structure

    app/        Flask API (api.py)
    client/     React frontend
    data/       raw scraped data (car_details.csv)
    src/        scraper, notebooks, trained model (car_price_model.pkl)
    report/     project report

## How it works

The data comes from `src/scraping.py`, which scrapes used-car listings from Avito.ma. The notebooks in `src/` clean the data and compare a few regression models (Linear Regression, Random Forest, XGBoost) with GridSearchCV. The trained pipeline is saved with Joblib and loaded by the Flask API, which exposes a single `/predict` endpoint. The React app sends the form values to that endpoint and shows the result.

The model expects these features: Marque, Modèle, Année, Type de carburant, Puissance fiscale, Kilométrage, Nombre de portes, Première main, État, Boîte à vitesses, Origine.

## Running locally

Backend:

    pip install -r requirements.txt
    cd app
    python api.py

The API starts on http://127.0.0.1:5000. Note that `api.py` currently loads the model from an absolute Windows path, so change that line to point to `src/car_price_model.pkl` on your machine first.

Frontend:

    cd client
    npm install
    npm start

The app opens on http://localhost:3000.

## API

`POST /predict` with a JSON body containing the features listed above:

    {
      "Marque": "Dacia",
      "Modèle": "Logan",
      "Année": 2018,
      "Type de carburant": "Diesel",
      "Puissance fiscale": 6,
      "Kilométrage": 120000,
      "Nombre de portes": 5,
      "Première main": "Oui",
      "État": "Bon",
      "Boîte à vitesses": "Manuelle",
      "Origine": "WW au Maroc"
    }

Returns:

    { "predicted_price": 95000.0 }

If a feature is missing the API returns a 400 with the list of missing fields.

## Author

Mouad Rad
