# @ulupstudio/sdk

Official TypeScript SDK for the [UluP Spaces API](https://www.ulupspaces.com/openapi.yaml).

## Install

```bash
npm install @ulupstudio/sdk
```

## Quickstart

```ts
import { UlupSpacesClient } from '@ulupstudio/sdk';

const client = new UlupSpacesClient({
  apiKey: process.env.ULUP_API_KEY!, // generate one from Profile → API Keys on ulupspaces.com
});

const project = await client.createProject('Podcast Launch');
const node = await client.createNode(project.id, 'Recording');
await client.addTask(node.id, 'Record intro');
```

## Methods

| Method | What it does |
|---|---|
| `createProject(name)` | Create a new project |
| `listNodes(projectId)` | List a project's nodes (structure only) |
| `createNode(projectId, name, color?)` | Create a node, duplicate-checked by name |
| `getOverview(projectId)` | Full project state: nodes, task text, connections |
| `connectNodes(projectId, sourceName, targetName)` | Connect two nodes by name |
| `addTask(nodeId, content)` | Add a task to a node |
| `completeTask(taskId, completed?)` | Mark a task complete/incomplete (default: complete) |

## Errors

Every method throws `UlupSpacesError` on a non-2xx response, with `.message` and `.status`:

```ts
import { UlupSpacesError } from '@ulupstudio/sdk';

try {
  await client.createNode(999999, 'Recording');
} catch (err) {
  if (err instanceof UlupSpacesError) {
    console.error(err.status, err.message); // 404 "Project not found or not yours."
  }
}
```

## Regenerating types

`src/types.ts` is generated from [`openapi.yaml`](../openapi.yaml) — don't edit it by hand. After changing the spec:

```bash
npm run regen-types
npm run build
```

## License

MIT