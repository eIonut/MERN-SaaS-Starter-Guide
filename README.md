# The MERN(Mongo, Express, React, NodeJs) Stack SaaS Guide
This repository acts as a guide that you can follow and use as an architecture guidance to build your first real big application using the MERN stacks. However this guide might be useful for other tech stacks as well.

A complete guide to building a real SaaS application using the MERN stack.
This repository demonstrates how to build a production-ready SaaS architecture, including:
- scalable frontend architecture
- maintainable backend services
- AI-assisted development workflow
- Docker-based infrastructure
- Docker Swarm deployment
- AWS / Hetzner hosting

Although this guide uses the MERN stack, the principles apply to any modern web stack.

This guide uses:
- Next.js / React
- Express
- Node.js
- MongoDB
- Docker + Docker Swarm
- AWS / Hetzner
  
No frontend state management libraries are used - but you can add your preffered one such as Zustand or Redux.

## Tech Stack and preffered libraries
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

#### Core stack and preffered libraries:

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
  baseURL: process.env.NEXT_PUBLIC_API_URL, // or import.meta.env.API_URL if you are using plain React
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

## Deploying the Application on Hetzner

### 1. Create a Hetzner Account

- Go to https://hetzner.com
- Create an account
- Add a payment method
- Open Hetzner Cloud Console

### 2. Create an SSH Key

Before creating a server, generate an SSH key locally.

```ssh-keygen -t ed25519 -C "your_email@example.com"```

This creates:

```
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

Copy the public key:

```cat ~/.ssh/id_ed25519.pub```

### 3. Add SSH Key to Hetzner

In the Hetzner Cloud console:

- Go to Security → SSH Keys

- Click Add SSH Key

- Paste the key

This allows passwordless server access.

### 4. Create a Server

In Hetzner Cloud:

Click Create Server

Recommended configuration:

#### Location

```FSN1 (Germany)```
or
```HEL1 (Finland)```

#### Image

Choose:

```Ubuntu 24.04```

#### Server Type

Good starting option:

```CPX21```

Example specs:

```
2 vCPU
4GB RAM
80GB SSD
```

#### SSH Keys

Select the SSH key you created earlier.

#### Server Name

Example:

```saas-prod-1```

Then click Create & Buy.

### 5. Configure the Hetzner Cloud Firewall

The Hetzner Firewall is a network-level firewall in front of the server (independent from UFW that runs on the OS). Use both for defence in depth.

1. Go to **Project → Firewalls → Create Firewall**.
2. Name: `yourproject-fw`.

### Inbound rules

| Protocol | Port | Source              | Purpose                      |
|----------|------|---------------------|------------------------------|
| TCP      | 22   | `0.0.0.0/0`, `::/0` | SSH                          |
| TCP      | 80   | `0.0.0.0/0`, `::/0` | HTTP (redirect + Let's Encrypt) |
| TCP      | 443  | `0.0.0.0/0`, `::/0` | HTTPS                        |

> **Block everything else.** In particular, never open port 27017 (MongoDB) or 2376/2377 (Docker). MongoDB must only be reachable inside the Docker overlay network.

### Outbound rules
Leave the default (allow all outbound). The server needs to reach DockerHub, Let's Encrypt, Google OAuth APIs, and Resend.

### Apply to server
1. Click **Apply to Servers** → select `yourapp-prod-1` → **Apply**.

### 6. Connect to the server

```bash
ssh -i ~/.ssh/your_ssh_key_name root@<server-ip>
```

For convenience, add a shortcut to `~/.ssh/config`:

```
Host yourshortcutname
  HostName <server-ip>
  User root
  IdentityFile ~/.ssh/your_ssh_key_name
```

Then connect with:

```bash
ssh yourshortcutname
```

---

### 7. What to do next

1. **Clone the repo** on the server:
   ```bash
   git clone <repo-url> /opt/yourapp
   cd /opt/yourapp
   ```
2. **Run the provision script** (DNS must already be pointing to this server):
   ```bash
   bash infra/provision.sh admin@yourdomain
   ```
3. **Follow `infra/docs/deployment-guide.md`** to build images and run deploy.sh to deploy the stack.

---

## 8. Optional: Add a volume for MongoDB

By default MongoDB data lives in the `mongo-data` Docker named volume on the server's root disk. For better durability and independent backup, attach a Hetzner Volume instead.
