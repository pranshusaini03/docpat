# DocPat: Disease Prediction Django App

A Django web application that serves multiple disease prediction tools:
- Diabetes
- Liver disease
- Heart disease
- Breast cancer (SVM)
- GI Diseases (image-based via CNN)

The app loads pre-trained models from pickle/H5 files and serves simple HTML templates for inputs and results.

## Prerequisites
- Python 3.10 or 3.11
- pip
- (Windows) Visual C++ Build Tools recommended for scientific packages

## Setup
1. Create and activate a virtual environment
```bash
python -m venv .venv
.venv\Scripts\activate  # Windows
# source .venv/bin/activate  # macOS/Linux
```

2. Install dependencies
```bash
pip install -r requirements.txt
```

3. Apply migrations
```bash
python manage.py migrate
```

4. Run the development server
```bash
python manage.py runserver
```

Visit http://127.0.0.1:8000/ in your browser.

## Important: Model file paths
Several view functions and `xc.py` reference ABSOLUTE paths to model files on a specific machine. Update these to point to the models in this repository under `model/`:

- In `docpat/views.py`:
  - `predictb` expects `model/breast_cancer_svm_model.pkl`
  - `predictd` expects `diabetes_model.pkl` (root of repo). You may want to move it into `model/` and update the path.
  - `predictl` expects `model/liver_prediction.pkl`
  - `predicth` expects `model/heart_disease_model.pkl`

- In `xc.py`:
  - `predict_image` expects `model/CNN_model.h5`

Recommended change: convert all absolute paths to project-relative paths using `BASE_DIR` from `settings.py`. Example pattern:
```python
from django.conf import settings
model_path = settings.BASE_DIR / "model" / "CNN_model.h5"
```

## Static and templates
- Templates live in `templates/`
- Static assets live in `static/`
- `TEMPLATES` in `docpat/settings.py` already includes the `templates/` directory
- `STATICFILES_DIRS` includes the `static/` directory

## Endpoints
See `docpat/urls.py` for available paths:
- `/` home
- `/diabetes`, `/liver`, `/heart`, `/breast`
- `/predictd`, `/predictl`, `/predicth`, `/predictb`
- `/GI_diseases`, `/predictg` (image upload and prediction)
- Info pages: `/diabetes_info`, `/breast_cancer_info`, `/heart_disease_info`, `/liver_disease_info`, `/GI_Diseases_info`

## Notes
- TensorFlow and Keras are heavy; use a Python environment compatible with your OS and hardware.
- If TensorFlow install fails on your system, consider using only the non-image predictors or install CPU-only TensorFlow wheels.
- SQLite DB (`db.sqlite3`) is included/created automatically; no external DB setup is required.

## License
MIT (adjust if needed). 
