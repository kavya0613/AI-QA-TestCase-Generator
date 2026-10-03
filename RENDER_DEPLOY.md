# Deploy AI Test Case Generator on Render

## 1. Push this folder to GitHub
Upload the contents of this folder to a GitHub repository. Do not upload `.env` or any API key.

## 2. Create the Render service
In Render: New -> Web Service -> connect the GitHub repository.

Use:
- Runtime: Python 3
- Build Command: `pip install -r requirements-render.txt && python scripts/train_models.py --epochs 30`
- Start Command: `uvicorn api:app --host 0.0.0.0 --port $PORT`
- Health Check Path: `/health`

`render.yaml` already contains these settings if you use Render Blueprint deployment.

## 3. Add the Gemini key
In Render -> Environment, add:
- `GOOGLE_API_KEY` = your Gemini API key

The deployment sets `LLM_PROVIDER=google` and `LLM_MODEL=gemini-2.0-flash`.

## 4. Open the service URL
The root URL `/` is the browser UI.
- `/` = Test Case Generator UI
- `/health` = health/status endpoint
- `/docs` = FastAPI Swagger documentation

## Important
The SQLite database uses the service filesystem. It is suitable for a demo, but data should not be treated as permanent storage across service replacement/redeployments. For permanent production history, move the database to PostgreSQL or another persistent data store.
