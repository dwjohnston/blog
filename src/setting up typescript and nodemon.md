# Setting up typescript and nodemon

When developing you often want a live reload happening. 

When using create-react-app you get this out of the box. 

But for doing a server side project or runtime agnostic project (such as a utility library), using nodemon works well for doing the live reload. 

**Install dependencies**

```
npm i -D nodemon ts-node
```

**Add nodemon.json**

```
//nodemon.json
{
    "watch": ["src"],
    "ext": "ts",
    "ignore": ["*.test.ts"],
    "delay": "1",
    "execMap": {
      "ts": "ts-node"
    }
}
```

**Modify package.json**

```
"start": "nodemon src/index.ts" //Or whatever
```

