---
title: JavaScript code snippets
description: JavaScript code snippets
toc: true
tags:  [js, javascript, typescript]
series: ['cheatsheet']
categories: [js]
date: 2022-09-15
featuredImage: https://picsum.photos/700/238
draft: false
---


## Login

### client.ts

```typescript
// client.ts
import axios, { AxiosInstance } from 'axios';

export class Client {
  private _client: AxiosInstance;

  constructor(accessToken?: string, url?: string) {
    const apiUrl = this.selectApiTarget();
    let headers = {};
    if (accessToken !== undefined) {
      headers = {
        'Authorization': `Bearer ${accessToken}`
      };
    }
    this._client = axios.create({
      baseURL: url || apiUrl,
      headers: headers,
    });
  }

  private selectApiTarget(): string {
    let backendUrl = config.backend.url;
    if (window.location.host.includes("node.sharedtodos.com")) {
      backendUrl = config.backend.url.slice().replace("api.sharedtodos.com", "node-api.sharedtodos.com");
    }
    return `${backendUrl}/api/v1/`;
  }

  async getLoggedInUser(): Promise<User> {
    return await this._client.get('/user/me').then((response) => response.data);
  }

  async forgetLoggedInUser(): Promise<void> {
    return await this._client.delete('/user/me').then((response) => response.data);
  }

  async getTasks(listId: number): Promise<Task[]> {
    return await this._client.get(`boards/${listId}/tasks`).then((response) => response.data);
  }

  async deleteTask(listId: number, taskId: number) {
    return await this._client.delete(`boards/${listId}/tasks/${taskId}`).then((response) => response.data);
  }

  async createTask(listId: number, title: string, description: string) {
    const task: Task = {
      title: title,
      description: description,
    };
    return await this._client.post(`boards/${listId}/tasks`, task);
  }
  async updateTask(listId: number, taskId: string, task: Task) {
    return await this._client.put(`boards/${listId}/tasks/${taskId}`, task);
  }

  async login(email: string): Promise<string> {
    let data = new FormData();
    data.append('user_email', email);
    return await this._client.post(`login`, data, {
      headers: {'Content-Type': 'multipart/form-data' }
    }).then((response) => response.data.access_token);
  }
}

export const getClient = (accessToken?, url?): Client => new Client(accessToken, url);
```

### config.ts

```ts
// config.ts
interface ConfigOptions {
  backend: { url: string };
  auth0: any;
  authentication: { provider: string };
  authorization: { embedUrl: string };
}
declare global {
  interface Window {
    _env_: any;
  }
}

const Config: ConfigOptions = {
  backend: {
    url:
      process.env.REACT_APP_BACKEND_URL ||
      window?._env_?.BACKEND_URL ||
      "http://localhost:8008",
  },
  auth0: {
    domain:
      process.env.AUTH0_DOMAIN ||
      window?._env_?.AUTH0_DOMAIN ||
      "acalla-demoapp.us.auth0.com",
    clientId:
      process.env.AUTH0_CLIENT_ID ||
      window?._env_?.AUTH0_CLIENT_ID ||
      "myClientID",
    audience:
      process.env.AUTH0_AUDIENCE ||
      window?._env_?.AUTH0_AUDIENCE ||
      "https://demoapi.server.com/v1/",
  },
  authentication: {
    provider: "auth0",
  },
  authorization: {
    embedUrl: window?._env_?.AUTHZ_EMBED_URL || "http://localhost:3000",
  }
};

export default Config;
```



## Resources

- [react cheatsheet](https://devhints.io/react)