# Next.js + Sass

Import `.sass` or `.scss` files in your Next.js project

## Installation

```
npm install --save @zeit/next-sass node-sass
```

or

```
yarn add @zeit/next-sass node-sass
```

## Usage

The stylesheet is compiled to `.next/static/css`. Next.js will automatically add the css file to the HTML. 
In production a chunk hash is added so that styles are updated when a new version of the stylesheet is deployed.

### Without CSS modules

Create a `next.config.js` in your project

```js
// next.config.js
const withSass = require('@zeit/next-sass')
module.exports = withSass({
  /* config options here */
})
```

Create a Sass file `styles.scss`

```scss
$font-size: 50px;
.example {
  font-size: $font-size;
}
```

Create a page file `pages/index.js`

```js
import "../styles.scss"

export default () => <div className="example">Hello World!</div>
```

### With CSS modules

```js
// next.config.js
const withSass = require('@zeit/next-sass')
module.exports = withSass({
  cssModules: true
})
```

Create a Sass file  `styles.scss`

```scss
$font-size: 50px;
.example {
  font-size: $font-size;
}
```

Create a page file `pages/index.js`

```js
import css from "../styles.scss"

export default () => <div className={css.example}>Hello World!</div>
```

### With CSS modules and options

You can also pass a list of options to the `css-loader` by passing an object called `cssLoaderOptions`.

For instance, [to enable locally scoped CSS modules](https://github.com/css-modules/css-modules/blob/master/docs/local-scope.md#css-modules--local-scope), you can write:

```js
// next.config.js
const withSass = require('@zeit/next-sass')
module.exports = withSass({
  cssModules: true,
  cssLoaderOptions: {
    importLoaders: 1,
    localIdentName: "[local]___[hash:base64:5]",
  }
})
```

Create a SCSS file `style.scss`

```css
.example {
  font-size: 50px;
}
```

Create a page file `pages/index.js` that imports your stylesheet and uses the hashed class name from the stylesheet

```js
import css from "../style.scss"

const Component = props => {
  return (
    <div className={css.example}>
      ...
    </div>
  )
}

export default Component
```

Your exported HTML will then reflect locally scoped CSS class names.

For a list of supported options, [refer to the webpack `css-loader` README](https://github.com/webpack-contrib/css-loader#options).

### With SASS loader options

You can pass options from [node-sass](https://github.com/sass/node-sass#options)

```js
// next.config.js
const withSass = require('@zeit/next-sass')
module.exports = withSass({
  sassLoaderOptions: {
    includePaths: ["absolute/path/a", "absolute/path/b"]
  }
})
```

### PostCSS plugins

Create a `next.config.js` in your project

```js
// next.config.js
const withSass = require('@zeit/next-sass')
module.exports = withSass({
  /* config options here */
})
```

Create a `postcss.config.js`

```js
module.exports = {
  plugins: {
    // Illustrational
    'postcss-css-variables': {}
  }
}
```

Create a CSS file `styles.scss` the CSS here is using the css-variables postcss plugin.

```css
:root {
  --some-color: red;
}

.example {
  /* red */
  color: var(--some-color);
}
```

When `postcss.config.js` is not found `postcss-loader` will not be added and will not cause overhead.

You can also pass a list of options to the `postcss-loader` by passing an object called `postcssLoaderOptions`.

For example, to pass theme env variables to postcss-loader, you can write:

```js
// next.config.js
const withSass = require('@zeit/next-sass')
module.exports = withSass({
  postcssLoaderOptions: {
    parser: true,
    config: {
      ctx: {
        theme: JSON.stringify(process.env.REACT_APP_THEME)
      }
    }
  }
})
```


### Configuring Next.js

Optionally you can add your custom Next.js configuration as parameter

```js
// next.config.js
const withSass = require('@zeit/next-sass')
module.exports = withSass({
  webpack(config, options) {
    return config
  }
})
```


### TypeScript

You can also use `Sass` with Typescript. 

```ts
// next.config.js
const withTypeScript = require('@zeit/next-typescript')
const withSass = require('@zeit/next-sass')

module.exports = withTypeScript(withSass({
webpack(config, options) {
return config
}
}))

```


Make `typings` folder in the root directory where `pages` belongs to.

```
root
--pages
----yourReact.tsx
--typings
----declarations.d.ts
```

```ts
// declarations.d.ts

declare module "*.scss" {
  const content: { [className: string]: string };
  export = content;
}
```
