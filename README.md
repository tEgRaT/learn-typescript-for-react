# Learn TypeScript for React

## Introduction to TypeScript

### Basic Types

```typescript
let num: number = 42; // Number
let name: string = "John Doe"; // String
let isActive: boolean = true; // Boolean
let scores: number[] = [90, 85, 80]; // Array
let user: [string, number] = ["Alice", 30]; // Tuple
// Enum
enum Color {
  Red,
  Green,
  Blue,
}
let c: Color = Color.Green;
// Any
let notSure: any = 4;
notSure = "maybe a string instead";
// Void
function logMessage(message: string): void {
  console.log(message);
}
```

### Type Inference

### Static Typing

#### Function Types

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

#### Optional and Default Parameters

```typescript
// Optional parameter
function greet(name: string, greeting?: string): void {
  console.log(`${greeting || "Hello"}, ${name}`);
}

// Default parameter
function multiply(a: number, b: number = 1): number {
  return a * b;
}
```

#### Interfaces

```typescript
// Defining an interface
interface Person {
  name: string;
  age: number;
  greet(): void;
}

const user: Person = {
  name: "Jane",
  age: 25,
  greet: () => console.log(`Hello, my name is ${user.name}`),
};

// Extending interface
interface Employee extends Person {
  employeeId: number;
}

const employee: Employee = {
  name: "John",
  age: 30,
  employeeId: 1234,
  greet: () =>
    console.log(
      `Hello, I am ${employee.name}, Employee ID: ${employee.employeeId}`
    ),
};
```

#### Classes

```typescript
// Defining a class
class Animal {
  constructor(public name: string) {}

  speak(): void {
    console.log(`${this.name} makes a noise.`);
  }
}

const dog = new Animal("Dog");
dog.speak(); // Outputs "Dog makes a noise."

// Inheritance
class Dog extends Animal {
  speak(): void {
    console.log(`${this.name} barks.`);
  }
}

const myDog = new Dog("Rex");
myDog.speak(); // Outputs "Rex barks."

// Access Modifiers

class Car {
  private engineType: string;

  constructor(engine: string) {
    this.engineType = engine;
  }

  public getEngineType(): string {
    return this.engineType;
  }
}
```

TypeScript supports access modifiers like `public`, `private`, and `protected` to control the visibility of class members.

- **public**: The default visibility; accessible anywhere.
- **private**: Accessible only within the class.
- **protected**: Accessible within the class and its subclasses.

## Setting Up TypeScript with React

```bash
pnpm create react-app my-app --template typescript
```

### Configuring TypeScript Features

Some common configurations in `tsconfig.json`:

1. **Strict Mode**: `"strict": true`

1. **Base URL and Paths**: For easier imports, developers can define a base URL and specific paths for modules. This is especially useful in large projects. For example:

   ```json
   {
     "compilerOptions": {
       "baseUrl": "src",
       "paths": {
         "@components/*": ["components/*"],
         "@hooks/*": ["hooks/*"],
         "@utils/*": ["utils/*"]
       }
     }
   }
   ```

1. **Type Definitions**: To install additional type definitions for libraries: `pnpm install @types/[library-name]`.

   ```bash
   pnpm install @types/react-router-dom
   ```

### Writing TypeScript in React Components

```tsx
import React from "react";

interface GreetingProps {
  name: string;
}

const Greeting: React.FC<GreetingProps> = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};

export default Greeting;
```

In this example:

- An interface `GreetingProps` is defined to the type `props` that the `Greeting` component will receive.
- The `React.FC` type is used to define the component, which provides type checking and auto-complete support for React functional components.

## TypeScript Basics for React

### Interfaces and Type Aliases

#### Using Type Aliases

Type aliases can be used to create a new name for existing types. They can also define union types, intersection types, and more.

```typescript
type UserID = number | string;

const userId: UserID = 1; // Valid
const userIdString: UserID = "123"; // Also valid
```

#### Enums in TypeScript

```typescript
enum Direction {
  Up = 1,
  Down,
  Left,
  Right,
}

let move: Direction = Direction.Up;
console.log(move); // Outputs: 1
```

### Generics

