# Multimodal Bug Summarizer

[![CI](https://github.com/Flakes8387/Bug-Analyser/workflows/CI/badge.svg)](https://github.com/Flakes8387/Bug-Analyser/actions)

A full-stack JavaScript monorepo for AI-powered bug report summarization with multimodal input support (text + screenshots).

## �Ys? Features

- **Multimodal Input**: Accept bug reports with text descriptions, stack traces, and screenshot images
- **AI-Powered Summarization**: Automatically generate structured summaries with 5 key dimensions:
  - Environment context
  - Actual behavior
  - Expected behavior
  - Bug category
  - Suggested solution
- **Human Annotation System**: Collect high-quality training data from human reviewers
- **JSONL Export**: Export annotated data for ML model training
- **Full-Stack Architecture**: React frontend, Express backend, FastAPI trainer service
- **Persistent Storage**: SQLite database for reports and annotations
- **Docker Support**: Fully containerized services with docker-compose

## �Y"� Project Structure

```
multimodal-bug-summarizer/
�"o�"?�"? packages/
�",   �"o�"?�"? frontend/          # React + Vite + Tailwind CSS
�",   �",   �"o�"?�"? src/
�",   �",   �",   �"o�"?�"? pages/     # SubmitBug, Review
�",   �",   �",   �"o�"?�"? components/# SummaryCard
�",   �",   �",   �""�"?�"? main.jsx
�",   �",   �""�"?�"? package.json
�",   �""�"?�"? backend/           # Express.js API
�",       �"o�"?�"? src/
�",       �",   �"o�"?�"? routes/    # Annotations API
�",       �",   �"o�"?�"? utils/     # Preprocessing utilities
�",       �",   �"o�"?�"? db.js      # SQLite database
�",       �",   �"o�"?�"? index.js   # Main server
�",       �",   �""�"?�"? internal.js# Inference integration
�",       �""�"?�"? package.json
�"o�"?�"? services/
�",   �""�"?�"? trainer/           # FastAPI inference service
�",       �"o�"?�"? main.py        # Mock trainer (ready for ML integration)
�",       �""�"?�"? requirements.txt
�"o�"?�"? models/                # Model storage directory
�"o�"?�"? docker-compose.yml
�""�"?�"? package.json           # Root workspace config
```

## �Y>�️ Technology Stack

### Frontend
- **React 18** - UI framework
- **Vite** - Build tool and dev server
- **Tailwind CSS** - Styling
- **React Router** - Navigation
- **Axios** - HTTP client

### Backend
- **Express.js** - Web framework
- **better-sqlite3** - Database
- **Multer** - File upload handling
- **Joi** - Validation
- **Winston** - Logging
- **Jest** - Testing

### Trainer Service
- **FastAPI** - Python web framework
- **Uvicorn** - ASGI server
- **Pydantic** - Data validation

## �Ys� Getting Started

### Prerequisites

- Node.js >= 18
- Yarn (or npm)
- Docker & Docker Compose (for containerized deployment)

### Installation

```bash
# Install dependencies
yarn bootstrap
```

### Development

#### Option 1: Docker Compose (Recommended)

```bash
# Start all services
docker compose up --build
```

Services will be available at:
- **Frontend**: http://localhost:3000
- **Backend**: http://localhost:4000
- **Trainer**: http://localhost:8000

#### Option 2: Local Development

```bash
# Terminal 1: Frontend
cd packages/frontend
yarn dev

# Terminal 2: Backend
cd packages/backend
yarn dev

# Terminal 3: Trainer
cd services/trainer
python -m uvicorn main:app --reload --port 8000
```

### Build

```bash
# Build frontend for production
yarn --cwd packages/frontend build

# Build backend (no build step required)
yarn --cwd packages/backend build
```

### Testing

```bash
# Run all tests
yarn test

# Run backend tests only
yarn --cwd packages/backend test

# Run with coverage
yarn --cwd packages/backend test --coverage
```

### Linting

```bash
yarn lint
```

## �Y"� API Documentation

### Bug Reports

#### Submit Bug Report
```bash
POST /api/reports
Content-Type: multipart/form-data

Fields:
- title: string (required)
- description: string (required)
- os: string
- browser: string
- browserVersion: string
- stacktrace: string
- screenshots: file[] (images)
```

#### Get All Reports
```bash
GET /api/reports?limit=20&offset=0
```

#### Get Single Report
```bash
GET /api/reports/:id
```

#### Accept Report
```bash
POST /api/reports/:id/accept
```

### Annotations

#### Create Annotation
```bash
POST /api/annotations
Content-Type: application/json

{
  "reportId": "report-id",
  "environment": "macOS Chrome 119",
  "actual": "Page crashes",
  "expected": "Page works correctly",
  "category": "UI Bug",
  "solution": "Add error boundary",
  "annotator": "reviewer@Flakes8387.com"
}
```

#### Export Training Data (Admin Only)
```bash
GET /api/annotations/export/all
X-Admin-Token: your-admin-token

Returns: JSONL file for ML training
```

See [ANNOTATIONS.md](packages/backend/ANNOTATIONS.md) for full API documentation.

## �Y"' Environment Variables

### Backend
```bash
PORT=4000
TRAINER_URL=http://localhost:8000
ADMIN_TOKEN=secure-token-change-in-production
USE_OCR=false  # Set to 'true' to enable Tesseract OCR
```

### Frontend
```bash
VITE_API_URL=http://localhost:4000
```

### Docker Compose
Environment variables are configured in `docker-compose.yml`.

## �Y�� Testing

The project includes comprehensive unit tests:

- **Backend**: 18 tests covering preprocessing utilities
- **Frontend**: Jest + React Testing Library (ready for implementation)

```bash
# Run backend tests
docker compose exec backend npm test

# Run with coverage
docker compose exec backend npm test -- --coverage
```

## �Y"" CI/CD

GitHub Actions workflow (`.github/workflows/ci.yml`) runs on every push and PR:

1. **Build & Test**
   - Install dependencies
   - Run backend tests
   - Run frontend tests (when implemented)
   - Build frontend for production

2. **Docker Build**
   - Build all Docker images
   - Validate docker-compose config

## �Y"� Deployment

### Docker Compose Production

```bash
# Build and start in production mode
docker compose up --build -d

# View logs
docker compose logs -f

# Stop services
docker compose down
```

### Manual Deployment

1. Build frontend: `yarn --cwd packages/frontend build`
2. Deploy backend: `node packages/backend/src/index.js`
3. Deploy trainer: `python -m uvicorn services.trainer.main:app --host 0.0.0.0`

## �Y"� Future Enhancements

- [ ] Replace mock trainer with actual ML model
- [ ] Add image OCR with Tesseract
- [ ] Implement frontend tests
- [ ] Add user authentication
- [ ] Create admin dashboard for annotations
- [ ] Add batch inference processing
- [ ] Implement model training pipeline
- [ ] Add monitoring and logging
- [ ] Deploy to cloud (AWS/GCP/Azure)

## �Y�� Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Commit changes: `git commit -m 'Add amazing feature'`
4. Push to branch: `git push origin feature/amazing-feature`
5. Open a Pull Request

## �Y"" License

This project is licensed under the MIT License.

##  Acknowledgments

- Yarn Workspaces for monorepo management
- FastAPI for the Python inference service
- Better-sqlite3 for embedded database
- Vite for lightning-fast frontend builds


