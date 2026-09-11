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

Let’s create one for this project too. Place it in the repository root:
```
for backend, use uv for dependency management. a few useful commands:

uv sync
uv add <PACKAGE-NAME>
uv run python <PYTHON-FILE>

regularly commit code to git 
```

## OpenAPI Specifications


Information based on [Build and Ship a Full-Stack App with AI Coding Assistants. Part 2](https://aishippingblog.com/p/build-and-ship-a-full-stack-app-with) and [Build and Ship a Full-Stack App with AI Coding Assistants - Alexey Grigorev](https://www.youtube.com/watch?v=x9dq5nBpDg8) from [AI Dev Tools Zoomcamp: AI-Native Software Engineering](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp)