Generics provide a way to create reusable components that can work with any data type while maintaining type safety. It allows developers to define functions, classes, and interfaces that can operate on various types without losing the information about what that type is.

#### Example of Generics in Functions

```typescript
function identity<T>(arg: T): T {
  return arg;
}

let output = identity<string>("Hello, TypeScript!");
```

#### Example of Generics in Classes

```typescript
class Box<T> {
  private contents: T;

  constructor(value: T) {
    this.contents = value;
  }

  getContents(): T {
    return this.contents;
  }
}

const box = new Box<number>(123);
console.log(box.getContents()); // Outputs: 123
```

### TypeScript with React

#### Example of a Functional Component

```tsx
import React from "react";

interface GreetingProps {
  name: string;
  age: number;
}

const Greeting: React.FC<GreetingProps> = ({ name, age }) => {
  return (
    <h1>
      Hello {name}. You are {age} years old.
    </h1>
  );
};
```

#### Example of a Class Component

```tsx
import React, { Component } from "react";

interface CounterProps {
  initialCount?: number;
}

interface CounterState {
  count: number;
}

class Counter extends Component<CounterProps, CounterState> {
  constructor(props: CounterProps) {
    super(props);
    this.state = {
      count: props.initialCount || 0,
    };
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}
```

## Creating Your First TypeScript React Component

### Setting Up the Environment

```bash
pnpm create react-app greeting --template typescript
cd greeting
pnpm start
```

### Creating a Functional Component

```tsx
// src/Greeting.tsx
import React, { useState } from "react";

interface GreetingProps {
  name: string;
}

const Greeting: React.FC<GreetingProps> = ({ name }) => {
  const [message, setMessage] = useState<string>("Welcome!");

  const changeMessage = () => {
    setMessage(`Hello, ${name}!`);
  };

  return (
    <div>
      <h1>{message}</h1>
      <button onClick={changeMessage}>Greet</button>
    </div>
  );
};

export default Greeting;
```

```tsx
// src/App.tsx
import React from "react";
import Greeting from "./Greeting";

const App: React.FC = () => {
  return (
    <div>
      <Greeting name="John" />
    </div>
  );
};

export default App;
```

## Handling Events in TypeScript React

### Typing Events in TypeScript

TypeScript provides types for the synthetic events provided by React. The common event types include:

- `React.MouseEvent`
- `React.KeyboardEvent`
- `React.ChangeEvent`
- `React.FormEvent`
- `React.FocusEvent`

#### Example: Type a Mouse Event

```tsx
import React from "react";

const Button: React.FC = () => {
  const handleClick = (event: React.MouseEvent<HTMLButtonElement>) => {
    console.log("Button clicked", event);
  };

  return <button onClick={handleClick}>Click Me</button>;
};
```

In this example:

- `handleClick` is typed to accepts a `React.MouseEvent` where the target element is a `HTMLButtonElement`.
- This ensures that the `event` parameter has all the properties associated with a mouse event, including the `target` property.

### Typing Input Events

#### Typing a Change Event

```tsx
import React, { useState } from "react";

const TextInput: React.FC = () => {
  const [value, setValue] = useState<string>("");

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setValue(event.target.value);
  };

  return <input type="text" value={value} onChange={handleChange} />;
};
```

### Handling Keyboard Events

#### Example: Typing a Keyboard Event

```tsx
import React from "react";

const KeyPressExample: React.FC = () => {
  const handleKeyPress = (event: React.KeyboardEvent<HTMLInputElement>) => {
    if (event.key === "Enter") {
      console.log("Enter key pressed");
    }
  };

  return <input type="text" onKeyPress={handleKeyPress} />;
};
```

### Form Event Handling

#### Example: Typing a Form Event

```tsx
import React, { useState } from "react";

const FormExample: React.FC = () => {
  const [inputValue, setInputValue] = useState<string>("");

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    setInputValue(event.target.value);
  };

  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log("Form submitted with value: ", inputValue);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" value={inputValue} onChange={handleChange} />
      <button type="submit">Submit</button>
    </form>
  );
};
```

## Managing Forms with TypeScript

### Basic Form Component Structure

