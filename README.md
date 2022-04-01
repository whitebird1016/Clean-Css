# Learning Css-in-Next.js with Shikamaru

![shikamaru](https://github.com/whitebird1016/Clean-Code-in-JavaScript/blob/main/CUEVhE5.webp)

<h2>1. Adding a Global Stylesheet </h2>

<p>To add a stylesheet to your application, import the CSS file within pages/_app.js.</p>

<p>For example, consider the following stylesheet named styles.css:</p>

```

body {
  font-family: 'SF Pro Text', 'SF Pro Icons', 'Helvetica Neue', 'Helvetica',
    'Arial', sans-serif;
  padding: 20px 20px 60px;
  max-width: 680px;
  margin: 0 auto;
}

```
<p>Create a pages/_app.js file if not already present. Then, import the styles.css file.</p>

```
import '../styles.css'

// This default export is required in a new `pages/_app.js` file.
export default function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}

```
<p>These styles (styles.css) will apply to all pages and components in your application. Due to the global nature of stylesheets, and to avoid conflicts, you may only import them inside pages/_app.js.</p>

<p>In development, expressing stylesheets this way allows your styles to be hot reloaded as you edit them—meaning you can keep application state.</p>

<p>In production, all CSS files will be automatically concatenated into a single minified .css file.</p>

<h2>2. Import styles from node_modules </h2>

```
// pages/_app.js
import 'bootstrap/dist/css/bootstrap.css'

export default function MyApp({ Component, pageProps }) {
  return <Component {...pageProps} />
}
  
```

```
// components/ExampleDialog.js
import { useState } from 'react'
import { Dialog } from '@reach/dialog'
import VisuallyHidden from '@reach/visually-hidden'
import '@reach/dialog/styles.css'

function ExampleDialog(props) {
  const [showDialog, setShowDialog] = useState(false)
  const open = () => setShowDialog(true)
  const close = () => setShowDialog(false)

  return (
    <div>
      <button onClick={open}>Open Dialog</button>
      <Dialog isOpen={showDialog} onDismiss={close}>
        <button className="close-button" onClick={close}>
          <VisuallyHidden>Close</VisuallyHidden>
          <span aria-hidden>×</span>
        </button>
        <p>Hello there. I am a dialog</p>
      </Dialog>
    </div>
  )
}

```

<h2>3. Adding Component-Level CSS
 </h2>
<ul>Next.js supports CSS Modules using the [name].module.css file naming convention.

CSS Modules locally scope CSS by automatically creating a unique class name. This allows you to use the same CSS class name in different files without worrying about collisions.

This behavior makes CSS Modules the ideal way to include component-level CSS. CSS Module files can be imported anywhere in your application.

For example, consider a reusable Button component in the components/ folder:

First, create components/Button.module.css with the following content:
</ul>

```

/*
You do not need to worry about .error {} colliding with any other `.css` or
`.module.css` files!
*/
.error {
  color: white;
  background-color: red;
}
  
```
<p>Then, create components/Button.js, importing and using the above CSS file:</p>

```
import styles from './Button.module.css'

export function Button() {
  return (
    <button
      type="button"
      // Note how the "error" class is accessed as a property on the imported
      // `styles` object.
      className={styles.error}
    >
      Destroy
    </button>
  )
}
  
```


<h2>4. Sass Support</h2>

```

npm install --save-dev sass
  
```
<p>If you want to configure the Sass compiler you can do so by using sassOptions in next.config.js.

For example to add includePaths:</p>

```
const path = require('path')

module.exports = {
  sassOptions: {
    includePaths: [path.join(__dirname, 'styles')],
  },
}
  
```

```
/* variables.module.scss */
$primary-color: #64FF00

:export {
  primaryColor: $primary-color
}
// pages/_app.js
import variables from '../styles/variables.module.scss'

export default function MyApp({ Component, pageProps }) {
  return (
    <Layout color={variables.primaryColor}>
      <Component {...pageProps} />
    </Layout>
  )
}

```


<h2>5. CSS-in-JS</h2>

```

function HiThere() {
  return <p style={{ color: 'red' }}>hi there</p>
}

export default HiThere  
```

```
function HelloWorld() {
  return (
    <div>
      Hello world
      <p>scoped!</p>
      <style jsx>{`
        p {
          color: blue;
        }
        div {
          background: red;
        }
        @media (max-width: 600px) {
          div {
            background: blue;
          }
        }
      `}</style>
      <style global jsx>{`
        body {
          background: black;
        }
      `}</style>
    </div>
  )
}

export default HelloWorld
  
```

