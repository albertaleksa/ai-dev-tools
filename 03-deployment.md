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
Create a Dockerfile that builds the frontend with Node,
then builds a Python image with the backend and the frontend static files.

Backend should serve the frontend.
```

Ask coding agent to build a Dockerfile:
```
I want you to build a Dockerfile from Dockerfile and run it.
Update README.md file and put there instructions for running and building a Dockerfile.
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
Create integration tests that run against docker-compose.yaml.
What scenarios should we test?
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

To deploy we can use Render, Railway, Fly.io, or any other managed container system.
You can ask the coding assistant to recommend an environment for your application:
```
Now I want to deploy an application. What are the options I have?
```

Ask your assistant to deploy it:
```
Deploy this application to AWS. Use AWS CloudFormation.
```
For that to work, you need to have an AWS user. 

## CI/CD with GitHub Actions

We used a user with the admin permissions to deploy the application. It’s okay for the first deployment, but only when we carefully watch it. The next step is to configure CI/CD and remove that access.

- Continuous integration (CI) means that every time we make a change and push it to GitHub, we automatically run all the tests to make sure we didn’t break anything.
- Continuous deployment (CD) is about deploying this change automatically.
- 
In GitHub, we use GitHub Actions for that.

Let’s configure it. Every time we make a push to main, we want to:
- run frontend and backend tests
- build the containers
- run the integration tests
- run the end-to-end tests
- if all the tests pass, deploy the new version

For the last step, the runner (the process that will deploy the application) will need to be able to access our AWS infrastructure. We will use OpenID Connect (OIDC) for this: the runner will assume a role with the necessary permissions and update the application.

Create the role and the workflow:
```
Create a CI/CD pipeline that:

- runs backend and frontend tests in parallel
- builds the Docker Compose stack and runs integration and end-to-end tests against it
- deploys to AWS using a GitHub OIDC role
- validates that the deploy is successful by checking the health endpoint
```

Ask agent to add this role to the CloudFormation and make sure that this role has the least amount of permissions it needs do the development, no extra rights.

Then disable admin permissions for the agent that it can deploy only using CI/CD.

## Clean up

When we’re done, we need to delete all the resources created by CloudFormation:
```
aws cloudformation delete-stack --stack-name <stack-name>
aws cloudformation wait stack-delete-complete --stack-name <stack-name>
```



Information based on [Deploy a Full-Stack App with AI Coding Assistants. Part 3](https://aishippingblog.com/p/deploy-a-full-stack-app-with-ai-coding) and [Test, Containerize, and Deploy an AI-Assisted App - Alexey Grigorev]([https://www.youtube.com/watch?v=x9dq5nBpDg8](https://www.youtube.com/watch?v=gxt5ZDVnBMM)) from [AI Dev Tools Zoomcamp: AI-Native Software Engineering](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp)
