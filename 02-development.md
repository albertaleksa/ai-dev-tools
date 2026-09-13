# Build and Ship a Full-Stack App with AI Coding Assistants

We will create an end-to-end application from scratch. We will cover both frontend and backend, and add a database.

## Overview

From there, we start building:
- Generate a frontend that uses mocked backend calls.
- Establish backend-frontend contract by creating OpenAPI specifications from the frontend service layer.
- Implement the FastAPI backend, connect it to the frontend, and test the application.
- Add persistence with SQLite and SQLAlchemy.


## Start with a specification
Ask ChatGPT to create a specification for the Application.
Brainstorm with it like described in [previous part](https://github.com/albertaleksa/ai-dev-tools/edit/main/01-ai-native-development.md).
Describe the app, services, what components and behaviour I need. Maybe some technologies that I want to use.

## Frontend First
Use https://lovable.dev (or Codex, Claude Design, Replit, v0, and Bolt, but Lovable is better) to create a frontend interfaces.

Now open any tool of your choice and give it this prompt:
```
Create a <your app(system design interview)> application. 

[Paste the ChatGPT-generated specification here.]

Centralize every backend call in one services layer, and create a mock
implementation of it so the whole app runs without a real backend.

Add tests.
```

Lovable creates a React app in TypeScript. We can interact with it in the Lovable interface.

Next, save it to GitHub. If you use Lovable:
- Select the plus icon in the bottom-left corner
- Connect your GitHub account.
- Lovable creates a private repository. I usually change it to public.


### Move the Frontend into the Project
Next, we can clone the repository locally.

For the rest of the project, I want this setup:
```
/backend     # backend application and its tests
/docs        # supporting documentation
/frontend    # frontend application
AGENTS.md    # instructions for coding agents
openapi.yaml # API agreemen
```
So let’s create these folders and move all the frontend stuff to “frontend”, and the specification we created to “/docs/spec.md”.

To run a project you exported from Lovable:
```
cd frontend
npm i
npm run dev
```

## AGENTS.md

Let’s create one for this project too. Place it in the repository root, then ask agent to make it relevant to the project:
```
for backend, use uv for dependency management. a few useful commands:

uv sync
uv add <PACKAGE-NAME>
uv run python <PYTHON-FILE>

regularly commit code to git 
```

## OpenAPI Specifications

But now we should define the specification - the agreement between frontend and backend.
This specification gives explicit information about the endpoints, paths, request bodies, response bodies, and authentication rules.

Ask agent:
```
Read the frontend's API client in frontend/

Create openapi.yaml at the repository root.

Specify the backend this frontend expects: every endpoint, method, path, request body, response body, and which endpoints need authentication.
```

## The Backend

Now we have the OpenAPI specs and we can use this file to create the backend. We will use Python and FastAPI for that.

For FastAPI backends, we start with a mocked database, and then later change it to the real one.

Let’s ask the coding assistant to implement it:
```
Build a FastAPI backend in backend/ that implements the openapi.yaml spec.

Use an in-memory store and seed it with data so the frontend has something to show. Add authentication with hashed passwords and bearer tokens for the endpoints that need it.

Split the code into modules - routers, models, store, auth

Write tests
```

## Makefile
Normally, for FastAPI, the command to run the application is something like that:
```
cd backend
uv run uvicorn backend.main:app --reload --port 8091
```

But it's easier to ask agent to create a Makefile:
```
Create a Makefile so I can easily run it.
```

Then running it is as simple as:
```
make run
```

http://localhost:8091/docs will have OpenAPI specification

## Connecting Frontend and Backend
Ask agent:
```
Switch the frontend to use the real backend client.
```

Run and test application.
If something doesn’t work, describe and ask the agent to fix it.

## Database

At this point we have a working application with frontend and backend. They can connect to each other.

But because we don’t have a real database yet, when the backend is restarted, all the data is lost.

```
Replace the in-memory store with a database. Use SQLite and SQLAlchemy.
Use an environment variable to configure which DB the server should connect to.
Make it database-agnostic - later we will add support for other databases (e.g. Postgres).
```

Now we a have working app with SQLite database.
If something doesn't work, ask the coding agent to fix it.

## Ready for Deployment

We have a working app:
- from idea created a specification
- created frontend
- defined the API contract
- implemented the backend from the contract
- connected backend and frontend
- added SQLite for persistence

But this application is only working locally. Next, we need to deploy it.


Information based on [Build and Ship a Full-Stack App with AI Coding Assistants. Part 2](https://aishippingblog.com/p/build-and-ship-a-full-stack-app-with) and [Build and Ship a Full-Stack App with AI Coding Assistants - Alexey Grigorev](https://www.youtube.com/watch?v=x9dq5nBpDg8) from [AI Dev Tools Zoomcamp: AI-Native Software Engineering](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp)
