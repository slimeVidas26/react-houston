# Getting Started with Create React App-aws-amplify

## Create React App

### `npx create-react-app react-gardena

### `cd react-fresno`

### `code .`

### `copy previous README file`



## Create Repo Github

### `git init`

### `git add .`

### `git commit -m "first commit"`

### `git remote add origin https://github.com/slimeVidas26/react-fresno.git`

### `git push -u origin master`

### `npm start`

## Make some change in app.js

## Configure Amplify

### `amplify configure`

## Initialize a new backend

### `amplify init`

## Install Amplify libraries

### `npm install aws-amplify @aws-amplify/ui-react@1.x.x`

## Set up frontend



## Create a GraphQL API and database

### `amplify add api`

### `amplify push`

### `amplify status`

To view the GraphQL API in the AppSync console at any time, run the following command:

### `amplify console api`

To view your entire app in the Amplify console at any time, run the following command:

### `amplify console`x

To test this out locally, you can run the mock command.

### `amplify mock api`

## Connect frontend to API

we need to configure Amplify on the client so that we can use it to interact with our backend services.

Open src/index.js and add the following code below the last import:

```javascript

import Amplify from "aws-amplify";
import awsExports from "./aws-exports";
Amplify.configure(awsExports);

```

Open src/App.js and replace it with the following code:

```javascript
/* src/App.js */
import React, { useEffect, useState } from 'react'
import Amplify, { API, graphqlOperation } from 'aws-amplify'
import { createTodo } from './graphql/mutations'
import { listTodos } from './graphql/queries'

import awsExports from "./aws-exports";
Amplify.configure(awsExports);

const initialState = { name: '', description: '' }

const App = () => {
  const [formState, setFormState] = useState(initialState)
  const [todos, setTodos] = useState([])

  useEffect(() => {
    fetchTodos()
  }, [])

  function setInput(key, value) {
    setFormState({ ...formState, [key]: value })
  }

  async function fetchTodos() {
    try {
      const todoData = await API.graphql(graphqlOperation(listTodos))
      const todos = todoData.data.listTodos.items
      setTodos(todos)
    } catch (err) { console.log('error fetching todos') }
  }

  async function addTodo() {
    try {
      if (!formState.name || !formState.description) return
      const todo = { ...formState }
      setTodos([...todos, todo])
      setFormState(initialState)
      await API.graphql(graphqlOperation(createTodo, {input: todo}))
    } catch (err) {
      console.log('error creating todo:', err)
    }
  }

  return (
    <div style={styles.container}>
      <h2>Amplify Todos</h2>
      <input
        onChange={event => setInput('name', event.target.value)}
        style={styles.input}
        value={formState.name}
        placeholder="Name"
      />
      <input
        onChange={event => setInput('description', event.target.value)}
        style={styles.input}
        value={formState.description}
        placeholder="Description"
      />
      <button style={styles.button} onClick={addTodo}>Create Todo</button>
      {
        todos.map((todo, index) => (
          <div key={todo.id ? todo.id : index} style={styles.todo}>
            <p style={styles.todoName}>{todo.name}</p>
            <p style={styles.todoDescription}>{todo.description}</p>
          </div>
        ))
      }
    </div>
  )
}

const styles = {
  container: { width: 400, margin: '0 auto', display: 'flex', flexDirection: 'column', justifyContent: 'center', padding: 20 },
  todo: {  marginBottom: 15 },
  input: { border: 'none', backgroundColor: '#ddd', marginBottom: 10, padding: 8, fontSize: 18 },
  todoName: { fontSize: 20, fontWeight: 'bold' },
  todoDescription: { marginBottom: 0 },
  button: { backgroundColor: 'black', color: 'white', outline: 'none', fontSize: 18, padding: '12px 0px' }
}

export default App

```

### `npm start`

## Authentication with Amplify

### `amplify add auth`

To deploy the service, run the push command:

### `amplify push`

Now, the authentication service has been deployed and you can start using it. To view the deployed services in your project at any time, go to Amplify Console by running the following command: ### `amplify console`

## Create login UI

Open src/App.js and make the following changes:

Import the withAuthenticator component:

```javascript
import { withAuthenticator } from '@aws-amplify/ui-react'

```

Change the default export to be the withAuthenticator wrapping the main component:

```javascript

export default withAuthenticator(App)

```

### `npm start`

## Add amplify Signout

```javascript
import { withAuthenticator , AmplifySignOut } from '@aws-amplify/ui-react'

      <AmplifySignOut/>


```
