# Notes


There are two ways to use emotion, and typically you use both of them in any
given application for different use cases. The first allows you to make a
component that "carries its styles with it." The second allows you to apply
styles to a component.

### Making a styled component with emotion

Here's how you make a "styled component":

```javascript
import styled from '@emotion/styled'

const Button = styled.button`
  color: turquoise;
`

// <Button>Hello</Button>
//
//     👇
//
// <button className="css-1ueegjh">Hello</button>
```

This will make a button who's text color is `turquoise`. It works by creating a
stylesheet at runtime with that class name.

You can also use object syntax (this is my personal preference):

```javascript
const Button = styled.button({
  color: 'turquoise',
})
```

You can even accept props by passing a function and returning the styles!

```javascript
const Button = styled.button(props => {
  return {
    color: props.primary ? 'hotpink' : 'turquoise',
  }
})

// or with the string form:

const Button = styled.button`
  color: ${props => (props.primary ? 'hotpink' : 'turquoise')};
`
```

There's lot more you can do with creating styled components, but that should get
you going for this exercise.

📜 https://emotion.sh/docs/styled

### Using emotion's css prop

The styled component is only really useful for when you need to reuse a
component. For one-off styles, it's less useful. You inevitably end up creating
components with meaningless names like "Wrapper" or "Container".

Much more often I find it's nice to write one-off styles as props directly on
the element I'm rendering. Emotion does this using a special prop and a custom
JSX function (similar to `React.createElement`). You can learn more about how
this works from emotion's docs, but for this exercise, all you need to know is
to make it work, you simply add this to the top of the file where you want to
use the `css` prop:

```javascript
/** @jsx jsx */
/** @jsxFrag React.Fragment */
import {jsx} from '@emotion/core'
import React from 'react'
```

With that, you're ready to use the CSS prop anywhere in that file:

```jsx
function SomeComponent() {
  return (
    <div
      css={{
        backgroundColor: 'hotpink',
        '&:hover': {
          color: 'lightgreen',
        },
      }}
    >
      This has a hotpink background.
    </div>
  )
}

// or with string syntax:

function SomeOtherComponent() {
  const color = 'darkgreen'

  return (
    <div
      css={css`
        background-color: hotpink;
        &:hover {
          color: ${color};
        }
      `}
    >
      This has a hotpink background.
    </div>
  )
}
```

Ultimately, this is compiled to something that looks a bit like this:

```javascript
function SomeComponent() {
  return <div className="css-bp9m3j">This has a hotpink background.</div>
}
```

With the relevant styles being generated and inserted into a stylesheet to make
this all work.

📜 https://emotion.sh/docs/css-prop
