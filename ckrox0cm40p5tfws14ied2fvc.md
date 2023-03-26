---
title: "Redux by example: Combined reducer with action creators"
datePublished: Thu Oct 05 2017 22:01:03 GMT+0000 (Coordinated Universal Time)
cuid: ckrox0cm40p5tfws14ied2fvc
slug: redux-by-example-combined-reducer-with-action-creators-34be581ddb0
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409804562/qWbhl0lcT.jpeg
tags: javascript, web-development, reactjs, redux

---


After reading and following along with the official Redux Read Me, I had many code samples saved to Codepen. This is my attempt to condense the most important concepts I learned, along with code examples into a short post.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409802298/fWht_p0Uc.jpeg)

Note: Some of the “next gen” code — such as [Default, Rest, & Spread](https://github.com/lukehoban/es6features#default--rest--spread) — used here may not be supported by your browser, yet. I recommend transpiling through [Babel and the env preset](https://babeljs.io/docs/plugins/preset-env/), which enables transforms for ES2015+.

The easiest way to get started with Redux is to include it in your page via `&lt;script&gt;` tag.

```
<script src="[https://cdnjs.cloudflare.com/ajax/libs/redux/3.0.5/redux.min.js](https://cdnjs.cloudflare.com/ajax/libs/redux/3.0.5/redux.min.js)"></script>
```


With Redux available, the first step is to set up some constants for action types.
> Actions must have a `type` property that indicates the type of action being performed. Types should typically be defined as string constants.

```
const ADD_TODO = 'ADD_TODO';
const COMPLETE_TODO = 'COMPLETE_TODO';
const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER';
const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE',
};
```


Now, define the [actions](http://redux.js.org/docs/basics/Actions.html) themselves.
> Actions are payloads of information that send data from your application to your store. They are the only* *source of information for the store.

```
const addTodo = (text) => {
  return { type: ADD_TODO, text);
};

const completeTodo = (index) => {
  return { type: COMPLETE_TODO, index };
}

const setVisibilityFilter = (index) => {
  return { type: SET_VISIBILITY_FILTER, filter };
};
```


And now a couple [reducers](http://redux.js.org/docs/basics/Reducers.html).
> Reducers specify how the application’s state changes in response to actions.

```
const todos = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [
        ...state,
        {
          text: action.text,
          completed: false
        }
      ];
    case COMPLETE_TODO:
      return [
        ...state.slice(0, action.index),
        Object.assign({}, state[action.index], {
          completed: true
        }),
        ...state.slice(action.index + 1)
      ];
    default:
      return state;
  }
};

const visibilityFilter = (state = VisibilityFilters.SHOW_ALL, action) => {
  switch (action.type) {
    case SET_VISIBILITY_FILTER:
      return action.filter;
    default:
      return state;
  }
};
```


`todos` and `visibilityFilter` are both reducers.

You will want to combine the reducers your app uses. Either of these next two examples will do the job. They are functionally equivalent.

```
/* MANUALLY */

const todoApp = (state = {}, action) => {
  return {
    visibilityFilter: visibilityFilter(state.visibilityFilter, action),
    todos: todos(state.todos, action)
  };
};


/* REDUX HELPER */

const todoApp = Redux.combineReducers({
  visibilityFilter,
  todos
});
```


Actions and reducers are useless without a [store](http://redux.js.org/docs/basics/Store.html).
> The store holds application state and provides methods to access to state, update state, and registers/unregisters listeners.

```
const store = Redux.createStore(todoApp);
```


We can listen and react to the store via the `[.subscribe()` method](http://redux.js.org/docs/api/Store.html#subscribe).

```
store.subscribe(() => {
  console.log(store.getState());
});
```


In this case, any time the state is mutated, we’ll log it to the console.

That’s it! The state can be mutated with actions via the `[.dispatch()` method](http://redux.js.org/docs/api/Store.html#dispatch). Here are some examples.

### Add a todo

```
store.dispatch(addTodo('Learn about actions'));

{
  "visibilityFilter": "SHOW_ALL",
  "todos": [
    {
      "text": "Learn about actions",
      "completed": false
    }
  ]
}
```


### Add another

```
store.dispatch(addTodo('Learn about reducers'));

{
  "visibilityFilter": "SHOW_ALL",
  "todos": [
    {
      "text": "Learn about actions",
      "completed": false
    },
    {
      "text": "Learn about reducers",
      "completed": false
    }
  ]
}
```


### One more

```
store.dispatch(addTodo('Learn about store'));

{
  "visibilityFilter": "SHOW_ALL",
  "todos": [
    {
      "text": "Learn about actions",
      "completed": false
    },
    {
      "text": "Learn about reducers",
      "completed": false
    },
    {
      "text": "Learn about store",
      "completed": false
    }
  ]
}
```


### Complete the first todo

```
store.dispatch(completeTodo(0));

{
  "visibilityFilter": "SHOW_ALL",
  "todos": [
    {
      "text": "Learn about actions",
      "completed": true
    },
    {
      "text": "Learn about reducers",
      "completed": false
    },
    {
      "text": "Learn about store",
      "completed": false
    }
  ]
}
```


### Complete the second todo

```
store.dispatch(completeTodo(1));

{
  "visibilityFilter": "SHOW_ALL",
  "todos": [
    {
      "text": "Learn about actions",
      "completed": true
    },
    {
      "text": "Learn about reducers",
      "completed": true
    },
    {
      "text": "Learn about store",
      "completed": false
    }
  ]
}
```


### Show completed todos

```
store.dispatch(setVisibilityFilter(VisibilityFilters.SHOW_COMPLETED));

{
  "visibilityFilter": "SHOW_COMPLETED",
  "todos": [
    {
      "text": "Learn about actions",
      "completed": true
    },
    {
      "text": "Learn about reducers",
      "completed": true
    },
    {
      "text": "Learn about store",
      "completed": false
    }
  ]
}
```


The completed code can be found in this pen.

%[https://codepen.io/travishorn/pen/mVpJNv]
