# The MERN Stack SaaS Guide
This repository acts as a guide that you can follow to build your first real big application using the MERN stacks. However this guide might be useful for other tech stacks as well.

A complete guide to building a real SaaS application using the MERN stack.
This repository demonstrates how to build a production-ready SaaS architecture, including:
- scalable frontend architecture
- maintainable backend services
- AI-assisted development workflow
- Docker-based infrastructure
- Docker Swarm deployment
- AWS / Hetzner hosting

Although this guide uses MERN, the principles apply to any modern web stack.

This guide uses:
- Next.js / React
- Express
- Node.js
- MongoDB
- Docker + Docker Swarm
- AWS / Hetzner
No frontend state management libraries are used - but you can add your preffered one such as Zustand or Redux.

## Tech Stack
### Frontend
* Next.js
* React
* TypeScript
* React Query
* Axios
* TailwindCSS
* React Hook Form / Formik
* Zod

#### Testing:
* Vitest
* React Testing Library

___

### Backend

#### Core stack:

* Node.js
* TypeScript
* Express
* MongoDB
* Mongoose

Optional middleware layer(more complex, but very useful):
* Apollo Server + GraphQL

___ 

### Infrastructure

#### Deployment:

* Docker
* Docker Swarm
* Nginx
* AWS or Hetzner
* Github Actions, Jenkins or GitLab CI/CD (CI/CD)

## AI Development Workflow

AI tools dramatically improve development speed when used correctly. This guide uses the following workflow.

### AI Tools
### 1. Design
Use: v0.dev or lovable.dev

These tools generate React/NextJS UI code from prompts.

Prompt example:


    
    I want you to create a mock application that will act as a design for me. The name of the app must be X. It should contain the following functionalities:
    
    - Internal company links grouped on projects(like jira link, gitlab repo, etc)
    
    - Customer support tickets are now sent to Slack but i want them to be on the app instead and can be filtered by priority (p1, p2, p3)
    
    - Other features that should replace some functionalities of Slack
    
    - The design should be very simple and dark mode only
    
    - The login should be done with company email(you should mock that) and the users will receive a OTP on email

Export/copy the generated React/NextJS code.

### 2. Coding
Use: Claude Code, Gemini, Cursor.

These tools are ideal for:
- refactoring UI
- generating backend endpoints
- writing tests
- codebase-wide edits
- creating new features
- basically everything code related

### 3. Typical AI Workflow

1️⃣ Design UI with v0.dev / lovable

2️⃣ Create a new GitHub/GitLab repository and import/copy UI code into the project

3️⃣ Refactor components and code using Claude Code / Gemini / Cursor

4️⃣ Generate backend code / endpoints

5️⃣ Have both services up and running and connected to each other

6️⃣ Write tests in Vitest using AI for both UI and API services
