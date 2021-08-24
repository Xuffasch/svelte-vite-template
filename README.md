<h1 
  style="padding: 8px;
  text-align: center;
  color: orangered;">
  svelte-vite-template
</h1>

Custom build Svelte with Vite project starting vite 'svelte' template.

As a reference to myself and others who might be interested, I write as much explanation to the settings currently applied in this repo.

The template will be upgraded with additions that could be generally useful to bootstrap any svelte project.

Progressively augmented with development tools.
Already integrated :

- Prettier
- ESLint

# Prettier

<h3 
  style="padding: 4px; 
  background: gray; 
  color: white"> 
  VSCode Settings files
</h3>

VSCode has User and Workspace (project) settings. The settings are save in settings.json for the user or workspace by project.

3 ways to setup these settings in the settings file :

- settings helper screen to search for settings which will write to the corresponding settings file.
  In VSCode, type `Cmd + Shift + P`, then choose "Preferences: Open User Settings"

- Directly access the 'user' settings.json file

  In VSCode, type `Cmd + Shift + P`, then choose "Preferences: Open Settings (JSON)"

- Directly access the 'workspace' settings.json file

  In VSCode, type `Cmd + Shift + P`, then choose "Preferences: Open Workspace Settings (JSON)"

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered"> 
  Install the development dependencies
</h3>

For VSCode usage only, install the VSCode extension :

- "Prettier - Code Formatter" by Prettier
- "Svelte for VSCode" by Svelte

For Terminal usage, install :

- prettier
- prettier-plugin-svelte

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered"> 
  To setup Prettier for javascript
</h3>

1.  Edit the 'user' or 'workspace' settings.json file

    In the 'user' or 'workspace' settings file, add these settings to setup prettier:

    ```json
    {
      ...
      "editor.formatOnSave": true,
      "editor.formatOnPaste": true,
      "[javascript]": {
          "editor.defaultFormatter": "esbenp.prettier-vscode",
      },
      "[javascriptreact]": {
          "editor.defaultFormatter": "esbenp.prettier-vscode",
      },
      ...
    }
    ```

    - "editor.formatOnSave" :
      if 'true', the file is edited to apply the format options when the file is saved.
      If 'false', the file is not automatically edited with the format options

    - "editor.formatOnPaste":
      if 'true', the copied code is automatically applied the format options set for the file
      if 'false', the copied code is not applyed the format options when pasted.

    - "[javascript]": VSCode language override to define specific VSCode settings for codes recognized as javascript. In our case, apply prettier-vscode extension on js code found between "script" tags in .svelte files.

    - "[javascriptreact]": VSCode language override to define specific VSCode settings for codes recognized as javascript. In our case, apply prettier-vscode extension on .js files as currently js files is associated to the type "javascriptreact".

      _NOTE_ I currently have to make that a files.association in the VSCode user.settings to link .js files to the type javascriptreact :

      ```js
        // user (or workspace) settings.json

      {
        ...
        "files.associations": {
          "*.js": "javascriptreact"
        },
        ...
      }
      ```

      in order to have automatic application of prettier config file format options apply on my .js files. Removing this link _breaks_ prettier from running on js file.

      I don't know if it is a bug or something in my VSCode settings.

2.  Add format options in a .prettierrc.json

    In the root folder of a Vite + Svelte project, add a .prettierrc.json file to define prettier format settings.

    example of .prettierrc.json file content

    ```json
    {
      "tabWidth": 2,
      "semi": true,
      "singleQuote": false,
      "trailingComma": "es5"
    }
    ```

    Changes to the format options are immediately used without restart of VSCode.

3.  Define different format options for specific files

    By adding the 'overrides' setting in the .prettierrc.json, the general format options can be changed depending on the "files" parameter for which a file glob pattern is provided.

    **IMPORTANT** Do not add './' to the beginning of the "files" parameter value. "files" accepts a file glob pattern which is not a file path.

    **Not Providing a correct file glob format breaks the automatic on save formatting of svelte files**

    ```js
    {
      "tabWidth": 2,
      "semi": true,
      "singleQuote": true,
      "trailingComma": "es5",
      "overrides": [{
          "files": "src/lib/tools/**/*.js",
          "options": {
              "tabWidth": 2,
              "semi": true,
              "singleQuote": true
          }
      }]
    }
    ```

<h3 
style="padding: 4px; 
background: white; 
color: orangered"> 
To setup Prettier for Svelte - for VSCode
</h3>

1.  The Svelte for VSCode should be installed

    Check that the extension is declared in the extensions.json

    ```js
    // .vscode/extensions.json

    {
      "recommendations": ["svelte.svelte-vscode"]
    }
    ```

