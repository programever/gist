___Compiler___

The Next.js Compiler, written in Rust using SWC.

___Turbopack___

Turbopack is an incremental bundler optimized for JavaScript and TypeScript, written in Rust.

On large applications, Turbopack updates 700x faster than Webpack.

___Server Side Rendering___

You can render React components on the server side, before sending the HTML to the client.

Next.js provides a backend that can server side render a response to request, allowing you to create a dynamic website, which means you will deploy it on a platform that can run Node.js.

VS: Gatsby is a static site generator, without a server. You build the site, and then you deploy the result of the build process statically on Netlify or another static hosting site.

___Streaming SSR___

Allows you to break down the page's HTML into smaller chunks and progressively send those chunks from the server to the client.

___Server Component___

Large dependencies that previously would impact the JavaScript bundle size on the client can instead remain entirely on the server, leading to improved performance.

So we have server component to prevent this situation.

___Hot Code Reloading___

Next.js reloads the page when it detects any change saved to disk.

___Automatic Routing___

Any URL is mapped to the filesystem, to files put in the pages folder, and you don't need any configuration (you have customization options of course).

___Single File Components___

Using styled-jsx, completely integrated as built by the same team, it's trivial to add styles scoped to the component.

___Ecosystem Compatibility___

Next.js plays well with the rest of the JavaScript, Node, and React ecosystem.

___Automatic Code Splitting___

Pages are rendered with just the libraries and JavaScript that they need, no more. Instead of generating one single JavaScript file containing all the app code, the app is broken up automatically by Next.js in several different resources.

___Prefetching___

The Link component, used to link together different pages, supports a prefetch prop which automatically prefetches page resources (including code missing due to code splitting) in the background.

___Dynamic Components___

You can import JavaScript modules and React Components dynamically.

___Static Exports___

Using the next export command, Next.js allows you to export a fully static site from your app.

___TypeScript Support___

Next.js is written in TypeScript and as such comes with an excellent TypeScript support.

___AMP Support___

Turn any React page into an AMP (Accelerated Mobile Pages), with minimal config, and without leaving React.

___MDX Support___

Provide a better way to write MD file.

___Preview Mode___

Not really useful.

