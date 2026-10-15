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

## What changed in Lab 5

CORS allows http://localhost:3000 so browser components can read
responses from the backend at http://localhost:8000.
All methods and headers are allowed.

Python fields use snake_case. CamelModel converts JSON field names
to camelCase, for example is_elective becomes isElective.
validate_by_name=True keeps Python field names accepted.
Query parameters still use is_elective and page_size.

### New routes

POST /courses/{course_id}/like increments likes and returns the course.
Example response:
```json
{"id":"web-security","title":"Web Security Essentials","description":"JWT/OAuth2, XSS, CSRF, and SQL injection defense.","credits":4,"isElective":false,"likes":22}
```

POST /enrollments creates an enrollment and returns 201.
Example request:
```json
{"courseId":"web-security","studentName":"Bekasyl"}
```

Example response:
```json
{"courseId":"web-security","studentName":"Bekasyl","id":1}
```

GET /enrollments returns the enrollment list:
```json
[{"courseId":"web-security","studentName":"Bekasyl","id":1}]
```

Names shorter than two characters are rejected with 422.
Unknown course IDs return 404.

Likes and enrollments are stored in memory and reset when
the backend restarts.

### Alternative startup command

If Windows blocks fastapi.exe, run:
```powershell
python -m uvicorn main:app --reload
```