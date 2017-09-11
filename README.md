sass-extract-js
---

[![npm](https://img.shields.io/npm/v/sass-extract-js.svg?style=flat-square)](http://www.npmjs.com/package/sass-extract-js)

Plugin for [sass-extract][sass-extract] to convert Sass global variables into a plain JS object.

## Huh? Why?

I work on a team that uses Sass. We've got a shared variables file that gets referenced throughout our styleguide and in our components. I wanted to start using [styled-components][] in some projects and wanted to keep things consistent but I didn't want to have to copy our Sass variables over. Using this plugin with [sass-extract][sass-extract], I can simply extract the global variables from our Sass stylesheet and they will be converted into a JS object that gets passed down to my components via styled-components' [`<ThemeProvider>`][theming].

## Cool! How do I get it?

You'll need to install [sass-extract][sass-extract], [node-sass][node-sass], and this plugin.

```sh
$ yarn add sass-extract node-sass sass-extract-js
```

*(npm install works too)*

## Nice! How do I use it?

Wrap your App in a ThemeProvider component like this:

```jsx
// Import `sass-extract` library
import sassExtract from 'sass-extract';

// Call the `renderSync` function with the path to your Sass file
// and specify the `sass-extract-js` plugin
const rendered = sassExtract.renderSync({
  file: './path/to/vars.scss'
}, {
  plugins: ['sass-extract-js']
});

// Pass the vars into your ThemeProvider component
render(
  <ThemeProvider theme={rendered.vars}>
    <App />
  </ThemeProvider>
);
```

Then use themes in your styled components:

```js
import styled from 'styled-components';

const Button = styled.button`
  background-color: ${props => props.theme.primary}
`;

```

## Sweet! What does the output look like?

Given a Sass file with some global variable declarations:

```sass
$primary: rgb(255, 202, 77);
$seondary: #1A93C8;
$primary-light: lighten($primary, 20%);
$base-padding: 10px;
$base-margin: 0 1em;
$base-border: 1px solid #ccc;
$font-family-sans: 'Helvetica', 'Arial', sans-serif;
$base-font-size: 16px;
$line-height: $base-font-size * 1.8;
```

It will yield the following object:

```js
{ 
  primary: 'rgb(255, 202, 77)',
  seondary: 'rgb(26, 147, 200)',
  primaryLight: 'rgb(255, 232, 179)',
  basePadding: '10px',
  baseMargin: '0 1em',
  baseBorder: '1px solid rgb(204, 204, 204)',
  fontFamilySans: '\'Helvetica\', \'Arial\', sans-serif',
  baseFontSize: '16px',
  lineHeight: '28.8px'
}
```

## Wait! What if I don't use styled-components?

No problem! I made this so I could use the extracted JS object as a theme but it's not specific to [styled-components][styled-components]. It should work the same with [glamorous][glamorous] too. Really you can use this plugin for any scenario where you need to convert Sass vars to JS.

## Help! It's not working for me.

This project is open source. I've tried to make sure it works for a lot of use cases (read: mine) but if I missed yours, feel free to [open an issue][issues]. Better yet, [submit a PR][pr]! Seriously, any feedback and help is welcome.

[issues]: https://github.com/adamgruber/sass-extract-js/issues
[pr]: https://github.com/adamgruber/sass-extract-js/pulls
[styled-components]: https://www.styled-components.com/
[theming]: https://www.styled-components.com/docs/advanced#theming
[node-sass]: https://github.com/sass/node-sass#options
[sass-extract]: https://github.com/jgranstrom/sass-extract
[glamorous]: https://github.com/paypal/glamorous