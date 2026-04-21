# Multimodal Bug Summarizer

[![CI](https://github.com/Flakes8387/Bug-Analyser/workflows/CI/badge.svg)](https://github.com/Flakes8387/Bug-Analyser/actions)

A full-stack JavaScript monorepo for AI-powered bug report summarization with multimodal input support (text + screenshots).

## Features

- Multimodal input: accept bug reports with text descriptions, stack traces, and screenshot images.
- AI-powered summarization: generate structured summaries with five dimensions:
  - Environment context
  - Actual behavior
  - Expected behavior
  - Bug category
  - Suggested solution
- Human annotation system for curated training data.
- JSONL export for ML model training.
- Full-stack architecture: React frontend, Express backend, FastAPI trainer service.
- Persistent storage using SQLite.
- Docker support via docker-compose.

## Project Structure

`	ext
multimodal-bug-summarizer/
|-- packages/
|   |-- frontend/                 # React + Vite + Tailwind CSS
|   |   |-- src/
|   |   |   |-- pages/            # SubmitBug, Review
|   |   |   |-- components/       # SummaryCard
|   |   |   -- main.jsx
|   |   -- package.json
|   -- backend/                  # Express.js API
|       |-- src/
|       |   |-- routes/           # Annotations API
|       |   |-- utils/            # Preprocessing utilities
|       |   |-- db.js             # SQLite database
|       |   |-- index.js          # Main server
|       |   -- internal.js       # Inference integration
|       -- package.json
|-- services/
|   -- trainer/                  # FastAPI inference service
|       |-- main.py               # Mock trainer (ready for ML integration)
|       -- requirements.txt
|-- models/                       # Model storage directory
|-- docker-compose.yml
-- package.json                  # Root workspace config
`

## Technology Stack

### Frontend
- React 18
- Vite
- Tailwind CSS
- React Router
- Axios

### Backend
- Express.js
- better-sqlite3
- Multer
- Joi
- Winston
- Jest

### Trainer Service
- FastAPI
- Uvicorn
- Pydantic

## Getting Started

### Prerequisites
- Node.js >= 18
- Yarn (or npm)
- Docker and Docker Compose (for containerized deployment)

### Installation

`ash
yarn bootstrap
`

### Development

#### Option 1: Docker Compose (recommended)

`ash
docker compose up --build
`

Services:
- Frontend: http://localhost:3000
- Backend: http://localhost:4000
- Trainer: http://localhost:8000

#### Option 2: Local development

`ash
# Terminal 1: Frontend
cd packages/frontend
yarn dev

# Terminal 2: Backend
cd packages/backend
yarn dev

# Terminal 3: Trainer
cd services/trainer
python -m uvicorn main:app --reload --port 8000
`

### Build

`ash
yarn --cwd packages/frontend build
yarn --cwd packages/backend build
`

### Testing

`ash
yarn test
yarn --cwd packages/backend test
yarn --cwd packages/backend test --coverage
`

### Linting

`ash
yarn lint
`

## API Documentation

### Bug Reports

#### Submit bug report

`http
POST /api/reports
Content-Type: multipart/form-data
`

Fields:
- title: string (required)
- description: string (required)
- os: string
- browser: string
- browserVersion: string
- stacktrace: string
- screenshots: file[] (images)

#### Get all reports

`http
GET /api/reports?limit=20&offset=0
`

#### Get single report

`http
GET /api/reports/:id
`

#### Accept report

`http
POST /api/reports/:id/accept
`

### Annotations

#### Create annotation

`http
POST /api/annotations
Content-Type: application/json
`

`json
{
  "reportId": "report-id",
  "environment": "macOS Chrome 119",
  "actual": "Page crashes",
  "expected": "Page works correctly",
  "category": "UI Bug",
  "solution": "Add error boundary",
  "annotator": "reviewer@example.com"
}
`

#### Export training data (admin only)

`http
GET /api/annotations/export/all
X-Admin-Token: your-admin-token
`

Returns: JSONL file for ML training.

See [ANNOTATIONS.md](packages/backend/ANNOTATIONS.md) for full API details.

## Environment Variables

### Backend

`ash
PORT=4000
TRAINER_URL=http://localhost:8000
ADMIN_TOKEN=secure-token-change-in-production
USE_OCR=false
`

### Frontend

`ash
VITE_API_URL=http://localhost:4000
`

### Docker Compose
Environment variables are configured in docker-compose.yml.

## Testing

The project includes unit tests:
- Backend: tests for preprocessing utilities
- Frontend: Jest and React Testing Library (ready for implementation)

`ash
docker compose exec backend npm test
docker compose exec backend npm test -- --coverage
`

## CI/CD

GitHub Actions workflow at .github/workflows/ci.yml runs on every push and PR:
- Build and test
- Docker image build and compose validation

## Deployment

### Docker Compose Production

`ash
docker compose up --build -d
docker compose logs -f
docker compose down
`

### Manual Deployment

1. Build frontend: yarn --cwd packages/frontend build
2. Deploy backend: node packages/backend/src/index.js
3. Deploy trainer: python -m uvicorn services.trainer.main:app --host 0.0.0.0

## Future Enhancements

- Replace mock trainer with actual ML model
- Add image OCR with Tesseract
- Implement frontend tests
- Add user authentication
- Create admin dashboard for annotations
- Add batch inference processing
- Implement model training pipeline
- Add monitoring and logging
- Deploy to cloud (AWS/GCP/Azure)

## Contributing

1. Fork the repository.
2. Create a feature branch.
3. Commit your changes.
4. Push to your branch.
5. Open a pull request.

## License

This project is licensed under the MIT License.

## Acknowledgments

- Yarn Workspaces for monorepo management
- FastAPI for the Python inference service
- better-sqlite3 for embedded storage
- Vite for frontend builds
