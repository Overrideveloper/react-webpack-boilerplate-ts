# Building a React TypeScript Boilerplate

We'll be using the [react-webpack-boilerplate](https://github.com/Overrideveloper/react-webpack-boilerplate) and converting it from being JavaScript-based to TypeScript-based.

* Install some more devDependencies

```powershell
npm install --save-dev typescript awesome-typescript-loader
```
**typescript** enables us use TypeScript in our codebase.<br>
**awesome-typescript-loader** enables Webpack understand our TypeScript code and compile it to JavaScript.

* Install types for React and ReactDOM

```javascript
npm install --save @types/react @types/react-dom
```

* Configure TypeScript by adding a tsconfig.json with the following configuration.

```JSON
{
    "compilerOptions": {
        "outDir": "./build/",
        "sourceMap": true,
        "strictNullChecks": true,
        "module": "es6",
        "jsx": "react",
        "target": "es5",
        "allowJs": true,
        "moduleResolution": "node"
    },
    "include": ["./src/"]
}
```

**compilerOptions** instructs the TypeScript compiler on how to compile Typescript files.<br>
    **outDir** tells the compiler where the compiled files should be stored.<br>
    **sourceMap** tells the compiler whether to create sourcemaps or not.<br>
    **strictNullChecks** tells the compiler whether or not to check for nulls strictly.<br>
    **module** tells the compiler what the target should be for module code generation.<br>
    **jsx** tells the compiler what kind of project it should support JSX in .tsx files in.<br>
    **target** specifies what ES version should be targeted during compilation.<br>
    **allowJs** specifies whether the compiler should allow JavaScript files to be compiled.<br>
    **moduleResolution** specifies how modules are resolved.<br>
    
    Setting it to **node** means that in addition to checking the included directories for the module to be resolved, it will also check the node_modules.
    Setting it to **classic** only checks the included directories.
**include** specifies what directories the compiler should compile [from].
        
* Modify the webpack.config.js to compile TypeScript and TSX files.
* First we'll change the entry file to index.tsx.

```JSON
entry: './src/index.tsx',
``` 

* Then we'll modify the resolve.extensions config to include ts and tsx files.

```JavaScript
resolve: {
    extensions: ['.ts', '.tsx', '.js', '.jsx']
},
```

* Next we'll replace the module rule for parsing and loading .js and .jsx files to one that includes ts and .tsx files.

```JavaScript
{
    test: /\.(js|jsx|ts|tsx)$/,
    exclude: /(node_modules)/,
    use: { loader: 'awesome-typescript-loader',}
},
```

* Finally we'll add an externals configuration.

```JavaScript
    externals: {
        "react": "React",
        "react-dom": "ReactDOM",
    }
```
The externals configuration prevents certain dependencies from being added to the bundle during the build process. In this case we are removing react and react-dom from the output bundles. This should keep the bundle size small.

* After modifying the webpack.config.js file, we can then run `npm run serve` and view our boilerplate in the browser.


