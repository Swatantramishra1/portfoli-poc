# nextjs-tailwind-typescript-boilerplate

First of all we’ll create our project folder and init our project with some default configurations:

take nextjs-typescript-tailwind && npm init -y
Great, now into handling Next.js, we’ll need to install 3 packages and change the scripts on our package.json file:

Install the necessary packages:

npm i next react react-dom
And, replace our scripts:

"scripts": {
  "dev": "next",
  "build": "next build",
  "start": "next start"
}
Awesome. We can now run npm run dev. This will probably throw an error, but that’s fine for now.

Error: > Couldn't find a `pages` directory. Please create one under the project root
This is expected since Next.js needs a pages folder with a file in it.

To create this and silence the error, run:

mkdir pages && touch pages/index.js
Now when you run npm run dev the server should start without any issues. Before putting some content in our file, let’s handle TypeScript.

TypeScript
We’ll need to create a tsconfig.json file. When running npm run dev Next.js will tell us what we’re missing to get TypeScript working.

Try running:

touch tsconfig.json && npm run dev
You may see something like:

It looks like you're trying to use TypeScript but do not have the required package(s) installed.

Please install typescript, @types/react, and @types/node by running:

npm install --save-dev typescript @types/react @types/node
To install these dependencies, run:

npm install --save-dev typescript @types/react @types/node
Now everything should work fine when you run npm run dev. This also means we can rename the index.js file to index.tsx. To do this, run:

mv pages/index.js pages/index.tsx
Now let’s add some content to it. Open pages/index.tsx and add the following:

function Homepage() {
  return <>Our homepage</>
}

export default Homepage
Run npm run dev and you should have a working application now.

TailwindCSS
Installing Tailwind in a Next.js application is very simple. First you’ll need to install 3 packages by running:

npm install tailwindcss@latest postcss@latest autoprefixer@latest
Configuration files
We’ll need 2 configuration files, tailwind.config.js and postcss.config.js, to add these, run:

npx tailwindcss init -p
Then open the new tailwind.config.js file and add the following:

module.exports = {
  purge: [
    './pages/**/*.{js,ts,jsx,tsx}',
    './components/**/*.{js,ts,jsx,tsx}'
  ],
  darkMode: false, // or 'media' or 'class'
  theme: {
    extend: {},
  },
  variants: {
    extend: {},
  },
  plugins: [],
}
Most tutorials and documentation don’t mention that ts and tsx should be included as part of the purge config. When that’s the case, you might see an application without any styling at all after deployed.

Alright, let’s add some styling to the Homepage component:

function Homepage() {
  return (
    <h1 className="text-center my-24 font-black tracking-tight text-6xl">Our homepage</h1>
  )
}

export default Homepage

Looks like it’s not working, right? That’s because we need to add styles to the app.

To do that, first create a file called _app.tsx inside the pages folder:

touch pages/_app.tsx
Then create the css file by running:

touch styles/base.css
Inside the _app.tsx we’ll put the Next.js default component and add styles by importing the new base.css file:

import '../styles/base.css'

function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}

export default MyApp
Then, inside the base.css file, we can import Tailwind CSS utility classes:

@tailwind base;
@tailwind components;
@tailwind utilities;
Now when you run the server, the Our Homepage title should look much better.

Restrict
The primary benefit of using TypeScript is to enforce types in the application. Now that the application is up and running, let’s get TypeScript configured.

To do so, we just need to change a configuration in the tsconfig.json file:

{
  "compilerOptions": {
    // ...
    "strict": true,
    // ...
  }
}
This changes strict from false to true. Depending on the IDE you’re using, you should start seeing warning that some props or components are missing a type.

If you take a look at _app.tsx, you should see 2 warnings:

Binding element 'Component' implicitly has an 'any' type.
Binding element 'pageProps' implicitly has an 'any' type.
To set the types of both Component and pageProps, use the AppProps that Next.js provides:

import type { AppProps } from 'next/app'

import '../styles/base.css'

function MyApp({ Component, pageProps }: AppProps) {
  return <Component {...pageProps} />
}

export default MyApp
