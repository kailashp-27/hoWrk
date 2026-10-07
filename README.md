# hoWrk

A hackathon project that brings incident reporting, guardian support, and emergency resource mapping into one app. Citizens, guardians, and authorities get separate dashboards around a shared map.

The frontend uses React, TypeScript, Vite, and Leaflet. The FastAPI backend stores records in Firebase Firestore and includes Twilio calls for assistance and guardian alerts.

## Main workflows

- Citizens can report incidents and view nearby reports and resources.
- Guardians can register their location and availability.
- Authorities can review incidents, acknowledge reports, and manage resources.
- Users can save an emergency contact and request assistance.
- Route previews show nearby incident warnings and a simple safety score.

Route scoring is a proximity heuristic over stored incidents. It does not find a proven safest route; when street routing is unavailable, the backend falls back to a direct path.

## Run locally

Use Node.js 22.12+ and Python 3.11+. You also need a Firebase project with Firestore enabled and a service account key.

Place the key at `backend/firebase-key.json`. This exact path is used by the database module; setting `GOOGLE_APPLICATION_CREDENTIALS` alone does not replace it.

From the repository root, copy `.env.example` to `.env`. For call features, fill in `TWILIO_SID`, `TWILIO_TOKEN`, `TWILIO_FROM`, and `ASSISTANCE_TO` with your own configuration.

Start the backend:

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r req.txt
uvicorn main:app --reload --port 8000
```

On Windows, activate with `.venv\Scripts\activate`.

In another terminal, from the repository root:

```bash
npm install
npm run dev
```

Open [localhost:3000](http://localhost:3000). The API runs on [localhost:8000](http://localhost:8000), with interactive documentation at `/docs`. Firestore-backed actions require the service account file even if the server starts without it.

## Development

```bash
npm run lint
npm run build
```

`npm run lint` runs TypeScript checks. `backend/test_api.py` is a manual API smoke script that registers a test account and reports an incident against a running server; use a development Firebase project if running it.

## Code guide

- `src/components/`: role dashboards, incident maps, forms, and SOS controls.
- `backend/main.py`: incident, resource, guardian, contact, and navigation endpoints.
- `backend/database.py`: Firebase Admin and Firestore setup.
- `backend/auth.py`: password hashing and JWT handling.
- `.env.example`: call-service configuration template.

The project is a hackathon prototype. JWT settings are currently fixed in `backend/auth.py`; the similarly named values in `.env.example` are not read by that module.
