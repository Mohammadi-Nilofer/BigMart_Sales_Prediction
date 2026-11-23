# 🛒 BigMart Sales Prediction

An end-to-end machine learning project that predicts how much of a product a BigMart store will sell, from loading raw data into MySQL through to an interactive Streamlit web app.

## Business problem

Retailers need to know how much of each product a store is likely to sell. This helps with stock planning, avoiding overstock and empty shelves, and setting realistic sales targets for each outlet. This project builds a model that predicts **item sales per outlet** from product details (weight, fat content, visibility, type and price) and store details (size, location tier, type and age).

## Dataset

- The public **BigMart Sales** dataset, supplied as three XML files: item details, outlet details and sales
- **8,523 records** and **11 input features** after joining the three tables
- Target variable: `Item_Outlet_Sales`

> The raw XML files are not included in this repository. The trained model (`bigmart_best_model.pkl`) is included, so the app runs without them.

## Workflow

1. **Database setup** (`DB_Creation.ipynb`): read the three XML files with pandas, created matching MySQL tables automatically from the data types, and bulk-inserted the data using PyMySQL.
2. **Data preparation** (`ML_Model_Creation.ipynb`):
   - Queried the three tables from MySQL and joined them on `ID`
   - Checked for duplicates and missing values (none found)
   - Standardised inconsistent fat-content labels (`LF`, `low fat` and `reg` became `Low Fat` and `Regular`)
   - Created an **Outlet Age** feature from the establishment year
   - Capped extreme `Item_Visibility` values at 0.3
3. **Modelling**: built a scikit-learn `Pipeline` with one-hot encoding for categorical features, then compared three regression models on an 80/20 train–test split.
4. **Deployment** (`app.py`): saved the best pipeline and built a Streamlit app that predicts sales from user inputs.

## Results

| Model | R² (test set) |
|---|---|
| **Gradient Boosting** ✅ | **0.593** |
| Random Forest | 0.563 |
| Linear Regression | 0.408 |

**Gradient Boosting performed best**, explaining about **59% of the variation in item sales** on unseen data. That is a 45% improvement over the Linear Regression baseline. The gap shows that sales depend on non-linear interactions between product and store features, which tree-based models capture better.

## Project structure

| File | Purpose |
|---|---|
| `DB_Creation.ipynb` | Loads the XML files into a MySQL database |
| `ML_Model_Creation.ipynb` | Data preparation, model comparison and model export |
| `app.py` | Streamlit app for predictions |
| `bigmart_best_model.pkl` | Trained Gradient Boosting pipeline |
| `requirements.txt` | Python dependencies |

## Run the app

```bash
pip install -r requirements.txt
streamlit run app.py
```

Then open the local link Streamlit shows, enter product and store details, and click **Predict Sales**.

> The model was trained with scikit-learn 1.6.1, which `requirements.txt` pins to make sure the saved model loads correctly.

## Next steps

- Add RMSE and MAE alongside R² to report prediction error in sales units
- Treat zero `Item_Visibility` values as missing data and impute them
- Tune hyperparameters with cross-validation
- Add feature importance charts to show which factors drive sales most
- Deploy the app publicly on Streamlit Community Cloud

## Tech stack

Python · pandas · NumPy · scikit-learn · MySQL · PyMySQL · Streamlit

---

**Author:** Mohammadi Nilofer · [LinkedIn](https://linkedin.com/in/yourprofile)

