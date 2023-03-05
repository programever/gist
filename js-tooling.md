# Build/task runners

__Typical examples:__ Gulp, Grunt

These are not exactly build tools in and of themselves; they're rather just used to glue together other tools. 
Example, if you have a set of build steps where you need to run tool A after tool B, a build runner can help to orchestrate those tools.

# Build tools

__Typical examples:__ Make, Backpack

These tools can generate production build artifacts - web apps, server-only apps, libraries - through an automated process including multiple tasks like compiling, packaging, testing, linking, etc.
Example, with Make, you can have `make run web-server` `make run test` to execute your CLI instead of using `npm run xxx`

# Bundlers

__Typical examples:__ Browserify, Webpack, Parcel, Fusebox, Rollup

These tools take a bunch of `.js` files that use [modules](https://nodejs.org/api/modules.html) (either CommonJS using `require()` statements, or ES Modules using `import` statements), and combine them into a *single* `.js` file. Some of them also allow specifying 'transformation steps', but their main purpose is bundling.

Why does bundling matter? While in Node.js you have access to a module system that lets you load files as-needed from disk, this wouldn't be practical in a browser; fetching every file individually over the network would be very slow. That's why people use a bundler, which effectively does all this work upfront, and then produces a single 'combined' file with all the same guarantees of a module system, but that can be used in a browser.

Bundlers can also be useful for running module-using code in very basic JS environments that don't have module support for some reason; this includes Google Sheets, extensions for PostgreSQL, GNOME, and so on.

# Transpilers (also called transcompilers)

__Typical examples:__ Babel, the TypeScript compiler, CoffeeScript

These tools take a bunch of code in one language, and 'compile' it to another language. They're called commonly 'transpilers' rather than 'compilers' because unlike traditional compilers, these tools don't compile to a lower-level representation; they're just different languages at a similar level of abstraction. Most transpilers use Abstract Syntax Tree (AST) as intermediate format while processing source file, transforming syntax, performing optimizations.

These are typically used to run code written against newer JS versions in older JS runtimes (eg. Babel), or to provide custom languages with more conveniences or constraints that can then be executed in any regular JS environment (TypeScript, CoffeeScript).

# Module loaders

__Typical examples:__ RequireJS, SystemJS

Module loaders are libraries that can handle loading modules using the above formats for further processing or executing, they may be different in terms of synchronous or asynchronous loading, static or dynamic loading.

# Process restarters

__Typical examples:__ nodemon

These tools automatically restart your (Node.js) process when the underlying code is changed. This is used for development purposes, to remove the need to manually restart your process every change.

A process restarter may either watch for file changes itself, or be controlled by an external tool like a build runner.

# JS/TS runner

__Typical examples:__ node, ts-node

Basically, these tools allow you to execute `.js` or `.ts`

# Page reloaders

__Typical examples:__ LiveReload, BrowserSync, Webpack hot-reload

These tools automatically refresh a page in the browser and/or reload stylesheets and/or re-render parts of the page, to reflect the changes in your *browser-side* code. They're kind of the equivalent of a process restarter, but for webpages.

These tools are usually externally controlled; typically by either a build runner or a bundler, or both.

# Static type checkers

__Typical examples:__ TypeScript, Flow

JavaScript only supports dynamic type checking, which means type safety is only verified at runtime; it brings the flexibility to the language but allows unexpected errors at runtime.

Static type checking, the process of checking type safety based on source code at compile-time, has several awesome benefits like caching errors early, limiting type errors, supporting auto-completion, generating documentation, and resulting in faster compilation.

# Code linters

__Typical examples:__ ESLint, JSLint, JSHint

These tools analyze source code to detect problems based on formatting and code quality rules then output as warnings or errors. They are used to increase code quality, configured manually, and often run automatically when code changes.

Even though JavaScript compilers or static type checkers have evolved to include many of linting functions, linters have also evolved to detect an even wider variety of suspicious constructs: warnings about syntax errors, uses of undeclared variables, calls to deprecated functions, spacing and formatting conventions, misuse of scope, implicit fallthrough in switch statements, or missing license headers.

# Code formatters

__Typical examples:__ Prettier, StandardJS

These tools to format the codebase automatically based on formatting rules to enforce a consistent coding style. They can behave like linters in term of formatting rules, but can not do anything to help with code quality rules. They can also be integrated into workflows with linters to do both formatting and linting in one step.

# Debuggers

__Typical examples:__ Chrome Developer Tools, node-inspect

These tools allow you to inspect *running* code; in Node.js, in your browser, or both. Typically they'll support things like pausing execution, stepping through function calls manually, inspecting variables, profiling memory allocations and CPU usage, viewing execution logs, and so on.

They're typically used to find tricky bugs. It's a good idea to learn how these tools work, but often it'll still be easier to find a bug by just 'dumb logging' variables throughout your code using eg. `console.log`.

# Node process managers

__Typical examples:__ Forever, PM2, Strong-PM, SystemD

These tools to manage your Node applications at run time; they provide high availability, automatic restart, file watcher, runtime performance and resource consumption insights, and clustering.

# NodeJS version manager

__Typical examples:__ nvm, fnm

These tools provide the ability to use multiple versions of Node.js within your workspace.

# Package managers

__Typical examples:__ npm, yarn, bower

These tools help you manage packages as dependencies and might also provide a global package registry. They work based on manifest files that keep track application metadata and needed dependencies, lockfiles to offer deterministic installs.

# Monorepo tools

__Typical examples:__ Lerna, Nx, Yarn workspaces

Monorepo systems in JavaScript are tools to manage JavaScript projects with multiple packages, useful for code sharing; making changes across many repositories; testing across repositories.

Projects like Babel, React, Angular,Ember, Meteor, Jest, and many other open-source projects develop all of their packages within a single repository.

# NodeJS

Node.js is a JavaScript runtime environment.

***Here is how PHP or ASP handles a file request:***
Sends the task to the computer's file system.
Waits while the file system opens and reads the file.
Returns the content to the client.
Ready to handle the next request.

***Here is how Node.js handles a file request:***
Sends the task to the computer's file system.
Ready to handle the next request.
When the file system has opened and read the file, the server returns the content to the client.

# Web Application

__Typical examples:__ ExpressJS, Fastify

This frameworks designed to build API's web applications cross-platform mobile apps quickly and make NodeJS easy.

# References:

https://gist.github.com/joepie91/3381ce7f92dec7a1e622538980c0c43d
https://byby.dev/js-tooling
