# Road Traffic Crash Prediction

## Project overview

This project explores historical road-crash records and uses machine learning to classify crash records into severity categories. The goal is to understand patterns in the available data and assess whether machine-learning models can help identify records belonging to the less common severity class.

**Important:** The notebook uses `severity_index` as the prediction target and reports two target classes as **Class 0** and **Class 1**. The notebook does not define the real-world meaning of each class (for example, whether a class means fatal, serious, or minor). Do not assign those meanings without checking the original dataset documentation or data dictionary.

This is an academic analysis, not a live crash-warning system and not a tool for predicting the outcome of an individual journey.

## Dataset

- **Input file:** `processed_crash_data.csv`
- **Dataset size described in the notebook:** 25,252 records and 17 features before cleaning.
- **Target column:** `severity_index`
- **Data source:** [World Bank Microdata Library – dataset catalogue](https://microdata.worldbank.org/index.php/catalog/6249)

The notebook describes fields such as crash year, hour, month, day of week, time of day, season, distance to Nairobi, local crash density, and indicators for pedestrian- or vehicle-related words in reports. The exact fields available after cleaning may differ because some columns are removed during preparation.

## Project workflow

1. **Load the data** – read the CSV file into a pandas DataFrame.
2. **Inspect the data** – review sample records, column types, summary statistics, and missing values.
3. **Clean the data** – remove duplicate records and constant columns, and inspect missing date/time information.
4. **Explore patterns** – examine class distribution and crash counts by selected time and category fields.
5. **Prepare model inputs** – separate the features from `severity_index` and remove columns treated as leakage or redundant.
6. **Preprocess features** – fill missing values, scale numerical fields, and encode categorical fields.
7. **Train models** – compare Logistic Regression, Decision Tree, and Random Forest.
8. **Evaluate models** – compare accuracy, precision, recall, F1-score, PR-AUC, and confusion matrices.

## Models and reported results

The notebook reports the following results on its test split:

| Model | Accuracy | Precision (Class 1) | Recall (Class 1) | F1-score (Class 1) | PR-AUC |
|---|---:|---:|---:|---:|---:|
| Logistic Regression | 57.68% | 11.37% | 60.46% | 0.191 | 0.121 |
| Decision Tree | 55.04% | 9.91% | 54.75% | 0.168 | 0.095 |
| Random Forest | 71.66% | 12.13% | 38.78% | 0.185 | 0.109 |

Logistic Regression was selected in the notebook because it achieved the highest Class 1 recall and F1-score among the three tested models. Random Forest had the highest overall accuracy, but lower Class 1 recall.

A majority-class Dummy Classifier achieved 91.72% accuracy but 0% recall for Class 1. This illustrates why accuracy alone can be misleading when one class is much more common than the other.

### Logistic Regression confusion matrix

The notebook reports:

| Actual / Predicted | Predicted Class 0 | Predicted Class 1 |
|---|---:|---:|
| Actual Class 0 | 1,673 | 1,240 |
| Actual Class 1 | 104 | 159 |

In plain language, the model correctly identified 159 of the 263 Class 1 records in the test set, but it also labelled 1,240 Class 0 records as Class 1. The number of false positive predictions is high, so model predictions need careful interpretation.

## Key conclusions

- The notebook successfully compares three classification approaches using the same general preprocessing workflow.
- Logistic Regression gave the strongest balance of Class 1 recall and F1-score among the tested models.
- The model still has low precision for Class 1 and produces many false positives.
- The results show potential for exploratory analysis, but they are not strong enough to justify operational or individual-level decisions.

## Recommendations

1. **Confirm what the severity classes mean.** Check the original data dictionary and document the definition of Class 0 and Class 1.
2. **Improve crash-record quality.** Reduce missing values and improve consistency in dates, times, locations, and outcome labels.
3. **Add relevant road-safety information where available.** Examples include road surface and layout, lighting, weather, vehicle type, traffic volume, and verified crash outcomes.
4. **Review possible target leakage.** Ensure that no input field contains information that would only be known after the crash outcome.
5. **Address class imbalance carefully.** Test suitable class-balancing approaches using training data only, then compare their effects on recall, precision, and false alarms.
6. **Validate on newer and separate data.** Test the model on records from a later period or another suitable source to see whether the patterns hold.
7. **Use findings to guide further investigation, not to assign blame.** Explore high-count time periods or locations with road-safety professionals and verify them against exposure data such as traffic volume.

## Requirements

The notebook uses Python and the following main packages:

- pandas
- NumPy
- Matplotlib
- Seaborn
- scikit-learn
- Jupyter Notebook or JupyterLab

Install the packages with:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

## How to run

1. Place `processed_crash_data.csv` in the same folder as the notebook.
2. Open the notebook in Jupyter Notebook, JupyterLab, or a compatible environment.
3. Run the cells from top to bottom.
4. Check that the data file path in the loading cell matches your folder structure.

The notebook saves a cleaned dataset as `cleaned_crash_data_with_temporal.csv`.

## Limitations

- The meanings of the two `severity_index` classes are not explicitly documented in the notebook text.
- The model has low precision for Class 1 and generates many false positive predictions.
- The dataset fields described in the notebook do not include all factors that may influence crash severity.
- The model is based on historical records and may not generalize to new locations or time periods.
- The results depend on the quality and representativeness of the source data.

## Suggested project structure

```text
road-traffic-crash-prediction/
├── Crash_Explained.ipynb
├── processed_crash_data.csv
├── cleaned_crash_data_with_temporal.csv   # created when the notebook runs
└── README_Road_Traffic_Crash_Prediction.md
```

## Disclaimer

This project is for learning and research purposes. It should not be used as the sole basis for emergency response, enforcement, insurance, driver assessment, or road-design decisions. Any practical use would require verified labels, broader data, independent validation, expert review, and ongoing monitoring.