```tsx
import React, { useState } from "react";

interface FormData {
  name: string;
  email: string;
}

const MyForm: React.FC = () => {
  const [formData, setFormData] = useState<FormData>({ name: "", email: "" });

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.target;
    setFormData({ ...formData, [name]: value });
  };

  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    console.log(formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name:</label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
        />
      </div>
      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};

export default MyForm;
```

### Implementing Validation

```typescript
import React, { useState } from "react";

interface FormState {
  name: string;
  email: string;
}

interface Errors {
  name?: string;
  email?: string;
}

const errorStyles = {
  color: "red",
};

const MyForm: React.FC = () => {
  const [formState, setFormState] = useState<FormState>({
    name: "",
    email: "",
  });
  const [errors, setErrors] = useState<Errors>({});

  const validate = () => {
    const errors: Errors = {};

    if (!formState.name) {
      errors.name = "Name is required";
    }

    if (!formState.email) {
      errors.email = "Email is required";
    } else if (!/\S+@\S+\.\S+/.test(formState.email)) {
      errors.email = "Email is invalid";
    }

    return errors;
  };

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.target;

    setFormState((prevState) => ({ ...prevState, [name]: value }));
    setErrors((prevErrors) => ({ ...prevErrors, [name]: undefined })); // Clear error message when user starts typing
  };

  const handleSubmit = (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();

    const validationErrors = validate();

    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
    } else {
      console.log("Form submitted: ", formState);
      // You can send formState to an API here
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name: </label>
        <input
          type="text"
          name="name"
          id="name"
          value={formState.name}
          onChange={handleChange}
        />
        {errors.name && <span style={errorStyles}>{errors.name}</span>}
      </div>
      <div>
        <label htmlFor="email">Email: </label>
        <input
          type="email"
          name="email"
          id="email"
          value={formState.email}
          onChange={handleChange}
        />
        {errors.email && <span style={errorStyles}>{errors.email}</span>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
};

export default MyForm;
```

### Managing Input Types and State

#### Example of Checkbox and Select Inputs

```tsx
interface FormData {
  name: string;
  email: string;
  acceptTerms: boolean;
  favoriteColor: string;
}

const [formData, setFormData] = useState<FormData>({
  name: "",
  email: "",
  acceptTerms: false,
  favoriteColor: "red",
});

const handleChange = (
  event: React.ChangeEvent<HTMLInputElement | HTMLSelectElement>
) => {
  const { name, type, value, checked } = event.target;

  setFormData({
    ...formData,
    [name]: type === "checkbox" ? checked : value,
  });
};

return (
  <form onSubmit={handleSubmit}>
    {/* Previous input fields */}
    <label>
      Accept Terms:
      <input
        type="checkbox"
        name="acceptTerms"
        check={formData.acceptTerms}
        onChange={handleChange}
      />
    </label>
    <label>
      Favorite Color:
      <select
        name="favoriteColor"
        value={formData.favoriteColor}
        onChange={handleChange}
      >
        <option value="red">Red</option>
        <option value="green">Green</option>
        <option value="blue">Blue</option>
      </select>
    </label>
    <button type="submit">Submit</button>
  </form>
);
```

### Managing Form Submission with Asynchronous Calls

```tsx
import React, { useState } from "react";

interface FormData {
  name: string;
  email: string;
}

const UserForm: React.FC = () => {
  const [formData, setFormData] = useState<FormData>({ name: "", email: "" });
  const [error, setError] = useState<string | null>(null);
  const [loading, setLoading] = useState<boolean>(false);

  const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.target;

    setFormData((prevState) => ({
      ...prevState,
      [name]: value,
    }));
  };

  const handleSubmit = async (event: React.FormEvent<HTMLFormElement>) => {
    event.preventDefault();

    setLoading(true);
    setError(null);

    // Simple validation
    if (!formData.name || !formData.email) {
      setError("Both fields are required.");
      setLoading(false);
      return;
    }

    try {
      const response = await fetch(
        "https://jsonplaceholder.typicode.com/posts",
        {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(formData),
        }
      );

      if (!response.ok) {
        throw new Error("Network response was not ok");
      }

      const result = await response.json();
      console.log("Success:", result);
      // Optionally reset the form
      setFormData({ name: "", email: "" });
    } catch (error) {
      console.error("Error during submission:", error);
      setError("There was an error submitting the form.");
    } finally {
      setLoading(false);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label>
          Name:
          <input
            type="text"
            name="name"
            value={formData.name}
            onChange={handleChange}
          />
        </label>
      </div>
      <div>
        <label>
          Email:
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
          />
        </label>
      </div>
      {error && <p style={{ color: "red" }}>{error}</p>}
      <button type="submit" disabled={loading}>
        {loading ? "Submitting..." : "Submit"}
      </button>
    </form>
  );
};

export default UserForm;
```

