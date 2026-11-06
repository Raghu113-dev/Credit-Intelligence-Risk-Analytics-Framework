# Credit Intelligence & Risk Analytics Framework

An end-to-end credit risk assessment application that uses a trained machine learning model to estimate a loan applicant's probability of default and classify the application as **High Risk** or **Low Risk**.

The project includes a browser-based interface, a FastAPI prediction service, and a saved model with a selected classification threshold.

## Overview

Credit risk assessment helps lenders evaluate the likelihood that a borrower may fail to repay a loan. This project accepts applicant and loan attributes, sends them to a prediction API, and returns a default probability with a risk classification.

### Key capabilities

- **Applicant-level prediction:** accepts demographic, employment, loan, and credit-history attributes.
- **Default probability:** returns the model's estimated probability of default.
- **Threshold-based classification:** compares the probability against a saved decision threshold.
- **Risk interpretation:** reports the result as High Risk or Low Risk.
- **Web interface:** provides a front end served from the application's `static/` directory.
- **API backend:** exposes a FastAPI `POST /predict` endpoint with Pydantic input validation.

## Technology Stack

| Area | Technology |
|---|---|
| Programming language | Python |
| API framework | FastAPI |
| Data handling | Pandas |
| Model persistence | Joblib |
| Input validation | Pydantic |
| Machine learning | Scikit-learn, XGBoost |
| Front end | HTML, CSS, JavaScript |
| ASGI server | Uvicorn |
| Deployment configuration | Render |

## How It Works

1. The user enters loan applicant information through the web interface.
2. The interface submits the applicant data to the `/predict` API endpoint.
3. FastAPI validates the request using the `LoanApplication` Pydantic model.
4. The backend converts the validated input into a Pandas DataFrame.
5. The saved model calculates the probability of default.
6. The probability is compared with the threshold loaded from `best_threshold.pkl`.
7. The API returns the probability, threshold, binary prediction, and risk label.

## Input Features

The prediction API expects the following fields:

| Feature | Description |
|---|---|
| `person_age` | Applicant's age |
| `person_income` | Applicant's income |
| `person_home_ownership` | Home ownership category |
| `person_emp_length` | Employment length |
| `loan_intent` | Purpose of the loan |
| `loan_grade` | Loan grade |
| `loan_amnt` | Requested loan amount |
| `loan_int_rate` | Loan interest rate |
| `loan_percent_income` | Loan amount as a proportion of income |
| `cb_person_default_on_file` | Historical default indicator |
| `cb_person_cred_hist_length` | Length of credit history |

Use the categorical values and formats expected by the model's training pipeline.

## API Reference

### `POST /predict`

Submit a JSON loan application to receive a risk prediction.

**Example request**

```json
{
  "person_age": 28,
  "person_income": 60000,
  "person_home_ownership": "RENT",
  "person_emp_length": 4.0,
  "loan_intent": "PERSONAL",
  "loan_grade": "B",
  "loan_amnt": 10000,
  "loan_int_rate": 11.5,
  "loan_percent_income": 0.17,
  "cb_person_default_on_file": "N",
  "cb_person_cred_hist_length": 5
}
```

**Example response shape**

```json
{
  "default_probability": 0.18,
  "default_prediction": 0,
  "threshold": 0.35,
  "Result": "Low Risk"
}
```

The values above illustrate the response format only; they are not a measured project result. The actual probability and threshold are produced by the loaded model and threshold file.

## Project Structure

```text
Credit-Intelligence-Risk-Analytics-Framework/
├── Credit_Risk.ipynb          # Model development and analysis notebook
├── credit_risk_dataset.csv    # Credit risk dataset
├── credit_risk_model.pkl      # Serialized trained model
├── best_threshold.pkl        # Selected classification threshold
├── main.py                    # FastAPI application and prediction endpoint
├── requirements.txt           # Python dependencies
├── runtime.txt                # Python runtime configuration
├── render.yaml                 # Render deployment configuration
└── static/
    ├── index.html              # Web application interface
    ├── script (1).js           # Client-side interactions
    └── style.css               # Interface styling
```

## Run Locally

### 1. Clone the repository

```bash
git clone https://github.com/Raghu113-dev/Credit-Intelligence-Risk-Analytics-Framework.git
cd Credit-Intelligence-Risk-Analytics-Framework
```

### 2. Create and activate a virtual environment (recommended)

**Windows**

```bash
python -m venv venv
venv\Scripts\activate
```

**macOS / Linux**

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

### 4. Start the API and web application

```bash
uvicorn main:app --reload
```

Open [http://127.0.0.1:8000](http://127.0.0.1:8000) in your browser.

Interactive API documentation is available at [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs).

> The application loads `credit_risk_model.pkl` and `best_threshold.pkl` during startup. Keep both files in the project root when running the service.

## Deployment

The repository includes a `render.yaml` configuration for deploying the FastAPI service on Render. Connect the GitHub repository to Render and use the configuration file to create the web service. Confirm that the required model and threshold files are available in the deployed repository and that the service starts successfully.

## Important Notes

- This is an educational and portfolio project; predictions should not be used as the sole basis for real lending decisions.
- A model's probability output is an estimate, not a guarantee of repayment or default.
- Model performance, fairness, calibration, and suitability should be independently evaluated before any real-world use.
- The example API response is illustrative. Do not interpret it as a benchmark or verified model metric.

## Future Improvements

- Add model evaluation results such as precision, recall, F1-score, ROC-AUC, and a confusion matrix to the documentation.
- Add explainability with SHAP plots or applicant-level feature contributions, provided these are implemented in the model pipeline.
- Add automated tests for input validation and prediction responses.
- Add structured logging, monitoring, and stronger API error handling.
- Document model training, preprocessing, and threshold-selection methodology.

## Author

**Raghu Datta Pailla**

- GitHub: [Raghu113-dev](https://github.com/Raghu113-dev)
- Project: [Credit Intelligence & Risk Analytics Framework](https://github.com/Raghu113-dev/Credit-Intelligence-Risk-Analytics-Framework)

---

If you find this project useful, consider giving the repository a star.
