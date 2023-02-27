# Doc covers tanstack query v4

Website - [https://tanstack.com/query/latest](https://tanstack.com/query/latest)

---

## **Installation**

Use based on the package manager

```bash
npm i @tanstack/react-query
# or
yarn add @tanstack/react-query
```

Also install axios if not installed

```bash
npm i axios
# or
yarn add axios
```

## **Setting up**

```jsx
import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from "@tanstack/react-query";

const queryClient = new QueryClient();

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  );
}
```

Need to set some default options on the QueryClient object. These default options can be overridden individually when using `useQuery()`

```jsx
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      refetchOnWindowFocus: false,
    },
  },
});
```

## **Using `useQuery()`**

`useQuery()` hook is used to fetch data from an API

```jsx
const result = useQuery({
  queryKey: ["todos"],
  queryFn: async () => {
    return axios.get(`${api}/todos`);
  },
});
```

### `queryKey`

- needs to be unique
- to fetch one todo we can use the following queryKey with the id of the todo

```jsx
useQuery({ queryKey: ['todo', 5], ... })
```

### `queryFn`

- Ideally this should be created in a different file and import here

```ts
// apiCalls.ts

const fetchTodos = async () => {
  return axios.get(`${api}/todos`);
};

export { fetchTodos };
```

```jsx
// Component.tsx

import {fetchTodos} from './apiCalls.ts'

function Component() {
  const result = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });

  return (
    ...
  )
}

```

## **Full Example**

- `useQuery()` result can be destructured as shown below

```jsx
function Todos() {
  const { isLoading, isError, data, error } = useQuery({
    queryKey: ["todos"],
    queryFn: fetchTodos,
  });

  if (isLoading) return <div>Loading..</div>;

  if (isError) return <div>{error.message}</div>;

  return (
    <ul>
      {data.map((todo) => (
        <li key={todo.id}>{todo.title}</li>
      ))}
    </ul>
  );
}
```

## **Using `useMutation()`**

To send data to an API

- Here the mutationFn can also be defined in apiCalls.ts file and import here

```jsx
// To create the mutation
const mutation = useMutation({
  mutationFn: (data) => {
    return axios.post(`${api}/todos`, data);
  },
  onSuccess: (data) => {
    // if the request is successfull handle here
  },
  onError: (error) => {
    // if the request failed for some reason handle here
  },
});

// to execute the mutation in form submit
mutation.mutate({
  name: "My First Todo",
  isCompleted: false,
});
```

## **Full Example**

?
