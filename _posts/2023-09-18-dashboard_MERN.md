---
layout: single
title: "[MERN] dashboard clone coding practice"
categories: codingPractice
tag: [codingPractice, JavaScript, MERN]
toc: true
published: true
---

# 01. MERN Dashboard Clone coding

I started this project to practice building the MERN stack app before attending the Goormthon team challenge. I followed Edroh's tutorial on Youtube.

In this post, I will write the details based on what I have learned from the tutorial and what obstacles I have faced.

## Problem type - authentication failure

# 🧩 Problem

There was a bad auth error, which did not connect MongoDB to the backend.

![Alt text](</images/2023-09-18-dashboard_MERN/Screenshot 2023-09-18 at 11.07.22 PM.png>)

![Alt text](</images/2023-09-18-dashboard_MERN/Screenshot 2023-09-18 at 11.09.17 PM.png>)

# 🎯 Strategy

### Google Search

From the stack overflow, I found this post:
https://stackoverflow.com/questions/55695565/error-message-mongoerror-bad-auth-authentication-failed-through-uri-string?page=1&tab=scoredesc#tab-top

In my case, I didn't set up the password because I used an existing database.

# 💻 Solution

I set up the password from the following area in the picture and put it in the .env file.

![Alt text](</images/2023-09-18-dashboard_MERN/Screenshot 2023-09-18 at 11.14.31 PM.png>)

# 🧩 Problem

This jsconfig.json file allows absolute imports from the source directory.

```json
{
  "compilerOptions": {
    "baseUrl": "src"
  },
  "include": ["src"]
}
```

# 🧩 Problem

## Problem type - adding the dark mode w mui

If we want to add the dark mode, we must follow the steps below.

To start, we are going to make the theme.js file. In that file, create the function that reverses the color between light and dark modes.

```js
// function that reverses the color palette => for the dark mode
function reverseColor(tokensDark) {
  const reversedTokens = {};
  Object.entries(tokensDark).forEach(([key, val]) => {
    const keys = Object.keys(val);
    const values = Object.values(val);
    const length = keys.length;
    const reversedObj = {};

    for (let i = 0; i < length; i++) {
      // put original key and value in reverse e.g) keys[1] = values[length - 1 - 1]
      reversedObj[keys[i]] = values[length - i - 1];
    }
    reversedTokens[key] = reversedObj;
  });
  return reversedTokens;
}

export const tokensLight = reverseColor(tokensDark);

// mui theme settings
export const themeSettings = (mode) => {
  return {
    palette: {
      mode: mode,
      ...(mode === "dark"
        ? {
            // palette values for dark mode
            primary: {
              ...tokensDark.primary,
              main: tokensDark.primary[400],
              light: tokensDark.primary[400],
            },
            secondary: {
              ...tokensDark.secondary,
              main: tokensDark.secondary[300],
            },
            neutral: {
              ...tokensDark.grey,
              main: tokensDark.grey[500],
            },
            background: {
              default: tokensDark.primary[600],
              alt: tokensDark.primary[500],
            },
          }
        : {
            // palette values for light mode
            primary: {
              ...tokensLight.primary,
              main: tokensDark.grey[50],
              light: tokensDark.grey[100],
            },
            secondary: {
              ...tokensLight.secondary,
              main: tokensDark.secondary[600],
              light: tokensDark.secondary[700],
            },
            neutral: {
              ...tokensLight.grey,
              main: tokensDark.grey[500],
            },
            background: {
              default: tokensDark.grey[0],
              alt: tokensDark.grey[50],
            },
          }),
    },
    typography: {}
```

And created the global state by using redux toolkit.

```js
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
  mode: "dark",
};

export const globalSlice = createSlice({
  name: "global",
  initialState,
  reducers: {
    // created the function setMode that allow us to change the mode from dark to light(functions that change the global state)
    setMode: (state) => {
      state.mode = state.mode === "light" ? "dark" : "light";
    },
  },
});

export const { setMode } = globalSlice.actions;

export default globalSlice.reducer;
```

The following code snippets are the redux toolkit boilerplate.

```js
import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import { configureStore } from "@reduxjs/toolkit";
// the file tree starts from the src, hence we can use just "state"
import globalReducer from "state";
import { Provider } from "react-redux";

const store = configureStore({
  reducer: {
    global: globalReducer,
  },
});

const root = ReactDOM.createRoot(document.getElementById("root"));
root.render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
    </Provider>
  </React.StrictMode>
);
```

We will also set the app.js to add dark and light modes.

```js
import { CssBaseline, ThemeProvider } from "@mui/material";
import { createTheme } from "@mui/material/styles";
import { useMemo } from "react";
import { useSelector } from "react-redux";
import { themeSettings } from "theme";

function App() {
  // grabbing the state from here
  const mode = useSelector((state) => state.global.mode);
  // passing the state into the createTheme function that we created
  const theme = useMemo(() => createTheme(themeSettings(mode)), [mode]);

  return (
    <div className="app">
      <ThemeProvider theme={theme}>
        <CssBaseline />
      </ThemeProvider>
    </div>
  );
}

export default App;
```
