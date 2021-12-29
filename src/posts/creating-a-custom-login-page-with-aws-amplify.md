---
title: Creating A Custom Login Page With AWS Amplify
date: 2021-08-27
tags: [aws, amplify, security]
splash: /images/padlock.jpg
splashalt: A picture of a padlock
summary: In this article, I'm going to show how to develop custom security around a page and how to protect it with a custom logon page. I'm going to be using React throughout and concentrating on the code and code flow rather than making the UI look nice (i.e., there's no CSS!).
---

## Introduction

In previous articles, I've shown how to [add authentication to a react app](https://cloudblogaas.com/using-aws-amplify-to-add-authentication-to-a-react-app) and how to [customise the look and feel](https://cloudblogaas.com/customizing-the-aws-amplify-components) of the provided security components, for example the login page.

In this article, I'm going to go one step further and show how to develop custom security around a page and how to protect it with a custom logon page.

I'm going to be using React throughout and concentrating on the code and code flow rather than making the UI look nice (i.e., there's no CSS!).

<!-- more -->

## The Application

The application is very simple. When the user tries to access the secure page, a login screen is displayed.

![Sample Login Page](https://cdn.hashnode.com/res/hashnode/image/upload/v1620682114573/BDZbwCh5Q.png)

When the user successfully authenticates, a welcome message is displayed along with the users login details.

![Sample Secure Page](https://cdn.hashnode.com/res/hashnode/image/upload/v1620682141553/DH27gGhTz.png)

The user can then log out and be returned to the login page.

The main application class is as below:

```javascript
import "./App.css";
import { Authenticator } from "aws-amplify-react";
import Amplify from "aws-amplify";
import config from "./aws-exports";
import SecurePage from "./components/SecurePage";

Amplify.configure(config);

function App() {
  return (
    <div className="App">
      <Authenticator hideDefault={true} amplifyConfig={config}>
        <SecurePage />
      </Authenticator>
    </div>
  );
}

export default App;
```

In this code, we can see that a `<SecurePage />` is being displayed. This is wrapped by the `<Authenticator />` which is part of Amplify. This properties on this class tell it to hide the default login component and use the configuration defined for the application.

There's not a lot to see here - the main thing being that we tell Amplify _not_ to show the default security pages.

## The Secure Page

The `<SecurePage />` makes use off the Amplify [Hub](https://docs.amplify.aws/lib/utilities/hub/q/platform/js) to determine whether a login page should be displayed, or whether the secure content should be displayed. The hub is a lightweight messaging system that can be configured to listen for events from specific sources. In this case, we're listening for events from the `auth` source. When an event from the `auth` source is received, it is stored within the component's state.

```javascript
import React, { useState } from "react";
import { Hub } from "aws-amplify";
import SignIn from "./SignIn";
import WhoAmI from "./WhoAmI";
import Logout from "./Logout";

function SecurePage() {
  const [authStatus, setAuthStatus] = useState("");

  const listener = (data) => {
    setAuthStatus(data.payload.event);
  };

  Hub.listen("auth", listener);

  return (
    <div>
      {authStatus === "signIn" ? (
        <div>
          Hello
          <WhoAmI />
          <Logout />
        </div>
      ) : (
        <SignIn />
      )}
    </div>
  );
}

export default SecurePage;
```

When the application runs, the `authStatus` is set to an empty string, so React displays the `<SignIn />` component (more about this in a minute). When the user successfully logs into the application, the Hub receives a `signIn` event which causes React to refresh the screen showing the secure contents.

Inside the secure contents is the greeting `Hello` along with two components, `<WhoAmI />` and `<Logout />`

## The WhoAmI Component

The `<WhoAmI />` component is a simple React component that queries Amplify to ask who the current logged on user is and then displays their email address on the screen.

```javascript
import React, { useState } from "react";
import { Auth } from "aws-amplify";

function WhoAmI() {
  const [username, setUsername] = useState("");

  Auth.currentUserInfo().then((user) => {
    if (user) {
      setUsername(user.attributes.email);
    }
  });
  return <div>{username}</div>;
}

export default WhoAmI;
```

This code calls the `Auth.currentUserInfo()` method which returns a promise from which we can get the user's email address.

## The Logout Component

The `<Logout />` component simply displays a button, which when clicked executes Amplify's `Auth.signOut()` method.

```javascript
import Auth from "@aws-amplify/auth";
import React from "react";

function Logout() {
  async function logOut() {
    Auth.signOut();
  }

  const handleFormSubmission = (e) => {
    e.preventDefault();

    logOut();
  };

  return (
    <div>
      <button type="submit" onClick={handleFormSubmission}>
        Logout
      </button>
    </div>
  );
}

export default Logout;
```

## The SignIn Component

Finally, we have the `<SignIn />` component which is responsible for displaying the login form and then performing the Sign In action using the `Auth` class.

```javascript
import React, { useState } from "react";
import { Auth } from "aws-amplify";

function SignIn() {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [errorMessage, setErrorMessage] = useState("");

  async function signIn() {
    try {
      Auth.signIn({ username, password })
        .then((user) => {
          // Handle case where New Password is Required.
        })
        .catch((e) => {
          setErrorMessage(e.message);
        });
    } catch (e) {}
  }

  const handleFormSubmission = (e) => {
    e.preventDefault();

    signIn();
  };
  return (
    <div>
      <form>
        <label>Username</label>
        <input
          value={username}
          type="text"
          name="username"
          onChange={(e) => setUsername(e.target.value)}
        />
        <label>Password</label>
        <input
          type="password"
          name="username"
          onChange={(e) => setPassword(e.target.value)}
        />
      </form>
      <button type="submit" onClick={handleFormSubmission}>
        Login
      </button>

      <div>{errorMessage}</div>
    </div>
  );
}

export default SignIn;
```

In this code, we declare component state for the username, password and error message. A simple form is displayed with an input for username and password which are linked to their corresponding state.

When the login button is pressed, the `signIn` function is called which in turn calls the `Auth.signIn()` function. If this fails, the `errorMessage` is set and displayed on the form.

This code works perfectly when a user has already logged onto the security backend. For a new user however, that has been created on the back end and requires a new password to be set, additional code is required within the promise returned from the `Auth.signIn()` method.

In this case, the `Auth.signIn()` method will return a challenge of `NEW_PASSWORD_REQUIRED`. In this instance the `Auth.completeNewPassword()` method needs to be called followed by `Auth.signIn()`, for example:

```javascript
Auth.signIn({ username, password })
  .then((user) => {
    if (user.challengeName === "NEW_PASSWORD_REQUIRED") {
      Auth.completeNewPassword(user, newPassword, {})
        .then((user) => {
          Auth.signIn({ username: username, password: newPassword })
            .then((user) => {
              // Signed in.
            })
            .catch((e) => {
              // Failed to sign in.
            });
...
```

## Conclusion

In this article, I've shown how to log in and out from AWS Amplify, how to secure a page so that login is required and how to use the Hub to receive event notifications from the security subsystem.

I hope you've enjoyed this article. Happy Amplifying !

## Credits

Photo by <a href="https://unsplash.com/@mr_williams_photography?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Micah Williams</a> on <a href="https://unsplash.com/s/photos/login?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a>
