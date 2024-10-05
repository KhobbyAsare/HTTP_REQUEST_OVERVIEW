In Next.js, you can make HTTP requests using methods like the Fetch API or Axios, similar to how you would in a standard React application. However, Next.js also offers features like API routes and server-side rendering (SSR) that can enhance your data-fetching strategies. Here’s how to handle requests using both methods in Next.js, along with best practices for error handling.

### 1. Using the Fetch API in Next.js

You can use the Fetch API in both client-side components and in server-side functions like `getServerSideProps` or `getStaticProps`.

#### Client-Side Fetching

```javascript
import React, { useEffect, useState } from 'react';

const DataFetchingComponent = () => {
    const [data, setData] = useState(null);
    const [error, setError] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await fetch('/api/data'); // API route in Next.js
                if (!response.ok) {
                    throw new Error(`HTTP error! Status: ${response.status}`);
                }
                const result = await response.json();
                setData(result);
            } catch (err) {
                setError(err.message);
            } finally {
                setLoading(false);
            }
        };

        fetchData();
    }, []);

    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;

    return (
        <div>
            <h1>Data:</h1>
            <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
    );
};

export default DataFetchingComponent;
```

#### Server-Side Fetching with `getServerSideProps`

This method allows you to fetch data on the server for each request, making it useful for dynamic pages.

```javascript
export async function getServerSideProps() {
    try {
        const response = await fetch('https://api.example.com/data');
        if (!response.ok) {
            throw new Error(`HTTP error! Status: ${response.status}`);
        }
        const data = await response.json();
        
        return {
            props: { data },
        };
    } catch (error) {
        return {
            props: { error: error.message },
        };
    }
}

const Page = ({ data, error }) => {
    if (error) return <div>Error: {error}</div>;

    return (
        <div>
            <h1>Data:</h1>
            <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
    );
};

export default Page;
```

### 2. Using Axios in Next.js

Axios can also be used in both client and server contexts. First, install Axios:

```bash
npm install axios
```

#### Client-Side Axios Fetching

```javascript
import React, { useEffect, useState } from 'react';
import axios from 'axios';

const DataFetchingComponent = () => {
    const [data, setData] = useState(null);
    const [error, setError] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await axios.get('/api/data'); // API route in Next.js
                setData(response.data);
            } catch (err) {
                if (err.response) {
                    setError(`Error: ${err.response.status} - ${err.response.data.message}`);
                } else if (err.request) {
                    setError('Network error. Please check your connection.');
                } else {
                    setError('An error occurred. Please try again.');
                }
            } finally {
                setLoading(false);
            }
        };

        fetchData();
    }, []);

    if (loading) return <div>Loading...</div>;
    if (error) return <div>Error: {error}</div>;

    return (
        <div>
            <h1>Data:</h1>
            <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
    );
};

export default DataFetchingComponent;
```

#### Server-Side Axios Fetching with `getServerSideProps`

```javascript
import axios from 'axios';

export async function getServerSideProps() {
    try {
        const response = await axios.get('https://api.example.com/data');
        return {
            props: { data: response.data },
        };
    } catch (error) {
        return {
            props: { error: error.message },
        };
    }
}

const Page = ({ data, error }) => {
    if (error) return <div>Error: {error}</div>;

    return (
        <div>
            <h1>Data:</h1>
            <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
    );
};

export default Page;
```

### 3. Using API Routes in Next.js

Next.js allows you to create API endpoints in the `pages/api` directory. You can use this feature to create your own serverless functions for data fetching.

#### Creating an API Route

```javascript
// pages/api/data.js

export default async function handler(req, res) {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();

        res.status(200).json(data);
    } catch (error) {
        res.status(500).json({ message: 'Error fetching data' });
    }
}
```

#### Fetching Data from the API Route

You can then fetch data from this API route as shown in the previous examples.

### Summary

- **Client-Side Fetching**: Use the Fetch API or Axios in React components.
- **Server-Side Fetching**: Use `getServerSideProps` for dynamic data fetching on each request.
- **Static Generation**: Consider using `getStaticProps` for static data fetching at build time.
- **API Routes**: Create your own API endpoints in Next.js to handle data fetching or processing.

By following these practices, you can efficiently manage data fetching in Next.js while ensuring a robust user experience.