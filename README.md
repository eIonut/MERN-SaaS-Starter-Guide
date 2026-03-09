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

## Project Structure

This project uses a monorepo structure and it represents the bare minimum EXAMPLE of your project(may not contain all the files).
   
    your-project
    │
    ├── /frontend
        ├── /src
             ├── /__tests__
             ├── /components
             ├── /contexts
             ├── /graphql (if applicable)
             ├── /hooks
             ├── /routes
             ├── /types
             ├── /utils
             ├── /lib
             App.tsx
             index.tsx
    │   .env.development
    │   .env.test
    │   .gitignore
    │   tsconfig.json
    │   package.json
    │   vite.config.ts
    │   vitest.config.js
    │
    ├── /backend
        ├── /src
             ├── /controllers
             └── /crons
             └── /middlewares
             └── /migrations
             └── /routes
             └── /scripts (bash scripts if applicable)
             └── /services
             └── /utils
             └── /models (if not using GraphQL)
             └── app.ts
             └── database.ts
             └── index.ts
             └── migrations.ts
    │   .env
    │   .gitignore
    │   package.json
    │   tsconfig.json
    │   vitest.config.js
    │
    ├── /infra
        ├── /backend
              ├── Dockerfile
        ├── /docs
              ├── deployment-guide.md
              ├── aws-configuration-guide.md
        ├── /frontend
              ├── Dockerfile
        ├── /monitoring
              ├── prometheus.yaml
        └── /nginx
              ├── nginx.conf
        └── deploy.sh
        └── provision.sh
        └── docker-compose.yml
        .env.dev.example
        .dockerignore
        .gitignore
    ├── package.json 
    ├── .gitignore
    └── README.md

## REST API Example

### Routes:

routes/projectRoutes.ts

- GET /projects
- POST /projects
- UPDATE /projects/:id
- DELETE /projects/:id

Controller naming example:

controllers/projects.ts

Responsibilities:

- parse request
- call services
- return response

## GraphQL Example

If using GraphQL, define schema.

  ```graphql/schema/projectSchema.js```

Example:

```
type Project {
  id: ID
  name: String
}
```

Resolver:

```graphql/resolvers/projectResolver.js```

GraphQL allows clients to request only required data.

## Frontend Architecture

Frontend uses Next.js / React.

Instead of using global state libraries (Redux, Zustand), this architecture uses:

- React Query for server state

- React state for UI state

HTTP requests are handled using Axios.

## Axios Setup

```lib/axios.ts

import axios from "axios"

export const api = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL,
  withCredentials: true,
})
```

## React Query Setup

```lib/react-query.ts

import { QueryClient } from "@tanstack/react-query"

export const queryClient = new QueryClient()
```

## React Query Hooks

```hooks/useProjects.ts

import { useQuery } from "@tanstack/react-query"
import { getProjects } from "../services/projectApi"

export function useProjects() {
  return useQuery({
    queryKey: ["projects"],
    queryFn: getProjects
  })
}
```

## Frontend Testing (Vitest)

Testing uses:

- Vitest

- React Testing Library

- jsdom

## Install Testing Libraries

```
npm install -D vitest \
@testing-library/react \
@testing-library/jest-dom \
jsdom
```

## Vitest Configuration

```vitest.config.ts
import { defineConfig } from "vitest/config"

export default defineConfig({
  test: {
    environment: "jsdom",
    globals: true
  }
})
```

## Example Test

```tests/button.test.tsx
import { render, screen } from "@testing-library/react"
import Button from "@/components/ui/button"

test("renders button", () => {
  render(<Button>Click</Button>)

  expect(screen.getByText("Click")).toBeInTheDocument()
})
```

## Infrastructure

Infrastructure is fully containerized.

Services run inside Docker containers and are orchestrated with Docker Swarm.

### Server Provision Script (infra/provision.sh)

Used when creating a new server.

```infra/provision.sh
#!/usr/bin/env bash

# System Update 1/7
# Docker engine Install 2/7
# UFW firewall - optional 3/7
# Docker Swarm Init 4/7
# TLS  Certificate - yourdomain.com 5/7
# Automatic certificate renewal - optional 6/7
# Create aplication directory and input docker swarm secrets 7/7
```

### Deploy Application (infra/deploy.sh)

```infra/deploy.sh
#!/usr/bin/env bash

# Login into dockerhub (you must provide access)
# Pull docker hub docker images
# Deploy docker swarm stack using the docker-compose.yml file
```

Optional: Add CI/CD - when pushing to the repo, deploy happens automatically.

### Manual Deployment Flow

Typical deployment process:

1️⃣ push code to GitHub
2️⃣ connect to server
3️⃣ run deploy.sh script

## Recommended Next Steps

After building the base architecture, add:

- authentication system
- payments (Stripe)
- object storage (S3) - for backups
