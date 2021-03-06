# create-emotion

### Create distinct instances of emotion.

The main [emotion](https://github.com/emotion-js/emotion/tree/master/packages/emotion) package can be thought of as a call to `createEmotion` with sensible defaults for most applications.

```javascript
import createEmotion from 'create-emotion'

const context = typeof global !== 'undefined' ? global : {}

export const {
  flush,
  hydrate,
  cx,
  merge,
  getRegisteredStyles,
  injectGlobal,
  keyframes,
  css,
  sheet,
  cache
} = createEmotion(context)
```

### Upside

- Calling it directly will allow for some low level customization.

- Create custom names for emotion APIs to help with migration from other, similar libraries.

- Could set custom `key` to something other than `css`

### Downside

- Introduces some amount of complexity to your application that can vary depending on developer experience.

- Required to keep up with changes in the repo and API at a lower level than if using `emotion` directly

### Primary use cases

- Using emotion in embedded contexts such as an `<iframe/>`

- Setting a [nonce]() on any `<style/>` tag emotion creates for security purposes

- Use emotion with a developer defined `<style/>` tag

- Using emotion with custom stylis plugins

## Multiple instances in a single app example

```jsx
import createEmotion from 'create-emotion'

export const {
  flush,
  hydrate,
  cx,
  merge,
  getRegisteredStyles,
  injectGlobal,
  keyframes,
  css,
  sheet,
  cache
} = createEmotion({
  // The key option is required when there will be multiple instances in a single app
  key: 'some-key'
})
```

## Options

### nonce: string

A nonce that will be set on each style tag that emotion inserts for [Content Security Policies](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP).

### stylisPlugins: Function | Array<Function>

A Stylis plugin or plugins that will be run by stylis during preprocessing. [Read Stylis' docs to find out more](https://github.com/thysultan/stylis.js#plugins). This can for be used for many purposes such as RTL.

### prefix: boolean | Function

Allows changing Stylis' prefixing settings, this defaults to `true`. It can be a boolean or a function to dynamicly set which properties are prefixed. [More information can be found in Stylis' docs](https://github.com/thysultan/stylis.js#vendor-prefixing)

### key: string

The prefix before class names, this defaults to `css`. It will also be set as the value of the `data-emotion` attribute on the style tags that emotion inserts and it's used in the attribute name that marks style elements in `renderStylesToString` and `renderStylesToNodeStream`. This is **required if using multiple emotion instances in the same app**.

### container: HTMLElement

A DOM Node that emotion will insert all of it's style tags into, this is useful for inserting styles into iframes.
