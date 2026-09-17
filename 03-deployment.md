# Deploy a Full-Stack App with AI Coding Assistants

Make a web application accessible for everyone on the internet:
- Take already working locally application
- Containerize the application
- Switch from SQLite to Postgres
- Add integration tests
- Deploy to AWS
- Set up CI/CD via GitHub Actions

## Recap

The process of creating a working app:
- Created the Frontend only with React
- Created an OpenAPI specification for defining the frontend-backend API
- Created the backend from this specification
- Added database support with SQLite and SQLAlchemy

## Containerization

To run the application locally, we need to execute two commands: one for the frontend and one for the backend.

In two separate terminals:
```
# first terminal
cd frontend
npm run dev

# second terminal
make run
```

In production, the frontend code doesn’t change while the application is running. We build it once and get a set of static HTML, CSS, and JavaScript files.

This means we don’t need a separate container for the frontend: the backend can serve these files. So, we need only one container.

Ask a coding agent to create a Docker container:
```
Create a Dockerfile that builds the frontend with Node, then builds a Python image with the backend and the frontend static files.

Backend should serve the frontend.
```

Ask coding agent to build a Dockerfile:
```
I want you to build a Dockerfile from Dockerfile and run it. Update README.md file and put there instructions for running and building a Dockerfile.
```

Now we can build and run the image.

## Switch from SQLite to Postgres

Start Postgres locally:
```
docker run -d \
  --name interview-canvas-db \
  -e POSTGRES_USER=sdip \
  -e POSTGRES_PASSWORD=sdip \
  -e POSTGRES_DB=sdip \
  -p 5432:5432 \
  -v interview-canvas-pgdata:/var/lib/postgresql/data \
  postgres:16-alpine
```

Now we can ask the assistant to use it:
```
Add Postgres support to the backend.
This is how I run Postgres:
docker run -d \
  --name interview-canvas-db \
  -e POSTGRES_USER=sdip \
  -e POSTGRES_PASSWORD=sdip \
  -e POSTGRES_DB=sdip \
  -p 5432:5432 \
  -v interview-canvas-pgdata:/var/lib/postgresql/data \
  postgres:16-alpine
```

Run the backend against Postgres from the repository root:
```
export SDIP_DATABASE_URL=postgresql://sdip:sdip@localhost:5432/sdip
make run
```

When it’s done, repeat the two-session test.

## Docker Compose

Ask agent:
```
Create docker-compose.yaml with two services: Postgres and the app.
```

Start it:
```
docker compose up --build
```

## Integration and end-to-end tests

The AI assistant might have created some backend tests.

If it didn’t, ask it to create integration tests:
```
Create integration tests that run against docker-compose.yaml. What scenarios should we test?
```

Ask the AI assistant to implement an end-to-end test (example):
```
Add an end-to-end test that runs against docker-compose.yaml.

Use Playwright to:

1. Log in as the interviewer (session 1).
2. Create an interview session.
3. Share the join link.
4. Join from a separate client as the candidate (session 2).
5. Change the canvas as the candidate (session 2).
6. Verify that the interviewer sees the change (session 1).

Put the tests in the e2e/ folder in the repository root.
```

After it finishes, we can run the tests with a single make command:
```
make e2e
```

## Deploy to AWS



Information based on [Deploy a Full-Stack App with AI Coding Assistants. Part 3](https://aishippingblog.com/p/deploy-a-full-stack-app-with-ai-coding) and [Test, Containerize, and Deploy an AI-Assisted App - Alexey Grigorev]([https://www.youtube.com/watch?v=x9dq5nBpDg8](https://www.youtube.com/watch?v=gxt5ZDVnBMM)) from [AI Dev Tools Zoomcamp: AI-Native Software Engineering](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp)
