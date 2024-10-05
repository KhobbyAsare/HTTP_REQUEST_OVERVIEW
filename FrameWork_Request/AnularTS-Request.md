In Angular (using TypeScript), you can handle HTTP requests using the built-in `HttpClient` service. This service makes it easy to interact with APIs and handle responses. Below, I’ll outline how to make HTTP requests using both the Fetch API and Axios in Angular, along with best practices for error handling.

### 1. Using Angular's HttpClient

**Step 1: Set Up HttpClient**

First, ensure that you have the `HttpClientModule` imported in your `AppModule`:

```typescript
import { HttpClientModule } from '@angular/common/http';
import { NgModule } from '@angular/core';

@NgModule({
  imports: [
    HttpClientModule,
    // other imports...
  ],
  // declarations, providers, etc.
})
export class AppModule {}
```

**Step 2: Create a Service**

It’s a good practice to create a service for handling HTTP requests. This keeps your components clean and focused.

```typescript
import { Injectable } from '@angular/core';
import { HttpClient, HttpErrorResponse } from '@angular/common/http';
import { Observable, throwError } from 'rxjs';
import { catchError } from 'rxjs/operators';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private apiUrl = 'https://api.example.com/data'; // Replace with your API URL

  constructor(private http: HttpClient) {}

  getData(): Observable<any> {
    return this.http.get<any>(this.apiUrl).pipe(
      catchError(this.handleError) // Handle errors
    );
  }

  private handleError(error: HttpErrorResponse) {
    // Handle different error cases
    let errorMessage = 'An unknown error occurred!';
    if (error.error instanceof ErrorEvent) {
      // Client-side error
      errorMessage = `Error: ${error.error.message}`;
    } else {
      // Server-side error
      errorMessage = `Error Code: ${error.status}\nMessage: ${error.message}`;
    }
    return throwError(errorMessage);
  }
}
```

**Step 3: Use the Service in a Component**

Now, you can use the `DataService` in any component:

```typescript
import { Component, OnInit } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-data-fetching',
  template: `
    <div *ngIf="loading">Loading...</div>
    <div *ngIf="error">{{ error }}</div>
    <pre *ngIf="data">{{ data | json }}</pre>
  `,
})
export class DataFetchingComponent implements OnInit {
  data: any;
  error: string | null = null;
  loading = true;

  constructor(private dataService: DataService) {}

  ngOnInit() {
    this.dataService.getData().subscribe({
      next: (result) => {
        this.data = result;
        this.loading = false;
      },
      error: (err) => {
        this.error = err;
        this.loading = false;
      },
    });
  }
}
```

### 2. Using Axios

If you prefer to use Axios instead, you can do so, but you'll need to install it first:

```bash
npm install axios
```

**Step 1: Create an Axios Service**

You can create a service for Axios requests as follows:

```typescript
import { Injectable } from '@angular/core';
import axios, { AxiosError } from 'axios';
import { Observable, throwError } from 'rxjs';

@Injectable({
  providedIn: 'root',
})
export class AxiosDataService {
  private apiUrl = 'https://api.example.com/data'; // Replace with your API URL

  getData(): Observable<any> {
    return new Observable((observer) => {
      axios.get(this.apiUrl)
        .then(response => {
          observer.next(response.data);
          observer.complete();
        })
        .catch((error: AxiosError) => {
          let errorMessage = 'An unknown error occurred!';
          if (error.response) {
            // Server-side error
            errorMessage = `Error Code: ${error.response.status}\nMessage: ${error.response.data}`;
          } else if (error.request) {
            // Client-side error
            errorMessage = 'Network error. Please check your connection.';
          }
          observer.error(errorMessage);
        });
    });
  }
}
```

**Step 2: Use the Axios Service in a Component**

You can use the `AxiosDataService` in a similar way:

```typescript
import { Component, OnInit } from '@angular/core';
import { AxiosDataService } from './axios-data.service';

@Component({
  selector: 'app-axios-fetching',
  template: `
    <div *ngIf="loading">Loading...</div>
    <div *ngIf="error">{{ error }}</div>
    <pre *ngIf="data">{{ data | json }}</pre>
  `,
})
export class AxiosFetchingComponent implements OnInit {
  data: any;
  error: string | null = null;
  loading = true;

  constructor(private axiosDataService: AxiosDataService) {}

  ngOnInit() {
    this.axiosDataService.getData().subscribe({
      next: (result) => {
        this.data = result;
        this.loading = false;
      },
      error: (err) => {
        this.error = err;
        this.loading = false;
      },
    });
  }
}
```

### Best Practices for Error Handling

1. **User-Friendly Messages**: Ensure error messages are user-friendly and do not expose technical details.
2. **Graceful Degradation**: Provide fallback options or UI states in case of errors, such as retry buttons or cached data.
3. **Loading States**: Implement loading indicators to improve user experience while waiting for data.
4. **Centralized Error Handling**: Consider centralizing error handling for HTTP requests to avoid repetitive code.

### Conclusion

- Use Angular's `HttpClient` for seamless integration and built-in features.
- If you prefer Axios, you can create a service that wraps Axios calls.
- Implement error handling and loading states to enhance user experience.