2.  Edit the VSCode 'use' or 'workspace' settings.json file

    According to the Svelte for VSCode extension manual, we need to manually set the VSCode 'svelte' language override and set 'svelte-vscode' as the extension to call on svelte file.
    As the extension name changed, check this name in the manual if the extension is not working.

    Current setting code example :

    ```js
    // settings.json

    {
      ...
      "[svelte]": {
        "editor.defaultFormatter": "svelte.svelte-vscode"
      }
      ...
    }
    ```

    **Note :** Most of the time, after a change of the Prettier config file, VSCode needs to be restarted so that settings are applied to .svelte file.

3.  Svelte js formatting options

    While editing the code in VSCode, the same .prettierrc.json file to format all found js code and formatting is automatic on file save.

    **IMPORTANT** Restart VSCode after each change of prettierrc.json to apply the new format options

    New formatting options are immediately applicable by VSCode on .js files declared as 'javascriptreact' but not on .svelte files which. A VSCode restart update the current formatting options for svelte files.

    **IMPORTANT**
    _Automatic formatting in VSCode **WILL BREAK**_

    - for _.svelte_ files, if there is an "overrides" parameter **BE CAREFUL** that the "files" parameter does not receive a string which starts with "./", as if it is a file path and not a file glob pattern.

      Example of breaking .prettierrc.json : "files" has received a file path, while the expected file glob pattern is only `"src/lib/tools/**/*.js"`.

      ```js
      {
          "tabWidth": 2,
          "semi": true,
          "singleQuote": true,
          "trailingComma": "es5",
          "overrides": [{
              "files": "./src/lib/tools/**/*.js",
              "options": {
                  "tabWidth": 2,
                  "semi": true,
                  "singleQuote": true,
                  "trailingComma": "es5"
              }
          }]
      }
      ```

    - for _.js_ files, if "plugins" for prettier are defined in the prettier config file

      Example of breaking .prettierrc.json

      ```js
      {
        "tabWidth": 2,
        "semi": true,
        "singleQuote": true,
        "trailingComma": "es5",
        "plugins": ["prettier-plugin-svelte"],
      }
      ```

<h3 
style="padding: 4px; 
background: white; 
color: orangered"> 
To setup Prettier for Javascript and Svelte - for the Terminal
</h3>

Change the script command in package.json

the '--plugin' parameter tells prettier where to find the svelte plugin as we cannot add it in the .prettierrc.json without breaking automatic formatting of js files.

```js
// package.json
{
  ...
  "scripts": {
    ...
    "format": "prettier --plugin=prettier-plugin-svelte --write ./src/**/*.{js,svelte} ./src/*.{js,svelte}",,
    ...
  }
  ...
}
```

# ESLint

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered"> 
  Install the development dependencies
</h3>

- eslint
- eslint-plugin-svelte3

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered"> 
  Create a .eslintrc.json in the root folder of the project
</h3>

```js
{
  "root": true,
  "env": {
    "browser": true,
    "es2021": true
  },
  "parserOptions": {
    "ecmaVersion": 12,
    "sourceType": "module"
  },
  "plugins": ["svelte3"],
  "overrides": [
    {
      "files": ["**/*.svelte"],
      "processor": "svelte3/svelte3",
      "quotes": ["warn", "double"]
    }
  ],
  "extends": "eslint:recommended",
  "rules": {
    "quotes": ["warn", "double"]
  }
}
```

- 'root' : base .eslintrc.json files

  set "root" to true to tell ESLint not look further up looking for settings files to merge with.

  ```js
  // .eslintrc.json

  {
    "root": true,
    ...
  }
  ```

- 'env' : [environment][eslint-environment] where the linted code will be running

  To add an environment, add a parameter with the name of the environment and set its value to 'true'.

  Adding an environment will add predefined global variables ESLint needs for all rules when it checks the code.

  In the example, the code will run in a browser environment and use ESMAScript 2021 official syntax.

  ```js
  // .eslintrc.json

  }
    ...
    "env": {
      "browser": true,
      "es2021": true
    }
    ...
  }
  ```

- 'parser': override default js 'estree' parser

  Set a different parser when experimental js features or babel generated js is used.

  Not used in the example but should be placed above the parseOptions parameter.

  example:

  ```js
  "parser": "@babel/eslint-parser",
  ```

- 'parserOptions': default to 'estree' parser options format

  In the example, as the env "es2021" is true, this corresponds in ESLint to an ecmaVersion of '12' for the version of JS to check against.

  "sourceType" means that code comes from imported modules

  ```js
  // .eslintrc.json

  }
    ...
    "parserOptions": {
      "ecmaVersion": 12,
      "sourceType": "module"
    }
    ...
  }
  ```

