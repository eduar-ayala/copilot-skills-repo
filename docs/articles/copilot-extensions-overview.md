# GitHub Copilot Extensions Overview

> **Reference Article** — Last reviewed: March 2025

GitHub Copilot Extensions allow organizations to build custom AI-powered tools that integrate directly into the Copilot Chat interface in VS Code, GitHub.com, and other surfaces. This article explains how extensions work, how to use existing ones, and how to build your own.

---

## What Are Copilot Extensions?

A **GitHub Copilot Extension** is a GitHub App that registers a **chat participant** — an `@mention`-able entity in Copilot Chat that can process user messages, call external services, and return AI-generated responses.

They enable use cases that go beyond what the built-in Copilot can do:
- Query your organization's internal knowledge base.
- Retrieve data from external APIs (issue trackers, monitoring systems, etc.).
- Enforce organization-specific policies or workflows.
- Provide domain-specific expertise (e.g., railway standards, proprietary frameworks).

---

## Extension Types

| Type | Description | Best For |
|---|---|---|
| **Skillset Extension** | Defines a set of functions Copilot can call. Simpler to build. | Integrating APIs and data retrieval |
| **Agent Extension** | Full control over request processing and response streaming. More powerful. | Complex, multi-step workflows |

---

## Using Copilot Extensions in VS Code

### Discovering Extensions

1. Go to [GitHub Marketplace → Copilot Extensions](https://github.com/marketplace?type=apps&copilot_app=true).
2. Browse available extensions. Examples:
   - **Sentry** — Query error reports from Copilot Chat.
   - **Datadog** — Monitor application performance.
   - **Octopus Deploy** — Manage deployments.

### Installing an Extension

1. Navigate to the extension on the GitHub Marketplace.
2. Click **"Set up a plan"** → **"Install it for free"** (or your plan).
3. Choose whether to install for your **personal account** or an **organization**.
4. Grant the required permissions.

### Using an Extension

In VS Code Copilot Chat, type:
```
@extension-name your question or command here
```

For example:
```
@sentry show me the most recent errors in production
@github search for open issues with label "safety-critical"
```

---

## Building a Copilot Extension for Hitachi Rail GTS

### Prerequisites

- A GitHub organization with Copilot Business or Enterprise.
- A publicly accessible HTTPS endpoint (use a cloud function, VM, or Azure App Service).
- Node.js, Python, or any language with an HTTP server.

### High-Level Architecture

```
VS Code (Copilot Chat)
      │
      │  @rail-standards query EN 50128 clause 6.7
      ▼
GitHub Copilot Platform
      │
      │  POST /agent  (SSE stream)
      ▼
Your Extension Server
      │
      │  Calls internal APIs / knowledge base
      ▼
Returns streamed response back to user
```

### Step 1 — Create a GitHub App

1. Go to **GitHub Organization Settings → Developer Settings → GitHub Apps → New GitHub App**.
2. Configure:
   - **GitHub App name**: `rail-standards-copilot`
   - **Homepage URL**: your organization URL
   - **Callback URL**: your server URL + `/callback`
   - **Copilot** section: Enable and set **Agent URL** to your server endpoint
3. Set permissions as needed (read access to your org's repositories, etc.).
4. Install the app in your organization.

### Step 2 — Implement the Agent Endpoint

**Example in Node.js using the official SDK:**

```javascript
import { createServer } from "node:http";
import {
  createAckEvent,
  createDoneEvent,
  createTextEvent,
  getUserMessage,
  verifyAndParseRequest,
} from "@copilot-extensions/preview-sdk";

const server = createServer(async (request, response) => {
  if (request.method !== "POST") {
    response.end("Only POST requests accepted");
    return;
  }

  // Verify the request comes from GitHub Copilot
  const { isValid, payload } = await verifyAndParseRequest(
    request,
    process.env.GITHUB_WEBHOOKS_SECRET
  );
  if (!isValid) {
    response.writeHead(401);
    response.end("Unauthorized");
    return;
  }

  const userMessage = getUserMessage(payload);

  // Your custom logic here
  const answer = await queryRailwayStandardsDatabase(userMessage);

  response.writeHead(200, { "Content-Type": "text/event-stream" });
  response.write(createAckEvent());
  response.write(createTextEvent(answer));
  response.write(createDoneEvent());
  response.end();
});

server.listen(3000);
```

### Step 3 — Deploy and Test

1. Deploy your server to a publicly accessible HTTPS endpoint.
2. Update the GitHub App with your production URL.
3. In VS Code, type `@your-extension-name hello` to test.

### Step 4 — Publish Internally

To make the extension available to all users in your organization:
1. Install the GitHub App at the **organization level**.
2. All organization members with Copilot access can now use `@your-extension-name`.

---

## Extension Security Considerations

- Always **verify request signatures** using the GitHub webhook secret.
- Never expose sensitive internal data unless the user is authenticated.
- Use **OAuth** callbacks to authenticate users if your extension accesses user-specific resources.
- Respect **data residency** requirements — be mindful of where your extension server is hosted.

---

## Potential Extensions for Hitachi Rail GTS

| Extension Name | Description |
|---|---|
| `@rail-standards` | Query EN 50128, EN 50657, and CENELEC clauses |
| `@requirements-db` | Search and retrieve requirements from the internal DOORS / ReqIF database |
| `@safety-checklist` | Generate a SIL-specific safety checklist for the current code context |
| `@test-runner` | Trigger CI/CD pipeline runs and report results in chat |
| `@incident-tracker` | Retrieve open incidents from the fault management system |

---

## References

- [GitHub Copilot Extensions documentation](https://docs.github.com/en/copilot/building-copilot-extensions/about-building-copilot-extensions)
- [Copilot Extensions preview SDK (npm)](https://www.npmjs.com/package/@copilot-extensions/preview-sdk)
- [GitHub Marketplace — Copilot Extensions](https://github.com/marketplace?type=apps&copilot_app=true)
- [GitHub Blog: Introducing Copilot Extensions](https://github.blog/2024-05-21-introducing-github-copilot-extensions/)
- [Building a GitHub Copilot Extension workshop](https://github.com/github-samples/copilot-extension-workshop)
