# Course Catalog API

A FastAPI backend for a catalog of six courses, with filtering, sorting and pagination.

## How to run

Python 3.10 or newer is required.

Run these commands in the project folder using Windows PowerShell:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
fastapi dev main.py
```

Open the interactive documentation:
http://127.0.0.1:8000/docs

## What was verified

- GET / — 200, server is running.
- GET /courses — 200, course list returned.
- GET /courses/web-security — 200, Web Security Essentials returned.
- GET /courses/nope — 404, detail: Course not found.
- GET /courses?is_elective=true — 2 elective courses: ai-integration and api-design.
- GET /courses?is_elective=false — 4 required courses.
- GET /courses?sort=title — all 6 courses sorted alphabetically by title.
- GET /courses?page=2&page_size=2 — web-security and backend-fastapi.
- GET /courses?page=4&page_size=2 — an empty list [].

Pagination uses Depends(pagination).