### Advanced Form Management with Libraries

While managing forms manually is effective for simple forms, complexity can increase for larger applications. Libraries like **Formik** and **React Hook Form** can simplify form management significantly.

#### Example: Formik with TypeScript

```tsx
import React from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";
import * as Yup from "yup";

interface FormValues {
  name: string;
  email: string;
}

const validationSchema = Yup.object().shape({
  name: Yup.string().required("Name is required"),
  email: Yup.string()
    .email("Invalid email format")
    .required("Email is required"),
});

const MyFormikForm: React.FC = () => {
  const initialValues: FormValues = {
    name: "",
    email: "",
  };

  const handleSubmit = (values: FormValues) => {
    console.log("Form submitted: ", values);
  };

  return (
    <Formik
      initialValues={initialValues}
      validationSchema={validationSchema}
      onSubmit={handleSubmit}
    >
      {() => (
        <Form>
          <div>
            <label htmlFor="name">Name:</label>
            <Field type="text" name="name" id="name" />
            <ErrorMessage name="name" component="span" />
          </div>
          <div>
            <label htmlFor="email">Email:</label>
            <Field type="email" name="email" id="email" />
            <ErrorMessage name="email" component="span" />
          </div>
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  );
};

export default MyFormikForm;
```

## React Router with TypeScript

### Setting up React Router in a TypeScript Project

```bash
pnpm install react-router-dom @types/react-router-dom
```

### Basic Routing Setup

```tsx
import React from "react";
import { BrowserRouter as Router, Routes, Route } from "react-router-dom";
import Home from "./Home";
import About from "./About";

const App: React.FC = () => {
  return (
    <Router>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
      </Routes>
    </Router>
  );
};

export default App;
```

### Defining Route Parameters

React Router allows the definition of dynamic routes using parameters.

```tsx
<Route path="/user/:id" element={<User />} />
```

You can access route parameters using the `useParams` hook.

```tsx
import React from "react";
import { useParams } from "react-router-dom";

const User: React.FC = () => {
  const { id } = useParams<{ id: string }>();

  return (
    <div>
      <h1>User ID: {id}</h1>
    </div>
  );
};

export default User;
```

### Type-Safe Navigation

#### Using the Link Component

```tsx
import { Link } from "react-router-dom";

const Navigation: React.FC = () => {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/user/123">User 123</Link>
    </nav>
  );
};
```

### Managing Route Props

#### Using Route Props

```tsx
const UserWrapper: React.FC = (props) => {
  const { id } = useParams<{ id: string }>();

  return <User id={id} {...props} />;
};

<Route path="/user/:id" element={<UserWrapper />} />;
```

## State Management with Context API and TypeScript

### Understanding the React Context API

The React Context API allows developers to share values across the component tree without having to pass props down manually at every level. It is particularly useful for managing global state - such as user authentication, themes, or language preferences - where a single source of truth is necessary.

#### Key Concepts

- **Context**: A React object that holds the current value of the context.
- **Provider**: A component that makes the context value available to its children.
- **Consumer**: A component that subscribes to context changes.

### Creating a Context

To create a context, developers first need to define an interface that specifies the state shape. This enhances type safety and provides a clear contract for what the context will manage.

```typescript
// types.ts
export interface AuthContextType {
  isAuthenticated: boolean;
  login: () => void;
  logout: () => void;
}
```

### Creating the Context and Provider Component

With the interface defined, developers can create the context itself and a provider component that holds the state and methods.

