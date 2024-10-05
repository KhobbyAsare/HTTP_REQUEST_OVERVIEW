Using TanStack Query (formerly known as React Query) in Angular is a powerful way to manage server state and data fetching. TanStack Query provides hooks for fetching, caching, and synchronizing server data. Although TanStack Query is primarily designed for React, you can achieve similar functionality in Angular by using the underlying principles with Angular's capabilities.

### 1. Install TanStack Query for Angular

First, ensure you have the necessary packages installed. As of now, you might need to install `@tanstack/query-core` and set up your Angular application accordingly.

```bash
npm install @tanstack/query-core
```

### 2. Set Up TanStack Query in Angular

You'll need to create a service to handle the data fetching and state management. Below is a step-by-step approach.

**Step 1: Create a Query Client**

Create a new service to configure and provide the query client. This service will allow you to use TanStack Query throughout your application.

```typescript
// src/app/query-client.service.ts
import { Injectable } from '@angular/core';
import { QueryClient } from '@tanstack/query-core';

@Injectable({
  providedIn: 'root',
})
export class QueryClientService {
  private queryClient = new QueryClient();

  getClient() {
    return this.queryClient;
  }
}
```

**Step 2: Create a Data Service**

Create a data service for fetching data. This service will be used with TanStack Query.

```typescript
// src/app/data.service.ts
import { Injectable } from '@angular/core';
import { QueryClient } from '@tanstack/query-core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private apiUrl = 'https://api.example.com/data'; // Replace with your API URL

  constructor(private http: HttpClient, private queryClient: QueryClient) {}

  getData(): Observable<any> {
    return this.http.get<any>(this.apiUrl);
  }
}
```

**Step 3: Set Up a Component with TanStack Query**

In your component, you can use the query client to fetch data and manage loading/error states. You can use the `useQuery` function to fetch data.

```typescript
// src/app/data-fetching/data-fetching.component.ts
import { Component, OnInit } from '@angular/core';
import { QueryClient } from '@tanstack/query-core';
import { DataService } from '../data.service';

@Component({
  selector: 'app-data-fetching',
  template: `
    <div *ngIf="isLoading">Loading...</div>
    <div *ngIf="error">{{ error.message }}</div>
    <pre *ngIf="data">{{ data | json }}</pre>
  `,
})
export class DataFetchingComponent implements OnInit {
  data: any;
  error: any;
  isLoading: boolean = true;

  constructor(private dataService: DataService, private queryClient: QueryClient) {}

  ngOnInit() {
    this.queryClient
      .fetchQuery('dataKey', () => this.dataService.getData())
      .then((result) => {
        this.data = result;
        this.isLoading = false;
      })
      .catch((err) => {
        this.error = err;
        this.isLoading = false;
      });
  }
}
```

### 3. Using TanStack Query with Dependency Injection

If you're using Angular's dependency injection system, you might want to leverage `ngOnInit` and handle the query lifecycle effectively. Here’s an approach that uses `ngOnInit`:

```typescript
import { Component, OnInit } from '@angular/core';
import { QueryClient } from '@tanstack/query-core';
import { DataService } from '../data.service';

@Component({
  selector: 'app-data-fetching',
  template: `
    <div *ngIf="isLoading">Loading...</div>
    <div *ngIf="error">{{ error.message }}</div>
    <pre *ngIf="data">{{ data | json }}</pre>
  `,
})
export class DataFetchingComponent implements OnInit {
  data: any;
  error: any;
  isLoading: boolean = true;

  constructor(private dataService: DataService, private queryClient: QueryClient) {}

  ngOnInit() {
    const query = this.queryClient.fetchQuery('dataKey', () => this.dataService.getData());
    
    query.then(result => {
      this.data = result;
      this.isLoading = false;
    }).catch(err => {
      this.error = err;
      this.isLoading = false;
    });
  }
}
```

### Summary

1. **Set Up the Query Client**: Create a query client service to manage queries and provide it throughout your application.
2. **Data Service**: Use Angular's `HttpClient` to create a service that fetches data from your API.
3. **Component Implementation**: Use the query client in your components to manage fetching, loading, and error states.

### Conclusion

While TanStack Query is primarily built for React, you can still use its principles in Angular by implementing a similar structure. This allows you to manage server state and data fetching effectively in your Angular applications. If TanStack Query releases an official Angular package in the future, it will simplify this process even further.