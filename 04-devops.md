
# DevOps and Observability for an AI-Built App

In part 2, we developed an application. In part 3, we deployed it to a cloud environment and configured CI/CD.

In this article, we’ll make this deployment production-ready:
- Separate development and production environments
- Create a manual CI/CD trigger for promoting the app from development to production
- Use OpenTelemetry to collect metrics, traces, and logs and create
- Define application-specific metrics and create a dashboard that displays them
- Create an alert when the metrics change
- Use an AI agent as the on-call responder for the alerts

<img width="701" height="183" alt="image" src="/devops.png" />

## Recap

In part 2:
- We brainstormed with an AI assistant to come up with the specification
- Based on that we created a React-based frontend with no backend
- Next, we defined the frontend-backend OpenAPI schema
- Using the API contract we created the backend
- We added database support with SQLite and SQLAlchemy

In part 3:
- We containerized the application
- Then added Postgres support
- To make sure the system works reliably, we created integration and end-to-end tests
- We deployed it to AWS
- In the end, we set up CI/CD via GitHub Actions, so every push runs the tests and deploys the app to development


## Dev and prod environments





Information based on [DevOps and Observability for an AI-Built App. Part 4](https://aishippingblog.com/p/devops-and-observability-for-an-ai) and [DevOps and Observability for AI-Built Apps - Alexey Grigorev](https://www.youtube.com/watch?v=YkxLo_FRoQw) from [AI Dev Tools Zoomcamp: AI-Native Software Engineering](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp)
