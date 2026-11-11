# Job Title Prediction by Resume

Machine-learning project that predicts a suitable job title from a candidate's
education, interests, skills, certifications, work experience, and academic
score.

The project includes:

- A Jupyter Notebook for data exploration, training, evaluation, and interactive
  predictions.
- A FastAPI service that trains the same pipeline and exposes a `/predict`
  endpoint.
- The resume/job-title dataset used by the notebook and API.

## How it works

1. The dataset is loaded from `Job Prediction By Resume.csv`.
2. The candidate's name and target column (`title job`) are excluded from the
   input features.
3. Categorical fields are one-hot encoded.
4. The academic score is imputed and standardized.
5. Interests, skills, and previous work are combined and transformed with
   TF-IDF (unigrams and bigrams).
6. A class-balanced `RandomForestClassifier` predicts the job title and returns
   the highest class probability as confidence.

## Project structure

```text
.
├── Job Prediction By Resume.csv  # Training dataset
├── Job Title Prediction.ipynb    # Notebook workflow
├── job_predict_api.py            # FastAPI application
├── requirements.txt              # Python dependencies
└── README.md
```

## Setup

Python 3.9 or newer is recommended.

```bash
python -m venv .venv
```

Activate the virtual environment:

**Windows PowerShell**

```powershell
.venv\Scripts\Activate.ps1
```

**macOS/Linux**

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

## Run the notebook

Start Jupyter from the project root so the CSV file can be found:

```bash
jupyter notebook
```

Open `Job Title Prediction.ipynb`, run the cells in order, and use the
interactive prediction section to try candidate profiles.

## Run the API

From the project root:

```bash
python job_predict_api.py
```

The API is available at `http://127.0.0.1:8000`. Interactive API
documentation is available at `http://127.0.0.1:8000/docs`.

### Check API status

```http
GET /status-codes
```

### Predict a job title

```http
POST /predict
Content-Type: application/json
```

Example request:

```json
{
  "Gender": "Male",
  "Course_in_UG": "B.Tech",
  "UG_specialization": "Computer Science Engineering",
  "Certificate_course_title": "Python, SQL",
  "Average_CGPA": 8.2,
  "Interests": "Technology and software development",
  "Skills": "Python, SQL, Machine Learning",
  "Work_in_the_past": "Internship in software development"
}
```

Example response:

```json
{
  "predicted_job": "IT Specialist",
  "confidence": 0.74,
  "status_code": 200,
  "status_message": "OK",
  "message": "Prediction successful."
}
```

The exact predicted title and confidence depend on the dataset and trained
model.

## Notes

- The API trains the model when the application starts, so startup may take a
  little time.
- Run commands from the repository root because the application loads the CSV
  using a relative path.
- The dataset contains personal/profile-style fields. Use it responsibly and
  do not treat the prediction as a hiring decision.

## License

No license has been specified for this project yet.
