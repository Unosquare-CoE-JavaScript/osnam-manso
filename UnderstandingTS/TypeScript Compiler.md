# Using Watch Mode

- tsc appt.ts --watch

# Compiling the Entire Project / Multiple Files

1. tsc --init
2. tsc
3. tsc -w

# Including & Excluding Files

- include - exclude

```jsx
  {
    "exclude": [
        "*.dev.ts",    // any name
        "**/*.dev.ts", // any folder, any name
        "node_modules" // would be the default
    ],
    "include":[
        "app.ts",
        "analytics.ts"
    ],
  }
```

# Setting a Compilation Target

<p align="left">
    <img src="https://www.notion.so/image/https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F4990ef5e-c931-4177-8063-a058ce009a36%2FScreen_Shot_2021-11-01_at_12.07.02.png?id=10b73977-1223-4e11-8b0b-7c13e1ab5ee4&table=block&spaceId=39c865bd-d151-4ddd-bfd8-f2239f411ed9&width=1920&userId=cc2028a7-e873-4ae8-988c-88e12db2775f&cache=v2"/>
</p>

# Understanding TypeScript Core Libs

- "lib": [], Specify a set of bundled library declaration files that describe the target runtime environment. Example:

```jsx
{
    "lib": ["dom", "es6", "dom.iterable", "scripthost"],
}
```

# More Configuration & Compilation Options

- View tsconfig.json

# Working with Source Maps

- "sourceMap": true, Create source map files for emitted JavaScript files. Simplify debugging.

# rootDir and outDir

- "outDir": "./", Specify an output folder for all emitted files.
- "rootDir": "./", Specify the root folder within your source files.
- Example:
  ```jsx
  {
    "outDir":"./dist",
    "rootDir": "./src",
  }
  ```

# Stop Emitting Files on Compilation Errors

- "noEmitOnError": true, Disable emitting files if any type checking errors are reported.

# Strict Compilation

- "strict": true, Enable all strict type-checking options.

# Debugging with Visual Studio Code

- Google Chrome.
- Run -> Start Debugging.
- You can set breakpoint.

# Useful Resources & Links

Attached you find all the code snapshots for this module - you also find them attached to individual lectures throughout this module.

These links might also be interesting:

- tsconfig Docs: https://www.typescriptlang.org/docs/handbook/tsconfig-json.html

- Compiler Config Docs: https://www.typescriptlang.org/docs/handbook/compiler-options.html

- VS Code TS Debugging: https://code.visualstudio.com/docs/typescript/typescript-debugging
