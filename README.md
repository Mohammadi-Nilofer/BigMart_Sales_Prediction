# 🛒 BigMart Sales Prediction

Predicts product sales at BigMart stores using product and store details, from a MySQL database through to a Streamlit web app.

## Workflow
1. **Database**: loaded three XML files (items, outlets, sales) into MySQL using Python and PyMySQL.
2. **Data preparation**: joined the tables (8,523 records), standardised fat-content labels, created an Outlet Age feature and capped extreme visibility values.
3. **Modelling**: compared three regression models in a scikit-learn pipeline.
4. **App**: built a Streamlit app that predicts sales from user inputs.

## Results
| Model | R² score |
|---|---|
| **Gradient Boosting** ✅ | **0.593** |
| Random Forest | 0.563 |
| Linear Regression | 0.408 |

## Tech stack
Python · pandas · scikit-learn · MySQL · Streamlit

## Run the app
```bash
pip install -r requirements.txt
streamlit run app.py
```