```tsx
// AuthProvider.tsx
import React, { createContext, useContext, useState } from "react";
import { AuthContextType } from "./types";

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: React.ReactNode }> = ({
  children,
}): => {
    const [isAuthenticated, setIsAuthenticated] = useState<boolean>(false);

    const login = () => setIsAuthenticated(true);
    const logout = () => setIsAuthenticated(false);

    return (
      <AuthContext.Provider value={{isAuthenticated, login, logout}}>
        {children}
      </AuthContext.Provider>
    );
};

// custom hook for easier access to context
export const useAuth = (): AuthContextType => {
  const context = useContext(AuthContext);

  if (context === undefined) {
    throw new Error("useAuth must be used within an AuthProvider");
  }

  return context;
};
```

### Using the Context in Components

Once the provider is set up, any component within the tree can access the context values using the `useAuth` hook.

```tsx
// LoginButton.tsx
import React from "react";
import { useAuth } from "./AuthProvider";

const LoginButton: React.FC = () => {
  const { isAuthenticated, login } = useAuth();

  return (
    <button onClick={login} disabled={isAuthenticated}>
      {isAuthenticated ? "Logged In" : "Login"}
    </button>
  );
};
```

```tsx
// LogoutButton.tsx
import React from "react";
import { useAuth } from "./AuthProvider";

const LogoutButton: React.FC = () => {
  const { isAuthenticated, logout } = useAuth();

  return (
    <button onClick={logout} disabled={!isAuthenticated}>
      {isAuthenticated ? "Logout" : "Logged Out"}
    </button>
  );
};
```

### Wrapping the Application with the Provider

To ensure that all components can access the context, the application must be wrapped with the `AuthProvider` at a high level (usually in `index.tsx` or `App.tsx`).

```tsx
// App.tsx
import React from "react";
import { AuthProvider } from "./AuthProvider";
import LoginButton from "./LoginButton";
import LogoutButton from "./LogoutButton";

const App: React.FC = () => {
  return (
    <AuthProvider>
      <h1>Authentication Example</h1>
      <LoginButton />
      <LogoutButton />
    </AuthProvider>
  );
};

export default App;
```

## Using TypeScript with Redux

### Defining Actions

#### Define Action Types

```ts
// src/redux/actionTypes.ts
export const INCREMENT = "INCREMENT";
export const DECREMENT = "DECREMENT";
```

#### Create Action Creators

```ts
// src/redux/actions.ts
import { INCREMENT, DECREMENT } from "./actionTypes";

interface IncrementAction {
  type: typeof INCREMENT;
}

interface DecrementAction {
  type: typeof DECREMENT;
}

export type CounterActionTypes = IncrementAction | DecrementAction;

export const increment = (): IncrementAction => ({
  type: INCREMENT,
});

export const decrement = (): DecrementAction => ({
  type: DECREMENT,
});
```

### Creating Reducers

#### Define the State Interface

```ts
// src/redux/reducers.ts
import { CounterActionTypes, INCREMENT, DECREMENT } from "./actions";

interface CounterState {
  count: number;
}

const initialState: CounterState = {
  count: 0,
};

const counterReducer = (
  state = initialState,
  action: CounterActionTypes
): CounterState => {
  switch (action.type) {
    case INCREMENT:
      return { ...state, count: state.count + 1 };
    case DECREMENT:
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
};

export default counterReducer;
```

#### Combine Reducers (If Necessary)

If multiple reducers are defined, combine them using the `combineReducers` functon from Redux:

```ts
import { combineReducers } from "redux";

const rootReducer = combineReducers({
  counter: counterReducer,
  // other reducers can go here
});

export default rootReducer;
```

### Configuring the Redux Store

```ts
// src/redux/store.ts
import { createStore } from "redux";
import rootReducer from "./reducers";

const store = createStore(rootReducer);

export default store;
```

### Connecting Redux with React Components

To make Redux state available to React components, the user must wrap the application in the `Provider` component from `react-redux` and pass the store as a prop:

In the `index.tsx` file, import the `Provider` and the store:

```tsx
// src/index.tsx
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import store from "./redux/store";
import App from "./App";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

#### Connecting a Component to the Store

To connect a component to the Redux store, the user can use the `useSelector` and `useDispatch` hooks from `react-redux`:

```tsx
// src/components/Counter.tsx
import React from "react";
import { useSelector, useDispatch } from "react-redux";
import { increment, decrement } from "../redux/actions";

