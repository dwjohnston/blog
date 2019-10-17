# Setting up jest and typescript

The basically follow the instructions from [the Jest documentation here](https://jestjs.io/docs/en/next/getting-started.html#using-typescript)

**Install dependencies**

```
npm i -D typescript jest babel-jest @babel/core @babel/preset-env @babel/preset-typescript @types/jest

```

**Init jest**

```
npx jest --init
```

**Add babel config**


```
//babel.config.js
module.exports = {
  presets: [
    ['@babel/preset-env', {targets: {node: 'current'}}],
    '@babel/preset-typescript',
  ],
};
```

**nb.** Note that this use of babel is seperate from any other use of babel that you might be using. 

ie. My understanding is that the `babel-jest` dependency is being used by jest directly to transpile your code for running in Jest. It's not being used to transpile your actual code builds. 

