# Setting up a CRA-based React component library, within a monorepo, with TypeScript. 

These are the steps I take to set up a component library within a mono repo, where the user libraries are set up with CRA, and we want to use CRA for the component library too.

**1. Use create-component-lib to instantiate**

Use this library here to instantiate the project: 

https://www.npmjs.com/package/create-component-lib

```
npx create-component-lib my-lib
```


**2. Add typescript**

```
npm i -D typescript @types/react @types/react-dom
```

**3. Change one of the files to typescript**

```
mv src/lib/TextInput.js src/lib/TextInput.tsx
```

Start: 

```
npm start 
```

CRA will detect this, and automatically add the tsconfig.json. 

Fix the errors in TextInput.tsx, (easiest way is to just delete all the parameters and display nothing). 

The live reload server is now working. 


**4. Add a tsconfig.build.json**

The tsconfig for `npm start` isn't correct for `npm run build`. 

To fix we do this: 

```
cp tsconfig.json tsconfig.build.json
```

You need to change the `tsconfig.build.json` to have the following settings: 

```
    "jsx": "react",
    "isolatedModules": false,
    "noEmit": false, 
    "outDir": "./dist", 
    "allowJs" : false, 
    "declaration": true
```

**5. Edit package.json scripts**

```
    "build": "tsc -p tsconfig.build.json",
    "build-examples": "react-scripts build",
    "build:watch": "tsc -p tsconfig.build.json --watch",
    "compile": "./node_modules/typescript/bin/tsc -p ./tsconfig.build.json",
    "eject": "react-scripts eject",
    "start": "PORT=2000 react-scripts start",
    "test": "react-scripts test --env=jsdom"
```



**6. Build!**

```
npm run build 
```

Check that .js, .css, and .d.ts files are being created in `/dist` folder

**7. (optional, but recommended) Add `/dist` to your `.gitignore`**

**8.Switch to your user library (the project that is using this component library**

```
cd ../my-user-library
```

**9.add the library as a dependency** 

In package.json 

```
    "simple-component-library": "file:../my-lib"
```

**10. Add react-app-rewired and customize-cra**

The reason for these steps is because other wise you will run issues where your project has two instances of react, and it doesn' towkr 

```
npm i -D react-app-rewired customize-cra
```

Change your package.json to use react-app-rewired

```
    "build": "react-app-rewired build",
    "eject": "react-app-rewired eject",
    "start": " react-app-rewired start",
    "test": "react-app-rewired test",
```

**11. create config-overides.js**

```
const {
    override,
    addWebpackAlias,
  } = require("customize-cra");

const path = require('path'); 

module.exports = override( 
    addWebpackAlias({
        react: path.resolve('./node_modules/react'), 
    }), 
)
```

**Important!** - From now on, any time you add a peer dependency to your component library (eg. material-ui, redux), you need to add them as an alias in the config overides. 




**12. You're ready to go!**

As a general work flow, my technique is to have two (or three) terminal windows: 

On the component liberary: 

`npm run build:watch` - to be constantly recompiling

`npm start` - If you want to be seeing the preview of the components 

And on the main project

`npm start` - To be building your main project. 

