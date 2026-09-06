# AI-Native Development: Specifications, Loop and Graph Engineering

There are multiple “engineering” levels when we work with coding agents:

- **Prompt engineering** - what we say when we interact with the agent
- **Context engineering** - what the agent knows before it starts and what it can get during the session
- **Loop engineering** - when it stops working
- **Graph engineering** - who does what when there’s more than one agent

## Specs before code

### Start in a chat assistant

```
I want to build a tool for <tools purpose, such as weekly feedback for projects>.

Help me set the scope for this project precisely. I want to brainstorm with you
and understand how the tool should work. Give me options.

Ask me one question at a time and keep your output short.
```

After answering some number of questions you can write:
```
I don't want to make decisions.
This should be enough for the MVP.
For the rest of the important things I want you to make some decisions and explain me
why you chose this decision, why you decided to go with this option and what were the other options you considered.
```

When we finish, I ask for a file with all the specifications:
```
Create a full scope for my project and save into a markdown file that I can download.
```
Download the file and save it as plan.md.

### Bootstrapping a project

Create a project from this specification:
```
mkdir project-name
cd project-name

git init
```

Copy the plan.md file:
```
mkdir -p _docs
mv ~/Downloads/plan.md _docs/plan.md
git add _docs/plan.md
git commit -m “Add project plan”
```

### Choose the stack and architecture

Run coding agent in current directory.

Ask the coding agent to come up with several options for tech stack:
```
Read _docs/plan.md. Propose multiple options for the tech stack and
explain each option.

Don't write code yet.
```

Choose tech stack or let the agent do it instead.
Then review the tach stack and tell the agent if don't need any of this or correct some.
Also, you can ask during the process about trade-off of the specific technology or alternatives.

Asl assistant to check newer versions of technologies in stack:
```
I want you for every single line of your technology choices
to see if there are newer versions available for that
```

### Turn the decisions into a backlog

Now that we’ve settled on the tech stack, we can ask the agent to decompose the specifications into a backlog with tasks:
```
Create a backlog with tasks in _docs/tasks.md.

Each task should be small enough to finish in one session, and
independent enough that I could hand it to someone who has not read
the others.

Use this template for each task:

## <number>. <title>
Goal: <one line>
Description: <two or three sentences on what the task involves>

The first task should be setting up an empty project with a passing test.

Don't write code yet.
```
It created these tasks.md.

Review the tasks and ask the agent to merge tasks that are too small or split tasks that don’t fit into one session. We want to create an MVP - the first version of the app. If something is out of scope for your vision of the MVP, remove it.

Select the best project name. Ask agent:
```
Can you please help me select the best name for our
tool that we're building, you can check plan.MD to see what
exactly we're building.
I want to have a nice
name that reflects the purpose of the project
```

Move tasks to a task tracker (GitHub issues).

```
Create a public GitHub repo for this project.
Move each task from _docs/tasks.md into a GitHub issue.
```

## Context engineering
The repository has a backlog now. When we start a new session, however, the agent doesn’t know which task we mean. It must figure that out every time.

These details go in AGENTS.md, which coding agents like Codex or OpenCode read when they start a new session.

Claude Code reads CLAUDE.md, while I use multiple coding assistants and want my workflow to be tool-agnostic.

That’s why I also create CLAUDE.md with a single line:
```
@AGENTS.md
```

### AGENTS.md
To make this context available in every new session, create AGENTS.md:
```
Create AGENTS.md with content similar to this


Commands

- `uv sync` - install dependencies
- `uv run pytest` - the whole suite
- `uv run pytest tests/test_home.py` - one test file

Rules

- Dependencies are added in `pyproject.toml`. Do not add one without
  asking
```

Review, ask if we need all rules:
```
Rules.
Do we really need all these rules?
Every agent session that starts in this repository needs information
from AGENTS.md file.
Keep only the most important ones
```

### The other documents

#### process.md - to describe how work is organized
Create _docs/process.md:
```
- Tasks are GitHub issues, one at a time
- Read the acceptance criteria before starting and before closing
- Commit regularly
```

As I continue working on a project, I may create other documents, such as:
- testing-guidelines.md for testing
- design-system.md so the UI doesn’t drift every session
- api.md, which describes what the API should look like

