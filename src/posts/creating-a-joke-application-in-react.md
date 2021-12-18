---
title: Creating a Joke Application in React
date: 2021-03-10
tags: [react, javascript]
---

![](/images/joke.jpg)

## Introduction

I've recently started learning React. I've been a backend developer for a long time, but have started to love doing client side work. As they say, the best way to learn is to practise. So here is a simple joke application that I've written in React. The source code for the application can be found on GitHub at: https://github.com/doobrie/react-joke

<!-- more -->

![joke.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615390598743/NbUE58zjP.png)

## Creating the project

Whilst practising, I've quite often created projects from scratch, but this is quite tedious as the same steps need to be taken for each project. I needed to create the project structure, configure Babel, write some control scripts etc. Instead of doing that this time, I've used the `create-react-app` tool to scaffold the basics of an application.

```bash
npx create-react-app
```

This sets up everything that you need to get started with a React app.

## Coding

As this is a simple project, I've created one react component, `function app()`. I've created this as a functional component rather than a class as there's very little to it, and no particular need to write a class.

First of all, I wrote the JSX for the app.

```html
  return (
    <div className="app">
      <header className="app-header">
        <h1>Jokes</h1>
      </header>
      <section className="app-content">
        <p>{joke}</p>
      </section>
      <section>
        <button
          onClick={refresh}
          disabled={joke.length === 0}
        >
          Ha ha.  Tell me more
        </button>
      </section>
    </div >
  );
```

There's very little to this, but the important things to notice are:

- The joke that is to be displayed is rendered via `{joke}`. This variable will be stored within the state of the application.
- Storing the joke within state, allows me to disable the refresh button if there is no joke. This will be when the application is fetching a joke from a remote api.
- A button invokes a `refresh` mothod when clicked which will fetch a new joke and store it in the application's state.

## Application State

As mentioned above, I'm storing the joke in the application's state. This is achieved using the React hook:

```javascript
const [joke, setJoke] = useState("");
```

## Creating the logic

To get the joke, I'm making a call to the `https://icanhazdadjoke.com` endpoint. This returns JSON data containing a random joke.

To call this, I use Axios to call the endpoint and then store the joke in the application's state.

Axios needed to be added to the project before using it. This was achieved using `yarn`

```bash
yarn add axios
```

The code to get the joke looks like this:

```javascript
const refresh = () => {
  setJoke("");
  axios
    .get("https://icanhazdadjoke.com/", {
      headers: {
        Accept: "application/json",
      },
    })
    .then((res) => {
      setJoke(res.data.joke);
    });
};
```

Finally, I want to make sure that the state (and therefore the joke to display) is updated when the application first runs. I do this with the `useEffect` hook.

```javascript
useEffect(() => {
  refresh();
}, []);
```

The final stage was to add some styling to the application. This is stored in the App.css file which you can see in the project's GitHub repo.

To run the application, I'm using `yarn start`

## Conclusion

This is a very straightforward app, but I've found it useful when practising React, and I hope you find it useful too. The full code for the application is available on GitHub at: https://github.com/doobrie/react-joke

## Credits

Photo by <a href="https://unsplash.com/@simplicity?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Marija Zaric</a> on <a href="/s/photos/joke?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