const Counter: React.FC = () => {
  const count = useSelector((state: any) => state.counter.count);
  const dispatch = useDispatch();

  return (
    <div>
      <h1>{count}</h1>
      <button onClick={() => dispatch(increment())}>Increment</button>
      <button onClick={() => dispatch(decrement())}>Decrement</button>
    </div>
  );
};

export default Counter;
```

### Best Practices

1. **Type Safety**: Always define action types and use them consistently thoughout the application to maintain type safety.
1. **Modular Structure**: Organize code into separate files for actions, reducers, and store setup enhance maintainability.
1. **Avoid State Mutations**: Alway return a new state object in reducers to ensure that state updates are predictable.
1. **Use Selector Functions**: Create and use selector functions for accessing state to keep components decoupled from the Redux store structure.

## Testing TypeScript React Components

### Setting up the Environment

#### Installing Required Packages

```bash
pnpm install --save-dev jest @types/ject ts-jest react-testing-library @testing-library/react @testing-library/jest-dom
```

- **jest**: A JavaScript testing framework.
- **@types/jest**: Type definitions for jest, ensuring type safety.
- **ts-jest**: A preprocessor that compiles TypeScript code before running tests with Jest.
- **react-testing-library**: A library for testing React components.
- **@testing-library/react**: An updated package for testing React components.
- **@testing-library/jest-dom**: Provides custom Jest matchers for asserting on DOM testing.

#### Configuring Jest

Next, the Jest configuration needs to be set up. In your `package.json`, add the following configuration:

```json
{
  "ject": {
    "preset": "ts-jest",
    "testEnvironment": "jsdom"
  }
}
```

This configuration tells Jest to to use `ts-jest` for TypeScript support and sets the test environment to `jsdom`, which simulates a browser environment.

### Writing Tests for React Components

#### Creating a Simple React Component

```tsx
// Greeting.tsx
import React from "react";

interface GreetingProps {
  name: string;
}

const Greeting: React.FC<GreetingProps> = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};

export default Greeting;
```

#### Writing Tests for the Component

```tsx
// Greeting.test.tsx
import React from "react";
import { render, screen } from "@testing-library/react";
import Greeting from "./Greeting";

describe("Greeting Component", () => {
  it("renders correctly for valid props", () => {
    const name: string = "Alice"; // Type safety is enforced here
    render(<Greeting name={name} />);
    expect(screen.getByText(/hello, alice!/i)).toBeInTheDocument();
  });

  it("renders the greeting with the provided name", () => {
    render(<Greeting name="John" />);
    const greetingElement = screen.getByRole("heading", {
      name: /hello, john!/i,
    });
    expect(greetingElement).toBeInTheDocument();
  });

  it("does not render the greeting for an empty name", () => {
    render(<Greeting name="" />);
    const greetingElement = screen.queryByRole("heading");
    expect(greetingElement).not.toBeInTheDocument();
  });
});
```

### Running Tests

```bash
pnpm test
```

### Testing User Interactions

#### Example with User Interactions

```tsx
// GreetingWithButton.tsx
import React, { useState } from "react";

const GreetingWithButton: React.FC = () => {
  const [name, setName] = useState<string>("John");

  return (
    <div>
      <h1>Hello, {name}!</h1>
      <button onClick={() => setName("Alice")}>Change Name</button>
    </div>
  );
};

export default GreetingWithButton;
```

#### Writing Tests for User Interactions

```tsx
// GreetingWithButton.test.tsx
import React from "react";
import { render, screen, fireEvent } from "@testing-library/react";
import GreetingWithButton from "./GreetingWithButton";

describe("GreetingWithButton Component", () => {
  it("changes the name when button is clicked", () => {
    render(<GreetingWithButton />);

    const buttonElement = screen.getByRole("button", { name: /change name/i });
    fireEvent.click(buttonElement);

    const greetingElement = screen.getByRole("heading", {
      name: /hello, alice!/i,
    });
    expect(greetingElement).toBeInTheDocument();
  });
});
```

## Real-Life Project Example: Building a To-Do List Application

## Real-Life Project Example: Developing a Weather Deshboard

## Real-Life Project Example: E-Commerce Product List
