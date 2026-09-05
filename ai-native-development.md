# AI-Native Development: Specifications, Loop and Graph Engineering

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






Information based on [AI-Native Development. Part 1](https://aishippingblog.com/p/ai-native-development-specifications) and [How to Work with AI Coding Agents](https://www.youtube.com/watch?v=VUJxJGpaDEs) from [AI Dev Tools Zoomcamp: AI-Native Software Engineering](https://github.com/DataTalksClub/ai-dev-tools-zoomcamp)