- 'plugins': modules that exports its own processor (js code extractor), set of linting rules or environment (global variables).

  Even if the plugin name is eslint-plugin-svelte3, to declare a plugin to use, add its name by omitting its prefix "eslint-plugin-\*"

  ```js
    // .eslintrc.json

    {
      ...
      "plugins": ["svelte3"],
      ...
    }
  ```

  To use the environment provided by a plugin, add in the 'env' parameter, the name

  ```js
    // .eslintrc.json

    {
      "plugins": ["example"],
      ...
      "env": {
        ...other env parameter
        "example/name_of_the_provided_env": true
        ...
      }
    }
  ```

- 'override': define file-type specific processing

  The specific processing that can be defined are

  - "files": file glob patterns to define the target files
  - "proceessor": provide a processor to extract js code from non-js format file. eslint-plugin-svelte3 provides a processor 'svelte3' that needs to be called to process svelte files as in the following example.

  Parameters not shown here

  - "excludeFiles": file glob patterns on which the override options (processor, rules) will not be applied.
  - "rules": ESLint rules to be applied only on the target of 'files'.

  ```js
    // .eslintrc.json

    {
      ...
      "overrides": [
        {
          "files": ["**/*.svelte"],
          "processor": "svelte3/svelte3"
        }
      ],
    }
  ```

- "extends": set of rules to apply for linting

  ESLint provides a set of linting rules grouped under ["eslint:recommended"][eslint:recommended]

  ```js
    // .eslintrc.json

    {
      ...
      "extends": "eslint:recommended",
      ...
    }
  ```

- "rules": single linting rules to apply with error level and expected value

  These single rules override "extends" rules set.

  ```js
    // .eslintrc.json

    {
      ...
      "rules": {
        "quotes": ["error", "double"]
        }
      ...
    }
  ```

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered">
  Run ESLint from the Terminal
</h3>

To run eslint, add the "lint" script to package.json

```json
  // package.json

  {
    ...
    "scripts": {
      ...
      "lint": "eslint './src/**/*.{js,svelte}'"
    },
    ...
  }
```

On the terminal, type `npm run lint` (in case of pnpm for package manager `pnpm run lint`)

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered">
  Run Prettier and ESLint 
</h3>

If the editor Prettier plugin is set on, disable it momentarily.

Set the following scripts in package.json

```js
// package.json

{
  ...
  "scripts": {
      ...
      "lint": "eslint './src/**/*.{js,svelte}'",
      "format": "prettier --write ./src/**/*.{js,svelte} ./src/*.{js,svelte}",
      "prelint": "pnpm run format"
      ...
  }
}
```

When lint is, `prelint` will be automatically called (if on pnpm, set `enable-pre-post-scripts` to `true`) which itself calls prettier.

The prettier script "format" has a second file glob pattern to cover the fact that it misses svelte file placed inside './src/'.

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered">
  with VScode
</h3>

To activate live application of lint rules, edit VSCode settings.json to add 'svelte' in the 'eslint.validate' array parameter

```js
  // VScode settings.json

  {
    ...
    "eslint.validate": [
      "html", "javascript", "javascriptreact", "svelte"
    ],
  }
```

# Typescript

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered"> 
  Manually add Typescript to the template
</h3>

As this project has not been initialized with the official Svelte template but with a Vite/Svelte (js) template, we cannot use the setupTypescript.js script to add Typescript, as it has step where it updates a rollup config file that is internally taken care of by Vitejs.

By mixing the steps in setupTypescript.js and the Vite svelte-ts template starting files, follow in order the following steps to add Typescript to the project

- add the missing Typescript development dependencies :

  svelte-check, svelte-preprocess, @rollup/plugin-typescript, typescript, tslib, @tsconfig/svelte

- add a script to use svelte-check to scan for ts error in svelte components :

  ```js
    // package.json

  {
    ...
    scripts{
      ...
      "check":"svelte-check --tsconfig ./tsconfig.json"
    }
  }
  ```

- change all .js files into .ts files

- add 'lang="ts"' to all script tags in svelte components

- create a tsconfig.json file in the root folder with:

  ```js
  {
    "extends": "@tsconfig/svelte/tsconfig.json",
    "include": [
      "src/**/*.d.ts",
      "src/**/*.ts",
      "src/**/*.svelte"],
    "exclude": ["node_modules/*", "public/*"]
  }
  ```

  "compilerOptions" can be added later.

