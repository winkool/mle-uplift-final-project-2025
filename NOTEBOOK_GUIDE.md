# Uplift Modeling Notebook - Quick Reference Guide

## 📁 File Location
`final_project_template.ipynb`

## 🎯 Project Goal
Optimize promo code targeting for Yandex Food using uplift modeling to identify customers most likely to respond positively to promotional offers.

## 📊 Dataset
- **File**: `uplift_fp_data.csv`
- **Size**: 64,000 customers
- **Features** (8):
  - `recency`: Days since last purchase
  - `history_segment`: Customer segment based on purchase history
  - `history`: Total purchase amount
  - `mens`: Bought men's products (0/1)
  - `womens`: Bought women's products (0/1)
  - `zip_code`: Geographic code
  - `newbie`: New customer flag (0/1)
  - `channel`: Communication channel
- **Treatment**: `treatment` (0=control, 1=received promo)
- **Target**: `target` (0=did not use, 1=used promo)

## 🚀 How to Use

### Option 1: View Executed Notebook
The notebook is already fully executed with all outputs visible. Simply open it in Jupyter:
```bash
jupyter notebook final_project_template.ipynb
```

### Option 2: Re-execute All Cells
If you want to run it from scratch:
```bash
jupyter nbconvert --to notebook --execute --inplace final_project_template.ipynb
```

Or in Jupyter interface: `Kernel > Restart & Run All`

## 📚 Notebook Structure

### Part 1: Data Preparation & Baseline Models
1. **Library Imports** - Load required packages
2. **Data Loading** - Import and inspect dataset
3. **EDA** - Exploratory data analysis with visualizations
4. **Statistical Testing** - T-test for treatment effect
5. **Correlation Analysis** - Feature relationships
6. **Baseline Models** - Train 3 uplift models:
   - Solo Model (S-learner)
   - TwoModels (T-learner)
   - Class Transformation

### Part 2: Optimization & Production
7. **Feature Engineering** - Create 7 new features
8. **Hyperparameter Tuning** - Optuna optimization (100 trials)
9. **Final Model Training** - Best model with optimal params
10. **Visualizations**:
    - Uplift curve
    - Qini curve
    - Uplift by percentiles
11. **Production Class** - `UpliftModelProduction` for deployment
12. **Testing** - Validate production class

## 🎯 Key Results

### Target Achievement
- **Metric**: Uplift@30% = **0.0441**
- **Target**: >= 0.035
- **Status**: ✅ **ACHIEVED** (26% above target)

### Model Performance
- **Uplift AUC**: 0.0264
- **Qini AUC**: 0.0598
- **Best Approach**: TwoModels + Feature Engineering + Optuna

### Business Impact
- **Conversion Lift**: 4.41% in top 30% of customers
- **Cost Savings**: ~70% (precision targeting)
- **ROI**: Significant improvement from avoiding wasted promotions

## 📦 Dependencies

Required libraries (install with `pip install -r requirements.txt`):
```
pandas>=2.2.1
numpy
scikit-learn
scikit-uplift>=0.5.1
optuna>=4.4.0
scipy
matplotlib
seaborn
```

## 🔧 Production Deployment

The notebook includes a production-ready class:

```python
from final_model import UpliftModelProduction

# Initialize
model = UpliftModelProduction(
    model=trained_model,
    feature_names=feature_columns
)

# Predict
customer_data = pd.DataFrame({...})
uplift_scores = model.predict(customer_data)

# Target high-uplift customers
top_customers = customer_data[uplift_scores > threshold]
```

## 📈 Interpretation

### Uplift Score Meaning
- **Positive**: Customer likely to respond to promo
- **Near Zero**: No expected effect
- **Negative**: Promo may harm relationship ("sleeping dogs")

### Recommended Strategy
1. Send promos to top 30% by uplift score
2. Avoid bottom percentiles (negative uplift)
3. Monitor and adjust threshold based on ROI

## 🐛 Troubleshooting

### Import Errors
```bash
pip install scikit-uplift optuna scipy
```

### Visualization Issues
If plots don't display:
```python
import matplotlib
matplotlib.use('TkAgg')  # or 'Qt5Agg'
```

### Memory Issues
For large datasets, reduce:
- `n_trials` in Optuna (default: 100)
- `n_estimators` in Random Forest (default: 96)

## 📝 Notes

- **Random State**: Fixed at 42 for reproducibility
- **Language**: Russian comments and markdown
- **Execution Time**: ~10-15 minutes for full notebook
- **File Size**: 662 KB (with outputs)

## 🤝 Support

For questions or issues:
1. Check cell outputs for error messages
2. Verify all dependencies are installed
3. Ensure dataset file is in same directory
4. Review utils.py for custom functions

## 📄 License

This notebook is part of the MLE Uplift Final Project 2025.

---

**Created**: February 2026  
**Status**: Production Ready ✅  
**Version**: 1.0
