When working with React.js, you can make HTTP requests using several methods, including the Fetch API, Axios, or even libraries like `react-query`. Here’s how to handle requests with each of these methods, along with best practices for error handling.

### 1. Using the Fetch API

The Fetch API is built into modern browsers and is a great option for making requests. Here’s how to use it in a React component:

```javascript
import React, { useEffect, useState } from 'react';

const DataFetchingComponent = () => {
    const [data, setData] = useState(null);
    const [error, setError] = useState(null);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        const fetchData = async () => {
            try {
                const response = await fetch('https://api.example.com/data');
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
    }, []); // Empty dependency array means it runs once after the initial render

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

### 2. Using Axios

Axios is a popular library for making HTTP requests. It provides a cleaner API and better error handling out of the box.

First, install Axios:

```bash
npm install axios
```

Then, use it in your component:

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
                const response = await axios.get('https://api.example.com/data');
                setData(response.data);
            } catch (err) {
                if (err.response) {
                    // Server responded with a status other than 2xx
                    setError(`Error: ${err.response.status} - ${err.response.data.message}`);
                } else if (err.request) {
                    // Request was made but no response was received
                    setError('Network error. Please check your connection.');
                } else {
                    // Something else happened
                    setError('An error occurred. Please try again.');
                }
            } finally {
                setLoading(false);
            }
        };

        fetchData();
    }, []); // Empty dependency array means it runs once after the initial render

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

### 3. Using `react-query`

`react-query` is a powerful library for managing server state in React applications. It simplifies data fetching, caching, and synchronizing server state.

First, install `react-query`:

```bash
npm install @tanstack/react-query
```

Then, use it in your component:

```javascript
import React from 'react';
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

const fetchData = async () => {
    const { data } = await axios.get('https://api.example.com/data');
    return data;
};

const DataFetchingComponent = () => {
    const { data, error, isLoading } = useQuery('dataKey', fetchData);

    if (isLoading) return <div>Loading...</div>;
    if (error) return <div>Error: {error.message}</div>;

    return (
        <div>
            <h1>Data:</h1>
            <pre>{JSON.stringify(data, null, 2)}</pre>
        </div>
    );
};

export default DataFetchingComponent;
```

### Summary

- **Fetch API**: Built into browsers, great for simple requests. Use async/await and handle errors manually.
- **Axios**: A more powerful library with a cleaner API and built-in error handling. Ideal for larger applications.
- **react-query**: Excellent for managing server state, handling caching, and providing a simple API for fetching data.

By following these patterns, you can handle requests effectively in React while ensuring a smooth user experience through proper error handling and loading states.