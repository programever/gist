# Main Features

___Hot Code Reloading___

Next.js reloads the page when it detects any change saved to disk.

___Automatic Routing___

Any URL is mapped to the filesystem, to files put in the pages folder, and you don't need any configuration (you have customization options of course).

___Single File Components___

Using styled-jsx, completely integrated as built by the same team, it's trivial to add styles scoped to the component.

___Server Rendering___

You can render React components on the server side, before sending the HTML to the client.

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

# Server side rendering vs Gatsby

They can both help with server-side rendering, but in 2 different ways.

The end result using Gatsby is a static site generator, without a server. You build the site, and then you deploy the result of the build process statically on Netlify or another static hosting site.

Next.js provides a backend that can server side render a response to request, allowing you to create a dynamic website, which means you will deploy it on a platform that can run Node.js.

