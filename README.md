# CodeAtlas

**Agentic Codebase Intelligence Platform**

CodeAtlas helps developers understand an unfamiliar codebase faster. A user connects or submits a GitHub repository, and CodeAtlas analyzes its structure to explain the project, important files, architecture, setup process, and development flow.

The goal is to reduce the time new team members spend searching through files, documentation, and source code before they can begin contributing.

## Project Status

CodeAtlas is currently under active development. The backend foundation, database integration, and Docker-based local development environment are being built first. Repository ingestion, code analysis, retrieval, and the conversational onboarding experience will be added incrementally.

## Planned Features

- Import and analyze a GitHub repository
- Generate a high-level project summary
- Explain the repository folder structure
- Identify important files, services, and dependencies
- Provide setup and run instructions
- Answer natural-language questions about the codebase
- Use retrieval-augmented generation (RAG) to ground answers in repository content
- Store repository metadata and analysis results
- Support repeatable onboarding for new developers

## Tech Stack

- **Backend:** Python and FastAPI
- **API documentation:** Swagger UI / OpenAPI
- **Database:** PostgreSQL
- **ORM:** SQLAlchemy
- **Database migrations:** Alembic
- **Containerization:** Docker and Docker Compose
- **Version control integration:** GitHub
- **AI layer:** LLM-based repository explanation and question answering
- **Future retrieval layer:** Embeddings and a vector database

## Repository Structure

```text
CodeAtlas/
├── apps/
│   ├── api/                 # FastAPI backend
│   │   ├── app/             # Application source code
│   │   ├── tests/           # Backend tests
│   │   ├── Dockerfile       # API container configuration
│   │   └── requirements.txt # Python dependencies
│   └── web/                 # Next.js frontend
├── compose.yaml             # Local multi-container environment
├── .env.example             # Example environment variables
├── .gitignore
└── README.md
```

The exact structure may grow as repository-processing workers and retrieval components evolve.

## Prerequisites

Install the following tools before running the project:

- Git
- Docker Desktop
- Python 3.11 or later, if running the API outside Docker
- Node.js 20 or later, if running the web app outside Docker

## Getting Started

### 1. Clone the repository

```bash
git clone git@github.com:sinchana2023/CodeAtlas.git
cd CodeAtlas
```

### 2. Configure environment variables

If the repository contains an example environment file, copy it:

```bash
cp .env.example .env
```

Update the values in `.env` for your local environment. Do not commit secrets or API keys.

Example variables:

```env
DATABASE_URL=postgresql+psycopg://onboardai:onboardai@db:5432/onboardai
POSTGRES_DB=onboardai
POSTGRES_USER=onboardai
POSTGRES_PASSWORD=onboardai
OPENAI_API_KEY=your-key-here
```

Additional GitHub and AI-provider credentials will be required when those integrations are enabled.

### 3. Start the application with Docker

Run this command from the repository root—the directory containing `compose.yaml`:

```bash
docker compose up --build
```

To run the containers in the background:

```bash
docker compose up -d --build
```

### 4. Check the running containers

```bash
docker compose ps
```

### 5. View backend logs

```bash
docker compose logs api --tail=100
```

To follow the logs continuously:

```bash
docker compose logs -f api
```

## API Documentation

After the API is running, open the interactive Swagger documentation:

http://localhost:8000/docs

The alternative ReDoc documentation is available at:

http://localhost:8000/redoc

Swagger UI lists the available endpoints and lets you test requests directly from the browser.

## Running the API Locally Without Docker

From the repository root:

```bash
cd apps/api
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload
```

The exact import path may change as the backend structure evolves.

## Running the Web App

From the repository root:

```bash
cd apps/web
npm install
npm run dev
```

Open http://localhost:3000 in your browser.

## Database Migrations

Run migrations from the API container:

```bash
docker compose exec api alembic upgrade head
```

Create a new migration after changing database models:

```bash
docker compose exec api alembic revision --autogenerate -m "describe the change"
```

## Stopping the Application

Stop the containers:

```bash
docker compose down
```

Stop the containers and remove their local volumes:

```bash
docker compose down -v
```

The second command deletes local database data stored in Docker volumes, so use it only when you want a fresh database.

## Development Roadmap

- Build the FastAPI application and health-check endpoint
- Configure PostgreSQL, SQLAlchemy, and Alembic
- Add repository records and ingestion APIs
- Integrate GitHub repository access
- Parse and chunk relevant source files
- Generate embeddings and store searchable repository context
- Add RAG-based repository question answering
- Build the onboarding dashboard and chat interface
- Add automated tests, observability, and deployment configuration

## Security Notes

- Keep `.env` files, access tokens, and API keys out of Git.
- Use a GitHub token with the minimum permissions required.
- Validate repository URLs and uploaded content before processing.
- Do not send secrets or unsupported private files to an external AI provider.

## Contributing

This project is currently being developed as a portfolio project. Contributions, suggestions, and issue reports can be submitted through GitHub.

## Author

Sinchana Nagesh Makam

GitHub: [sinchana2023](https://github.com/sinchana2023)