- create a svelte.config.js file in root folder with :

  ```js
  import sveltePreprocess from 'svelte-preprocess';

  export default {
    preprocess: sveltePreprocess(),
  };
  ```

- change 'lint' scripts to remove checks on .js files to checks on .ts files

<h3 
  style="padding: 4px; 
  background: white; 
  color: orangered"> 
  Setup ESLint for Typescript
</h3>

After those steps, when I create a new variable with a type annotation, VSCode shows a parsing error. This error is due to the need of a specific parser for ESLint to read Typescript.

Thanks to codechips [Eslint, Svelte and TypeScript][codechips] blog post, those are the steps to add an adapted parser

- add the needed developement Dependencies

  @typescript-eslint/parser @typescript-eslint/eslint-plugin

- modify the .eslintrc.json

  In the context of Vite, we cannot use an .eslintrc.js file as advised by the blog post, I had to check the instructions from eslint-plugin-svelte3 to settle on a working .json file.

  ```js
    // .eslintrc.json

    {
      "root": true,
      "env": {
        "browser": true,
        "es2021": true
      },
      "parser": "@typescript-eslint/parser",
      "parserOptions": {
        "ecmaVersion": 12,
        "sourceType": "module"
      },
      "plugins": ["svelte3", "@typescript-eslint"],
      "overrides": [
        {
          "files": ["**/*.svelte"],
          "processor": "svelte3/svelte3",
          "rules": {
            "quotes": ["warn", "single"]
          }
        }
      ],
      "settings": {
        "svelte3/typescript": true
      },
      "extends": "eslint:recommended",
      "rules": {
        "quotes": ["warn", "single"]
      }
    }
  ```

  We need set :

  - "parser" with "@typescript-eslint/parser"
  - "plugin" with "@typescript-eslint"
  - "settings" with `true` :

    "settings" parameter in .eslintrc files provide shared variables to all plugins.

    the [codechips][codechips] blog post used `require(typescript)` to give the typescript package to the svelte plugin as explained in the official eslint-plugin-svelte documentation.

    As I'm working in the context of Vite, I cannot switch the .json file to .js file.
    I choose the pass a `true` value which will pass typescript as a peer dependencies and it works. I'm relieved !

- Add type-aware lint checks (only on VSCode)

  Codechips blog setting is working in my case as I'm locked in to .json eslint config file. But type-aware rules proposed by @typescript-eslint can be detected in VSCode with these additions to the .eslintrc.json

  ```js
    // .eslintrc.json
    {
      ...
      "parserOptions": {
        ...
        "project": "./tsconfig.json",
        "extraFileExtensions": [".svelte"]
        ...
      },
      "overrides": [
        {
          "files": ["**/*.svelte"],
          "processor": "svelte3/svelte3",
          "extends": [
            "eslint:recommended",
            "plugin:@typescript-eslint/recommended",
            "plugin:@typescript-eslint/recommended-requiring-type-checking"
          ],
          "rules": {
            "quotes": ["warn", "single"]
          }
        }
      ],
      ...
    }
  ```

  For svelte files, declare the linting rules in the extends array and override them in the "rules" parameter.

  Example of rules turn-off :

  ```js
    // .eslintrc.json

    {
      ...
      "overrides": [
        {
          "files": ["**/*.svelte"],
          "processor": "svelte3/svelte3",
          "extends": [
            "eslint:recommended",
            "plugin:@typescript-eslint/recommended",
            "plugin:@typescript-eslint/recommended-requiring-type-checking"
          ],
          "rules": {
            "quotes": ["warn", "single"],
            "@typescript-eslint/no-unsafe-member-access": "off",
          }
        }
      ],
    }

  ```

  Now VSCode is able to detect the unsafe-member-access to the writable store value.
  The CLI command to lint should also be working.

  See [comments][type-aware-github] by gustavopch on Feb.6 2021.

  See [Linting with Type Information][ts-eslint-type-aware] for checks provided by @typescript-eslint

[eslint-environment]: https://eslint.org/docs/user-guide/configuring/language-options#specifying-environments
[eslint-recommended]: https://eslint.org/docs/rules/
[mdnsvelte]: https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Client-side_JavaScript_frameworks/Svelte_getting_started
[ivov-ts]: https://ivov.dev/posts/typescript
[sv-vite-github]: https://github.com/Xuffasch/svelte-vite-template
[codechips]: https://codechips.me/eslint-svelte-typescript/
[type-aware-github]: https://github.com/sveltejs/eslint-plugin-svelte3/issues/68#issuecomment-774531870
[ts-eslint-type-aware]: https://github.com/typescript-eslint/typescript-eslint/blob/master/docs/getting-started/linting/TYPED_LINTING.md
