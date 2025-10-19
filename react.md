# React Cheat Sheet Complet 2025

## Table des matières
- [Installation et Setup](#installation-et-setup)
- [Composants](#composants)
- [JSX](#jsx)
- [Props](#props)
- [State](#state)
- [Hooks](#hooks)
- [Hooks Avancés](#hooks-avancés)
- [Nouveaux Hooks React 18+](#nouveaux-hooks-react-18)
- [Event Handling](#event-handling)
- [Conditional Rendering](#conditional-rendering)
- [Listes et Keys](#listes-et-keys)
- [Forms](#forms)
- [Lifecycle](#lifecycle)
- [Context API](#context-api)
- [Refs](#refs)
- [React Server Components](#react-server-components)
- [Suspense et Streaming](#suspense-et-streaming)
- [Performance](#performance)
- [Patterns Courants](#patterns-courants)
- [Best Practices](#best-practices)

---

## Installation et Setup

### Créer une nouvelle app React

```bash
# Avec Vite (recommandé)
npm create vite@latest mon-app -- --template react
cd mon-app
npm install
npm run dev

# Avec Create React App (legacy)
npx create-react-app mon-app
cd mon-app
npm start

# Avec Next.js (pour SSR/SSG)
npx create-next-app@latest mon-app
cd mon-app
npm run dev
```

### Installation manuelle

```bash
npm install react react-dom
# Pour TypeScript
npm install -D @types/react @types/react-dom
```

---

## Composants

### Function Component (moderne)

```jsx
// Composant simple
function Greeting() {
  return <h1>Hello, World!</h1>;
}

// Avec props
function Greeting({ name }) {
  return <h1>Hello, {name}!</h1>;
}

// Arrow function
const Greeting = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};

// Return implicite
const Greeting = ({ name }) => <h1>Hello, {name}!</h1>;
```

### Class Component (legacy, éviter)

```jsx
class Greeting extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

### Export/Import

```jsx
// Export par défaut
export default function App() {
  return <div>App</div>;
}

// Export nommé
export function Button() {
  return <button>Click</button>;
}

// Import
import App from './App';
import { Button } from './Button';
```

---

## JSX

### Syntaxe de base

```jsx
// Expression JavaScript dans JSX
const name = "Alice";
const element = <h1>Hello, {name}!</h1>;

// Attributs
const element = <img src={user.avatarUrl} alt={user.name} />;

// className au lieu de class
const element = <div className="container">Content</div>;

// Style inline (objet JavaScript)
const element = <div style={{ color: 'red', fontSize: '20px' }}>Text</div>;

// Fragments
return (
  <>
    <h1>Title</h1>
    <p>Paragraph</p>
  </>
);

// Ou avec Fragment explicite
return (
  <React.Fragment>
    <h1>Title</h1>
    <p>Paragraph</p>
  </React.Fragment>
);

// Fragments avec keys
return (
  <>
    {items.map(item => (
      <React.Fragment key={item.id}>
        <dt>{item.term}</dt>
        <dd>{item.description}</dd>
      </React.Fragment>
    ))}
  </>
);
```

### Expressions JSX

```jsx
// Ternaire
const element = <div>{isLoggedIn ? <UserGreeting /> : <GuestGreeting />}</div>;

// AND logique
const element = <div>{isLoggedIn && <UserGreeting />}</div>;

// Fonctions
const element = <div>{formatName(user)}</div>;

// Commentaires
const element = (
  <div>
    {/* Ceci est un commentaire */}
    <h1>Title</h1>
  </div>
);
```

---

## Props

### Props de base

```jsx
// Passer des props
<Greeting name="Alice" age={25} />

// Recevoir des props
function Greeting({ name, age }) {
  return <h1>Hello, {name}! You are {age} years old.</h1>;
}

// Props avec valeurs par défaut
function Greeting({ name = "Guest", age = 0 }) {
  return <h1>Hello, {name}!</h1>;
}

// Spread operator
const props = { name: "Alice", age: 25 };
<Greeting {...props} />
```

### Props.children

```jsx
// Composant wrapper
function Card({ children }) {
  return <div className="card">{children}</div>;
}

// Utilisation
<Card>
  <h2>Title</h2>
  <p>Content</p>
</Card>
```

### Props destructuring

```jsx
// Destructuring dans les paramètres
function User({ name, email, avatar }) {
  return (
    <div>
      <img src={avatar} alt={name} />
      <h2>{name}</h2>
      <p>{email}</p>
    </div>
  );
}

// Avec rest operator
function Button({ children, ...rest }) {
  return <button {...rest}>{children}</button>;
}
```

### Prop Types (TypeScript recommandé)

```jsx
// Avec PropTypes (legacy)
import PropTypes from 'prop-types';

function Greeting({ name, age }) {
  return <h1>Hello, {name}!</h1>;
}

Greeting.propTypes = {
  name: PropTypes.string.isRequired,
  age: PropTypes.number
};

// Avec TypeScript (moderne)
interface GreetingProps {
  name: string;
  age?: number;
}

function Greeting({ name, age }: GreetingProps) {
  return <h1>Hello, {name}!</h1>;
}
```

---

## State

### useState Hook

```jsx
import { useState } from 'react';

// State simple
function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

// State avec objet
function Form() {
  const [user, setUser] = useState({ name: '', email: '' });
  
  const handleChange = (e) => {
    setUser({ ...user, [e.target.name]: e.target.value });
  };
  
  return (
    <form>
      <input name="name" value={user.name} onChange={handleChange} />
      <input name="email" value={user.email} onChange={handleChange} />
    </form>
  );
}

// State avec fonction (lazy initialization)
function ExpensiveComponent() {
  const [data, setData] = useState(() => {
    return computeExpensiveValue();
  });
}

// Functional update (basé sur le state précédent)
function Counter() {
  const [count, setCount] = useState(0);
  
  const increment = () => {
    setCount(prevCount => prevCount + 1);
  };
}
```

### Multiples states

```jsx
function Form() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  
  // OU avec un seul state objet
  const [form, setForm] = useState({ name: '', email: '', age: 0 });
}
```

---

## Hooks

### useEffect

```jsx
import { useEffect } from 'react';

// Effect basique (s'exécute après chaque render)
useEffect(() => {
  document.title = `Count: ${count}`;
});

// Effect avec cleanup
useEffect(() => {
  const subscription = subscribeToData();
  
  return () => {
    subscription.unsubscribe();
  };
});

// Effect avec dependencies (s'exécute uniquement si count change)
useEffect(() => {
  fetchData(count);
}, [count]);

// Effect une seule fois au mount (array vide)
useEffect(() => {
  fetchData();
}, []);

// Exemples pratiques
function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  
  useEffect(() => {
    let cancelled = false;
    
    async function fetchUser() {
      const response = await fetch(`/api/users/${userId}`);
      const data = await response.json();
      
      if (!cancelled) {
        setUser(data);
      }
    }
    
    fetchUser();
    
    return () => {
      cancelled = true;
    };
  }, [userId]);
  
  return <div>{user?.name}</div>;
}
```

### useContext

```jsx
import { createContext, useContext } from 'react';

// Créer un context
const ThemeContext = createContext('light');

// Provider
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

// Consommer le context
function ThemedButton() {
  const theme = useContext(ThemeContext);
  return <button className={theme}>Click me</button>;
}

// Exemple avec state
const UserContext = createContext();

function App() {
  const [user, setUser] = useState(null);
  
  return (
    <UserContext.Provider value={{ user, setUser }}>
      <Dashboard />
    </UserContext.Provider>
  );
}

function Profile() {
  const { user } = useContext(UserContext);
  return <div>{user?.name}</div>;
}
```

### useReducer

```jsx
import { useReducer } from 'react';

// Reducer function
function counterReducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return { count: 0 };
    default:
      throw new Error(`Unknown action: ${action.type}`);
  }
}

// Utilisation
function Counter() {
  const [state, dispatch] = useReducer(counterReducer, { count: 0 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>+</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>-</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
}

// Avec payload
function todoReducer(state, action) {
  switch (action.type) {
    case 'add':
      return [...state, action.payload];
    case 'remove':
      return state.filter(todo => todo.id !== action.payload);
    case 'toggle':
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    default:
      return state;
  }
}
```

### useRef

```jsx
import { useRef } from 'react';

// Référence DOM
function TextInput() {
  const inputRef = useRef(null);
  
  const focusInput = () => {
    inputRef.current.focus();
  };
  
  return (
    <>
      <input ref={inputRef} />
      <button onClick={focusInput}>Focus</button>
    </>
  );
}

// Valeur persistante (ne déclenche pas de re-render)
function Timer() {
  const intervalRef = useRef(null);
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    intervalRef.current = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
    
    return () => clearInterval(intervalRef.current);
  }, []);
  
  return <div>Count: {count}</div>;
}

// Stocker valeur précédente
function usePrevious(value) {
  const ref = useRef();
  
  useEffect(() => {
    ref.current = value;
  }, [value]);
  
  return ref.current;
}
```

### useMemo

```jsx
import { useMemo } from 'react';

// Mémoriser un calcul coûteux
function ExpensiveComponent({ data, filter }) {
  const filteredData = useMemo(() => {
    console.log('Filtering...');
    return data.filter(item => item.includes(filter));
  }, [data, filter]);
  
  return <List items={filteredData} />;
}

// Mémoriser un objet pour éviter les re-renders
function Parent() {
  const [count, setCount] = useState(0);
  
  const config = useMemo(() => ({
    color: 'blue',
    size: 'large'
  }), []); // Objet stable
  
  return <Child config={config} />;
}
```

### useCallback

```jsx
import { useCallback } from 'react';

// Mémoriser une fonction
function Parent() {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    console.log('Clicked!');
  }, []); // Fonction stable
  
  return <Child onClick={handleClick} />;
}

// Avec dependencies
function SearchBox({ onSearch }) {
  const [query, setQuery] = useState('');
  
  const handleSearch = useCallback(() => {
    onSearch(query);
  }, [query, onSearch]);
  
  return (
    <>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <button onClick={handleSearch}>Search</button>
    </>
  );
}
```

---

## Hooks Avancés

### useImperativeHandle

```jsx
import { useImperativeHandle, forwardRef, useRef } from 'react';

// Exposer des méthodes personnalisées à un parent
const FancyInput = forwardRef((props, ref) => {
  const inputRef = useRef();
  
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    },
    clear: () => {
      inputRef.current.value = '';
    }
  }));
  
  return <input ref={inputRef} />;
});

// Utilisation
function Parent() {
  const inputRef = useRef();
  
  return (
    <>
      <FancyInput ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
      <button onClick={() => inputRef.current.clear()}>Clear</button>
    </>
  );
}
```

### useLayoutEffect

```jsx
import { useLayoutEffect, useRef } from 'react';

// S'exécute de manière synchrone après les mutations DOM
function Tooltip() {
  const ref = useRef();
  const [position, setPosition] = useState({ x: 0, y: 0 });
  
  useLayoutEffect(() => {
    const { height, width } = ref.current.getBoundingClientRect();
    setPosition({ x: width / 2, y: height });
  }, []);
  
  return <div ref={ref}>Tooltip</div>;
}
```

### useDebugValue

```jsx
import { useDebugValue } from 'react';

// Pour les hooks personnalisés (visible dans React DevTools)
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);
  
  useDebugValue(isOnline ? 'Online' : 'Offline');
  
  useEffect(() => {
    // Subscribe logic
  }, [friendID]);
  
  return isOnline;
}
```

---

## Nouveaux Hooks React 18+

### useTransition

```jsx
import { useTransition, useState } from 'react';

// Marquer les updates comme non-urgentes
function SearchResults() {
  const [isPending, startTransition] = useTransition();
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  
  const handleChange = (e) => {
    const value = e.target.value;
    setQuery(value); // Urgent
    
    startTransition(() => {
      // Non-urgent - peut être interrompu
      setResults(searchData(value));
    });
  };
  
  return (
    <div>
      <input value={query} onChange={handleChange} />
      {isPending && <Spinner />}
      <Results data={results} />
    </div>
  );
}
```

### useDeferredValue

```jsx
import { useDeferredValue, useState } from 'react';

// Reporter un update non-urgent
function SearchResults() {
  const [query, setQuery] = useState('');
  const deferredQuery = useDeferredValue(query);
  
  const results = useMemo(() => {
    return searchData(deferredQuery);
  }, [deferredQuery]);
  
  return (
    <div>
      <input value={query} onChange={e => setQuery(e.target.value)} />
      <Results data={results} />
    </div>
  );
}
```

### useId

```jsx
import { useId } from 'react';

// Générer des IDs uniques pour l'accessibilité
function TextField({ label }) {
  const id = useId();
  
  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input id={id} />
    </div>
  );
}

// Avec plusieurs IDs
function Form() {
  const id = useId();
  
  return (
    <form>
      <label htmlFor={`${id}-name`}>Name</label>
      <input id={`${id}-name`} />
      
      <label htmlFor={`${id}-email`}>Email</label>
      <input id={`${id}-email`} />
    </form>
  );
}
```

### useSyncExternalStore

```jsx
import { useSyncExternalStore } from 'react';

// S'abonner à un store externe
function useOnlineStatus() {
  const isOnline = useSyncExternalStore(
    subscribe,
    getSnapshot,
    getServerSnapshot
  );
  
  function subscribe(callback) {
    window.addEventListener('online', callback);
    window.addEventListener('offline', callback);
    return () => {
      window.removeEventListener('online', callback);
      window.removeEventListener('offline', callback);
    };
  }
  
  function getSnapshot() {
    return navigator.onLine;
  }
  
  function getServerSnapshot() {
    return true; // Toujours en ligne côté serveur
  }
  
  return isOnline;
}
```

### useInsertionEffect

```jsx
import { useInsertionEffect } from 'react';

// Pour les bibliothèques CSS-in-JS (très spécifique)
function useCSS(rule) {
  useInsertionEffect(() => {
    const style = document.createElement('style');
    style.textContent = rule;
    document.head.appendChild(style);
    
    return () => {
      document.head.removeChild(style);
    };
  }, [rule]);
}
```

### use (React 19+)

```jsx
import { use } from 'react';

// Lire une Promise ou un Context dans le render
function Comments({ commentsPromise }) {
  const comments = use(commentsPromise);
  
  return (
    <div>
      {comments.map(comment => (
        <Comment key={comment.id} {...comment} />
      ))}
    </div>
  );
}

// Avec Context
function MyComponent() {
  const theme = use(ThemeContext);
  return <div className={theme}>Content</div>;
}
```

### useOptimistic (React 19+)

```jsx
import { useOptimistic } from 'react';

// Updates optimistes
function TodoList({ todos }) {
  const [optimisticTodos, addOptimisticTodo] = useOptimistic(
    todos,
    (state, newTodo) => [...state, { ...newTodo, pending: true }]
  );
  
  async function addTodo(formData) {
    const newTodo = { id: Date.now(), text: formData.get('text') };
    addOptimisticTodo(newTodo);
    
    await saveTodo(newTodo);
  }
  
  return (
    <div>
      {optimisticTodos.map(todo => (
        <div key={todo.id} style={{ opacity: todo.pending ? 0.5 : 1 }}>
          {todo.text}
        </div>
      ))}
      <form action={addTodo}>
        <input name="text" />
        <button>Add</button>
      </form>
    </div>
  );
}
```

### useFormStatus (React 19+)

```jsx
import { useFormStatus } from 'react-dom';

// Status d'un formulaire
function SubmitButton() {
  const { pending, data, method, action } = useFormStatus();
  
  return (
    <button disabled={pending}>
      {pending ? 'Submitting...' : 'Submit'}
    </button>
  );
}

function MyForm() {
  async function handleSubmit(formData) {
    await saveData(formData);
  }
  
  return (
    <form action={handleSubmit}>
      <input name="username" />
      <SubmitButton />
    </form>
  );
}
```

### useActionState (React 19+)

```jsx
import { useActionState } from 'react';

// Gérer l'état d'une action
function LoginForm() {
  const [state, formAction] = useActionState(login, { error: null });
  
  async function login(previousState, formData) {
    try {
      await signIn(formData);
      return { error: null };
    } catch (error) {
      return { error: error.message };
    }
  }
  
  return (
    <form action={formAction}>
      <input name="username" />
      <input name="password" type="password" />
      {state.error && <p>{state.error}</p>}
      <button>Login</button>
    </form>
  );
}
```

---

## Event Handling

### Événements de base

```jsx
// Click
<button onClick={handleClick}>Click me</button>

// Change
<input onChange={handleChange} />

// Submit
<form onSubmit={handleSubmit}>
  <button type="submit">Submit</button>
</form>

// Mouse events
<div
  onMouseEnter={handleMouseEnter}
  onMouseLeave={handleMouseLeave}
  onClick={handleClick}
  onDoubleClick={handleDoubleClick}
>
  Hover me
</div>

// Keyboard events
<input
  onKeyDown={handleKeyDown}
  onKeyUp={handleKeyUp}
  onKeyPress={handleKeyPress}
/>

// Focus events
<input
  onFocus={handleFocus}
  onBlur={handleBlur}
/>
```

### Event handlers

```jsx
function Button() {
  // Handler simple
  function handleClick() {
    alert('Clicked!');
  }
  
  // Handler avec paramètre
  function handleClickWithParam(id) {
    console.log('Clicked item:', id);
  }
  
  // Handler inline
  return (
    <>
      <button onClick={handleClick}>Click</button>
      <button onClick={() => handleClickWithParam(123)}>Click with param</button>
      <button onClick={(e) => console.log(e.target)}>Click inline</button>
    </>
  );
}
```

### Event object

```jsx
function Form() {
  function handleSubmit(e) {
    e.preventDefault(); // Empêcher le comportement par défaut
    const formData = new FormData(e.target);
    console.log(Object.fromEntries(formData));
  }
  
  function handleClick(e) {
    e.stopPropagation(); // Empêcher la propagation
    console.log(e.target); // Élément cible
    console.log(e.currentTarget); // Élément avec le handler
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input name="username" />
      <button onClick={handleClick}>Submit</button>
    </form>
  );
}
```

---

## Conditional Rendering

```jsx
// If-else avec ternaire
function Greeting({ isLoggedIn }) {
  return (
    <div>
      {isLoggedIn ? <UserGreeting /> : <GuestGreeting />}
    </div>
  );
}

// AND logique (&&)
function Mailbox({ unreadMessages }) {
  return (
    <div>
      <h1>Hello!</h1>
      {unreadMessages.length > 0 && (
        <h2>You have {unreadMessages.length} unread messages.</h2>
      )}
    </div>
  );
}

// Null pour ne rien rendre
function Warning({ warn }) {
  if (!warn) {
    return null;
  }
  
  return <div className="warning">Warning!</div>;
}

// Variable
function LoginButton({ isLoggedIn, onLogin, onLogout }) {
  let button;
  
  if (isLoggedIn) {
    button = <button onClick={onLogout}>Logout</button>;
  } else {
    button = <button onClick={onLogin}>Login</button>;
  }
  
  return <div>{button}</div>;
}

// Switch case dans une fonction
function StatusMessage({ status }) {
  function getMessage() {
    switch (status) {
      case 'loading':
        return <Spinner />;
      case 'success':
        return <SuccessMessage />;
      case 'error':
        return <ErrorMessage />;
      default:
        return null;
    }
  }
  
  return <div>{getMessage()}</div>;
}
```

---

## Listes et Keys

```jsx
// Liste basique
function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}

// Avec index (seulement si pas d'ID unique)
function List({ items }) {
  return (
    <ul>
      {items.map((item, index) => (
        <li key={index}>{item}</li>
      ))}
    </ul>
  );
}

// Avec composant
function TodoItem({ todo }) {
  return <li>{todo.text}</li>;
}

function TodoList({ todos }) {
  return (
    <ul>
      {todos.map(todo => (
        <TodoItem key={todo.id} todo={todo} />
      ))}
    </ul>
  );
}

// Filtrer et mapper
function FilteredList({ items, filter }) {
  return (
    <ul>
      {items
        .filter(item => item.includes(filter))
        .map(item => (
          <li key={item.id}>{item.text}</li>
        ))}
    </ul>
  );
}

// Extraire le JSX dans une variable
function TodoList({ todos }) {
  const listItems = todos.map(todo => (
    <li key={todo.id}>{todo.text}</li>
  ));
  
  return <ul>{listItems}</ul>;
}
```

---

## Forms

### Controlled Components

```jsx
// Input simple
function NameForm() {
  const [name, setName] = useState('');
  
  function handleSubmit(e) {
    e.preventDefault();
    console.log('Submitted:', name);
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={name}
        onChange={e => setName(e.target.value)}
      />
      <button type="submit">Submit</button>
    </form>
  );
}

// Multiple inputs
function Form() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    age: 0
  });
  
  function handleChange(e) {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
  }
  
  return (
    <form>
      <input name="name" value={formData.name} onChange={handleChange} />
      <input name="email" value={formData.email} onChange={handleChange} />
      <input name="age" type="number" value={formData.age} onChange={handleChange} />
    </form>
  );
}

// Textarea
function EssayForm() {
  const [essay, setEssay] = useState('');
  
  return (
    <textarea
      value={essay}
      onChange={e => setEssay(e.target.value)}
    />
  );
}

// Select
function FlavorForm() {
  const [flavor, setFlavor] = useState('coconut');
  
  return (
    <select value={flavor} onChange={e => setFlavor(e.target.value)}>
      <option value="grapefruit">Grapefruit</option>
      <option value="lime">Lime</option>
      <option value="coconut">Coconut</option>
      <option value="mango">Mango</option>
    </select>
  );
}

// Checkbox
function CheckboxForm() {
  const [isChecked, setIsChecked] = useState(false);
  
  return (
    <input
      type="checkbox"
      checked={isChecked}
      onChange={e => setIsChecked(e.target.checked)}
    />
  );
}

// Radio buttons
function RadioForm() {
  const [selected, setSelected] = useState('option1');
  
  return (
    <div>
      <label>
        <input
          type="radio"
          value="option1"
          checked={selected === 'option1'}
          onChange={e => setSelected(e.target.value)}
        />
        Option 1
      </label>
      <label>
        <input
          type="radio"
          value="option2"
          checked={selected === 'option2'}
          onChange={e => setSelected(e.target.value)}
        />
        Option 2
      </label>
    </div>
  );
}
```

### Uncontrolled Components

```jsx
import { useRef } from 'react';

function UncontrolledForm() {
  const inputRef = useRef(null);
  
  function handleSubmit(e) {
    e.preventDefault();
    console.log('Value:', inputRef.current.value);
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <input ref={inputRef} defaultValue="Initial value" />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Validation

```jsx
function LoginForm() {
  const [email, setEmail] = useState('');
  const [password, setPassword] = useState('');
  const [errors, setErrors] = useState({});
  
  function validate() {
    const newErrors = {};
    
    if (!email) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(email)) {
      newErrors.email = 'Email is invalid';
    }
    
    if (!password) {
      newErrors.password = 'Password is required';
    } else if (password.length < 6) {
      newErrors.password = 'Password must be at least 6 characters';
    }
    
    return newErrors;
  }
  
  function handleSubmit(e) {
    e.preventDefault();
    const newErrors = validate();
    
    if (Object.keys(newErrors).length === 0) {
      console.log('Form is valid');
    } else {
      setErrors(newErrors);
    }
  }
  
  return (
    <form onSubmit={handleSubmit}>
      <div>
        <input
          type="email"
          value={email}
          onChange={e => setEmail(e.target.value)}
        />
        {errors.email && <span>{errors.email}</span>}
      </div>
      <div>
        <input
          type="password"
          value={password}
          onChange={e => setPassword(e.target.value)}
        />
        {errors.password && <span>{errors.password}</span>}
      </div>
      <button type="submit">Submit</button>
    </form>
  );
}
```

---

## Lifecycle

### Function Component Lifecycle (avec hooks)

```jsx
function Component() {
  // Mount et Update
  useEffect(() => {
    console.log('Component mounted or updated');
  });
  
  // Mount seulement
  useEffect(() => {
    console.log('Component mounted');
  }, []);
  
  // Update seulement (quand prop change)
  useEffect(() => {
    console.log('Prop changed');
  }, [prop]);
  
  // Cleanup (unmount)
  useEffect(() => {
    console.log('Component mounted');
    
    return () => {
      console.log('Component will unmount');
    };
  }, []);
  
  // Équivalent componentDidUpdate
  useEffect(() => {
    console.log('Component updated');
  });
  
  return <div>Component</div>;
}
```

### Class Component Lifecycle (legacy)

```jsx
class Component extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  
  componentDidMount() {
    console.log('Component mounted');
  }
  
  componentDidUpdate(prevProps, prevState) {
    console.log('Component updated');
  }
  
  componentWillUnmount() {
    console.log('Component will unmount');
  }
  
  render() {
    return <div>{this.state.count}</div>;
  }
}
```

---

## Context API

### Création et utilisation

```jsx
import { createContext, useContext, useState } from 'react';

// 1. Créer le context
const ThemeContext = createContext();

// 2. Créer un Provider custom
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');
  
  const toggleTheme = () => {
    setTheme(prev => prev === 'light' ? 'dark' : 'light');
  };
  
  const value = {
    theme,
    toggleTheme
  };
  
  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// 3. Créer un hook custom pour utiliser le context
export function useTheme() {
  const context = useContext(ThemeContext);
  
  if (context === undefined) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  
  return context;
}

// 4. Utilisation
function App() {
  return (
    <ThemeProvider>
      <Toolbar />
    </ThemeProvider>
  );
}

function Toolbar() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <div className={theme}>
      <button onClick={toggleTheme}>Toggle theme</button>
    </div>
  );
}
```

### Context avec reducer

```jsx
const TodoContext = createContext();

function todoReducer(state, action) {
  switch (action.type) {
    case 'add':
      return [...state, action.payload];
    case 'remove':
      return state.filter(todo => todo.id !== action.payload);
    case 'toggle':
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    default:
      return state;
  }
}

export function TodoProvider({ children }) {
  const [todos, dispatch] = useReducer(todoReducer, []);
  
  return (
    <TodoContext.Provider value={{ todos, dispatch }}>
      {children}
    </TodoContext.Provider>
  );
}

export function useTodos() {
  return useContext(TodoContext);
}
```

### Multiple contexts

```jsx
function App() {
  return (
    <ThemeProvider>
      <UserProvider>
        <LanguageProvider>
          <Dashboard />
        </LanguageProvider>
      </UserProvider>
    </ThemeProvider>
  );
}

function Dashboard() {
  const { theme } = useTheme();
  const { user } = useUser();
  const { language } = useLanguage();
  
  return <div>{/* ... */}</div>;
}
```

---

## Refs

### DOM Refs

```jsx
import { useRef, useEffect } from 'react';

// Focus automatique
function AutoFocusInput() {
  const inputRef = useRef(null);
  
  useEffect(() => {
    inputRef.current.focus();
  }, []);
  
  return <input ref={inputRef} />;
}

// Mesurer un élément
function MeasureElement() {
  const divRef = useRef(null);
  
  useEffect(() => {
    const { width, height } = divRef.current.getBoundingClientRect();
    console.log('Size:', width, height);
  }, []);
  
  return <div ref={divRef}>Content</div>;
}

// Scroller vers un élément
function ScrollToElement() {
  const ref = useRef(null);
  
  const scrollToElement = () => {
    ref.current.scrollIntoView({ behavior: 'smooth' });
  };
  
  return (
    <>
      <button onClick={scrollToElement}>Scroll</button>
      <div ref={ref}>Target element</div>
    </>
  );
}
```

### Callback Refs

```jsx
function MeasureElement() {
  const [height, setHeight] = useState(0);
  
  const measureRef = useCallback(node => {
    if (node !== null) {
      setHeight(node.getBoundingClientRect().height);
    }
  }, []);
  
  return (
    <>
      <div ref={measureRef}>Content</div>
      <p>Height: {height}px</p>
    </>
  );
}
```

### Forward Refs

```jsx
import { forwardRef, useRef } from 'react';

// Composant avec forwardRef
const CustomInput = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});

// Utilisation
function Parent() {
  const inputRef = useRef(null);
  
  return (
    <>
      <CustomInput ref={inputRef} />
      <button onClick={() => inputRef.current.focus()}>Focus</button>
    </>
  );
}
```

---

## React Server Components

### Server Component

```jsx
// app/page.tsx (Server Component par défaut dans Next.js App Router)

// Accès direct à la base de données
import { db } from '@/lib/db';

export default async function HomePage() {
  const posts = await db.query('SELECT * FROM posts');
  
  return (
    <div>
      {posts.map(post => (
        <article key={post.id}>
          <h2>{post.title}</h2>
          <p>{post.content}</p>
        </article>
      ))}
    </div>
  );
}

// Pas de useState, useEffect, event handlers
// Pas de browser APIs
// Peut faire des appels async directement
```

### Client Component

```jsx
// components/Counter.tsx
'use client'; // Directive pour indiquer un Client Component

import { useState } from 'react';

export function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <button onClick={() => setCount(count + 1)}>
      Count: {count}
    </button>
  );
}
```

### Composition Server/Client

```jsx
// app/page.tsx (Server Component)
import { Counter } from '@/components/Counter';

export default async function HomePage() {
  const data = await fetchData();
  
  return (
    <div>
      <h1>{data.title}</h1>
      <Counter /> {/* Client Component imbriqué */}
    </div>
  );
}
```

### Server Actions

```jsx
// app/actions.ts
'use server';

import { db } from '@/lib/db';

export async function createTodo(formData: FormData) {
  const text = formData.get('text');
  
  await db.insert('todos', { text, completed: false });
  
  revalidatePath('/todos');
}

// app/todos/page.tsx
import { createTodo } from './actions';

export default function TodosPage() {
  return (
    <form action={createTodo}>
      <input name="text" />
      <button type="submit">Add</button>
    </form>
  );
}
```

---

## Suspense et Streaming

### Suspense basique

```jsx
import { Suspense } from 'react';

function App() {
  return (
    <Suspense fallback={<Loading />}>
      <UserProfile />
    </Suspense>
  );
}

// Composant qui suspend
function UserProfile() {
  const user = use(fetchUser()); // React 19+
  
  return <div>{user.name}</div>;
}
```

### Suspense avec lazy loading

```jsx
import { lazy, Suspense } from 'react';

const HeavyComponent = lazy(() => import('./HeavyComponent'));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <HeavyComponent />
    </Suspense>
  );
}
```

### Nested Suspense

```jsx
function App() {
  return (
    <Suspense fallback={<PageSkeleton />}>
      <Header />
      <Suspense fallback={<SidebarSkeleton />}>
        <Sidebar />
      </Suspense>
      <Suspense fallback={<ContentSkeleton />}>
        <Content />
      </Suspense>
    </Suspense>
  );
}
```

### Suspense avec Server Components

```jsx
// app/page.tsx
import { Suspense } from 'react';

export default function Page() {
  return (
    <>
      <Header />
      <Suspense fallback={<Skeleton />}>
        <SlowComponent />
      </Suspense>
      <Suspense fallback={<Skeleton />}>
        <AnotherSlowComponent />
      </Suspense>
    </>
  );
}

async function SlowComponent() {
  const data = await fetchSlowData();
  return <div>{data}</div>;
}
```

### Error Boundary avec Suspense

```jsx
import { ErrorBoundary } from 'react-error-boundary';

function App() {
  return (
    <ErrorBoundary fallback={<ErrorFallback />}>
      <Suspense fallback={<Loading />}>
        <UserProfile />
      </Suspense>
    </ErrorBoundary>
  );
}
```

---

## Performance

### React.memo

```jsx
import { memo } from 'react';

// Éviter les re-renders inutiles
const ExpensiveComponent = memo(function ExpensiveComponent({ data }) {
  console.log('Rendering ExpensiveComponent');
  return <div>{data}</div>;
});

// Avec comparaison personnalisée
const Component = memo(
  function Component({ user }) {
    return <div>{user.name}</div>;
  },
  (prevProps, nextProps) => {
    return prevProps.user.id === nextProps.user.id;
  }
);
```

### useMemo pour optimiser les calculs

```jsx
function FilteredList({ items, filter }) {
  const filteredItems = useMemo(() => {
    console.log('Filtering items...');
    return items.filter(item => item.includes(filter));
  }, [items, filter]);
  
  return (
    <ul>
      {filteredItems.map(item => (
        <li key={item.id}>{item.text}</li>
      ))}
    </ul>
  );
}
```

### useCallback pour mémoriser les fonctions

```jsx
function Parent() {
  const [count, setCount] = useState(0);
  
  // Sans useCallback, nouvelle fonction à chaque render
  const handleClick = useCallback(() => {
    console.log('Clicked');
  }, []);
  
  return (
    <>
      <Child onClick={handleClick} />
      <button onClick={() => setCount(count + 1)}>Count: {count}</button>
    </>
  );
}

const Child = memo(function Child({ onClick }) {
  console.log('Child rendered');
  return <button onClick={onClick}>Click</button>;
});
```

### Lazy loading

```jsx
import { lazy, Suspense } from 'react';

// Code splitting automatique
const Dashboard = lazy(() => import('./Dashboard'));
const Profile = lazy(() => import('./Profile'));

function App() {
  const [page, setPage] = useState('dashboard');
  
  return (
    <Suspense fallback={<Loading />}>
      {page === 'dashboard' ? <Dashboard /> : <Profile />}
    </Suspense>
  );
}
```

### useTransition pour les updates non-urgentes

```jsx
import { useTransition, useState } from 'react';

function SearchResults() {
  const [isPending, startTransition] = useTransition();
  const [query, setQuery] = useState('');
  const [results, setResults] = useState([]);
  
  function handleChange(e) {
    const value = e.target.value;
    setQuery(value); // Urgent
    
    startTransition(() => {
      // Non-urgent
      setResults(filterResults(value));
    });
  }
  
  return (
    <div>
      <input value={query} onChange={handleChange} />
      {isPending && <Spinner />}
      <Results data={results} />
    </div>
  );
}
```

### Virtualization (pour grandes listes)

```jsx
// Avec react-window
import { FixedSizeList } from 'react-window';

function VirtualList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      {items[index].text}
    </div>
  );
  
  return (
    <FixedSizeList
      height={600}
      itemCount={items.length}
      itemSize={35}
      width="100%"
    >
      {Row}
    </FixedSizeList>
  );
}
```

---

## Patterns Courants

### Custom Hooks

```jsx
// Hook pour fetch de données
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    let cancelled = false;
    
    async function fetchData() {
      try {
        setLoading(true);
        const response = await fetch(url);
        const json = await response.json();
        
        if (!cancelled) {
          setData(json);
          setError(null);
        }
      } catch (err) {
        if (!cancelled) {
          setError(err);
        }
      } finally {
        if (!cancelled) {
          setLoading(false);
        }
      }
    }
    
    fetchData();
    
    return () => {
      cancelled = true;
    };
  }, [url]);
  
  return { data, loading, error };
}

// Utilisation
function UserProfile({ userId }) {
  const { data, loading, error } = useFetch(`/api/users/${userId}`);
  
  if (loading) return <Spinner />;
  if (error) return <Error message={error.message} />;
  
  return <div>{data.name}</div>;
}
```

### Compound Components

```jsx
// Composants qui travaillent ensemble
function Tabs({ children }) {
  const [activeIndex, setActiveIndex] = useState(0);
  
  return (
    <div>
      {React.Children.map(children, (child, index) => {
        return React.cloneElement(child, {
          isActive: index === activeIndex,
          onActivate: () => setActiveIndex(index),
        });
      })}
    </div>
  );
}

function Tab({ isActive, onActivate, children }) {
  return (
    <button
      onClick={onActivate}
      style={{ fontWeight: isActive ? 'bold' : 'normal' }}
    >
      {children}
    </button>
  );
}

// Utilisation
<Tabs>
  <Tab>Tab 1</Tab>
  <Tab>Tab 2</Tab>
  <Tab>Tab 3</Tab>
</Tabs>
```

### Render Props

```jsx
// Composant avec render prop
function DataProvider({ render }) {
  const [data, setData] = useState(null);
  
  useEffect(() => {
    fetchData().then(setData);
  }, []);
  
  return render(data);
}

// Utilisation
<DataProvider
  render={data => (
    data ? <div>{data.name}</div> : <Loading />
  )}
/>
```

### Higher-Order Component (HOC)

```jsx
// HOC qui ajoute des props
function withAuth(Component) {
  return function AuthComponent(props) {
    const { user, loading } = useAuth();
    
    if (loading) return <Loading />;
    if (!user) return <Redirect to="/login" />;
    
    return <Component {...props} user={user} />;
  };
}

// Utilisation
const ProtectedDashboard = withAuth(Dashboard);
```

### Container/Presenter Pattern

```jsx
// Container (logique)
function UserContainer({ userId }) {
  const { data, loading, error } = useFetch(`/api/users/${userId}`);
  
  if (loading) return <Loading />;
  if (error) return <Error message={error.message} />;
  
  return <UserPresenter user={data} />;
}

// Presenter (UI)
function UserPresenter({ user }) {
  return (
    <div>
      <img src={user.avatar} alt={user.name} />
      <h2>{user.name}</h2>
      <p>{user.email}</p>
    </div>
  );
}
```

---

## Best Practices

### 1. Structure de fichiers

```
src/
├── components/
│   ├── common/
│   │   ├── Button.jsx
│   │   └── Input.jsx
│   ├── features/
│   │   ├── user/
│   │   │   ├── UserProfile.jsx
│   │   │   └── UserList.jsx
│   │   └── posts/
│   │       ├── PostCard.jsx
│   │       └── PostList.jsx
├── hooks/
│   ├── useFetch.js
│   └── useAuth.js
├── contexts/
│   ├── ThemeContext.jsx
│   └── AuthContext.jsx
├── utils/
│   └── helpers.js
├── services/
│   └── api.js
└── App.jsx
```

### 2. Naming conventions

```jsx
// Composants: PascalCase
function UserProfile() {}

// Fichiers de composants: PascalCase
// UserProfile.jsx

// Hooks: camelCase avec préfixe 'use'
function useAuth() {}

// Fonctions utilitaires: camelCase
function formatDate() {}

// Constantes: UPPER_SNAKE_CASE
const API_URL = 'https://api.example.com';

// Props: camelCase
<Button onClick={handleClick} isDisabled={false} />
```

### 3. Props destructuring

```jsx
// ✅ Bon
function User({ name, email, avatar }) {
  return <div>{name}</div>;
}

// ❌ Éviter
function User(props) {
  return <div>{props.name}</div>;
}
```

### 4. État local vs global

```jsx
// État local pour UI state
function Dropdown() {
  const [isOpen, setIsOpen] = useState(false); // Local
  return <div>{/* ... */}</div>;
}

// Context pour état partagé
const UserContext = createContext();

function App() {
  const [user, setUser] = useState(null); // Global via context
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {/* ... */}
    </UserContext.Provider>
  );
}
```

### 5. Éviter les inline functions dans le render

```jsx
// ❌ Mauvais (nouvelle fonction à chaque render)
function Parent() {
  return <Child onClick={() => console.log('clicked')} />;
}

// ✅ Bon
function Parent() {
  const handleClick = useCallback(() => {
    console.log('clicked');
  }, []);
  
  return <Child onClick={handleClick} />;
}
```

### 6. Diviser les gros composants

```jsx
// ❌ Mauvais
function Dashboard() {
  return (
    <div>
      {/* 500 lignes de JSX */}
    </div>
  );
}

// ✅ Bon
function Dashboard() {
  return (
    <div>
      <Header />
      <Sidebar />
      <Content />
      <Footer />
    </div>
  );
}
```

### 7. Utiliser PropTypes ou TypeScript

```tsx
// TypeScript (recommandé)
interface ButtonProps {
  onClick: () => void;
  children: React.ReactNode;
  disabled?: boolean;
}

function Button({ onClick, children, disabled = false }: ButtonProps) {
  return <button onClick={onClick} disabled={disabled}>{children}</button>;
}
```

### 8. Éviter les indices comme keys

```jsx
// ❌ Mauvais
{items.map((item, index) => (
  <div key={index}>{item}</div>
))}

// ✅ Bon
{items.map(item => (
  <div key={item.id}>{item}</div>
))}
```

### 9. Cleanup des effets

```jsx
// ✅ Toujours nettoyer les effets
useEffect(() => {
  const subscription = subscribe();
  
  return () => {
    subscription.unsubscribe();
  };
}, []);
```

### 10. Error Boundaries

```jsx
// Créer un Error Boundary pour capturer les erreurs
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  
  static getDerivedStateFromError(error) {
    return { hasError: true };
  }
  
  componentDidCatch(error, errorInfo) {
    console.error('Error caught:', error, errorInfo);
  }
  
  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }
    
    return this.props.children;
  }
}

// Utilisation
<ErrorBoundary>
  <MyApp />
</ErrorBoundary>
```

---

## Ressources

- [Documentation officielle React](https://react.dev)
- [React Beta Docs](https://beta.reactjs.org)
- [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app)
- [Awesome React](https://github.com/enaqx/awesome-react)