Add section in AGENTS.md:
```
Documents

- `_docs/process.md` - how work is organized
- Before writing tests, read `_docs/testing-guidelines.md`
- For anything touching the UI, read `_docs/design-system.md`
```

These documents are living documents and often updated.
If you need to correct an agent during a coding session, you can ask it to modify the documents.
Next time, it knows what you need, so you don’t have to correct it again.

You can use a prompt like this:
```
Based on the corrections I made, find the relevant documents and update them.
Commit the current work before changing the documents.
```

### Bootstrap the first task
With AGENTS.md and process.md in place, we can start a new session and ask the agent to implement the first task:
```
Implement task 1.
```

### Grooming: the product manager agent
This process is called “grooming”: we groom a task to make it more specific. Then an engineer can implement it without asking a single question.

Create a document:
```
_docs/team/
  pm.md
```

Inside, write the description for the product manager agent:
```
You’re a Product Manager

You groom a task before anyone implements it.

- Read the issue as written
- Rewrite it using the template in `_docs/task-template.md`
- Make the acceptance criteria checkable - someone should be able to
  point at the screen and say yes or no
- Think about the edge cases the person who filed it did not consider
- Do not write any code

Definition of done:

- The issue has all four sections filled in
- Every acceptance criterion can be checked by looking at the result
- Everything moved out of scope links to a follow-up issue
- An engineer who has never spoken to you could implement it from the
  issue and the documents it links

If something does not belong in this task, do not silently drop it.
File a follow-up issue and list it under out of scope with a link to
that issue, so it is clear what was moved and where it went.
```

A groomed task has four sections:
1. Goal - one or two sentences on what should be true afterwards.
2. Acceptance criteria - checkable statements.
3. Out of scope - what this change must not do.
4. Constraints - files it should stay inside, libraries it should or shouldn’t use, prior decisions it has to follow.

We save the issue template as _docs/task-template.md:
```
## Goal

One or two sentences on what should be true when this is done.

## Acceptance criteria

- [ ] A statement you can check by looking at the result
- [ ] One line per case, including the awkward ones

## Out of scope

- Something that does not belong in this task, moved to #TASK-NUMBER

## Constraints

- Files this should stay inside
- Libraries to use
- Guidelines to follow
```

We’ll need to groom every task, so we’ll add it to process.md:
```
Roles

- PM - grooms a task before anyone implements it, follows _docs/team/pm.md
```

We can create **outdated** folder in _docs and move plan.md and tasks.md files there.

Ask agent to groom one issue to see the results. Ask to use plan.md file from outdated folder if it needs some info.

## Loop engineering
The /goal command will prompt the agent to continue, so we won’t need to do it manually. Instead, we delegate that responsibility to the harness: the system around an agent, such as Claude Code or Codex. When the agent stops, the harness checks whether the condition has been met. If it hasn’t, the harness resumes the work.

Run:
```
/goal groom all issues
```

### Implementation: the software engineer agent
After grooming the issue, we can give it to a software engineer, the agent who will write the code.

Define the second role:
```
_docs/team/
  software-engineer.md
```

Put this definition inside:
```
You’re a Software Engineer

You implement one groomed task at a time.

- Read the issue and implement what it describes
- Implement against the acceptance criteria, do not change them
- Stay inside the files and constraints the issue names
- Write tests for what you built
- Do not close the issue
- Commit regularly

Definition of done:

- Every acceptance criterion in the issue is implemented
- Tests are written for the new behaviour, and the whole suite passes
- The work is committed
- The issue is still open, with a comment saying what you did

If an acceptance criterion is wrong, impossible, or contradicts
another one, create a comment on the issue about it.
```

Add one more line to process.md:
```
Roles

- PM - grooms a task before anyone implements it, follows _docs/team/pm.md
- Engineer - implements one groomed task, follows _docs/team/software-engineer.md
```

Then ask the agent to implement a task in a fresh session:
```
Implement issue #2
```

### Testing: the QA engineer agent






Information based on [AI-Native Development. Part 1](https://aishippingblog.com/p/ai-native-development-specifications) and [How to Work with AI Coding Agents](https://www.youtube.com/watch?v=VUJxJGpaDEs) from [AI Dev Tools Zoomcamp: AI-Native Software Engineering](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp)
