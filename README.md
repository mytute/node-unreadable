
# Make unreadble(hard to manipulate) js code

There are some packages that can help to make unreadble code.    

1. <mark>uglify-js</mark> remove space and comments from repo.

2. <mark>javascript-obfuscator</mark> replace variable name with number code.

3. <mark>pkg</mark> create entire node project as standalong single file.


## uglify-js
 ...

## javascript-obfuscator

1.remove node default packages if its uesed.  

  eg: "path"

2.install package as dev

```bash
npm install --save-dev javascript-obfuscator
```

3.add script file to <package.json>     
here js file directory should add following result of webpack(we will add it here later)
>package.json  

```json
  "scripts": {
    ....
    "obuscator": "javascript-obfuscator <js-file-directory> --output built.js ",
    ....
  },
```


### here we are using webpack for convert node project to single file.

4.install webpack

```bash
npm install  webpack webpack-cli webpack-node-externals --save-dev
```

5.create 'webpack.config.js' file on app directory

> webpack.config.js

```javascript
const path = require('path');

module.exports ={
    target: 'node12.22', // put you node verion here
    entry: "./app.js",
    devtool: "source-map", // to remove eval function from bundle
    // module:{
    //     rules:[


    //     ]    
    // },
    output:{
        path: path.resolve(__dirname, "dist"), // define result directory
        filename:"bundle.js"
    },
    devServer: {
        contentBase: path.resolve(__dirname, 'dist'),
        watchContentBase: true,
        inline: true,
        hot: true
    },
    plugins:[],
    mode:process.env.NODE_ENV=== "production" ? "production" : "development"
}
```

## pkg


```bash
$ npm install -g pkg
```

2.add script file to <package.json>    

>package.json  

```json
  "bin": "./app.js"  // node entry file
  "scripts": {
    ....
    "build-pkg": "pkg  --out-path pkg-dist -t node12-linux  ./app.js ",
    ....
  },
  "pkg": { // we cant use this config when locally
  "scripts": "./app.js",
  "assets": "views/**/*",
  "targets": [
    "node14-linux-arm64"
  ],
  "outputPath": "pkg-dist"
},
```

we can't use pkg config on pakage.json file when pkg run locally.
because it need permission that not own by webpack. we can change permission but here we are define config on build script.

script config

```bash
  # target packages (use --out-path before this)
  $ pkg -t node12-linux,node14-linux,node14-win index.js

  # set output path as "package dist"
  $ pkg --out-path pkg-dist ./app.js
```
