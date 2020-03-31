# ✨ Notes ✨

# Styling


- There are two ways to use emotion, and typically you use both of them in any
given application for different use cases. 
  - The first allows you to make a component that "carries its styles with it." 
  - The second allows you to apply styles to a component.

### Making a styled component with emotion

- Here's how you make a "styled component":

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


- You can also use object syntax (this is my personal preference):

```javascript
const Button = styled.button({
  color: 'turquoise',
})
```

- You can even accept props by passing a function and returning the styles!

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

- 📜 https://emotion.sh/docs/styled

### Using emotion's css prop

- The styled component is only really useful for when you need to reuse a
component. For one-off styles, it's less useful. You inevitably end up creating
components with meaningless names like "Wrapper" or "Container".

- Much more often I find it's nice to write one-off styles as props directly on
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

- With that, you're ready to use the CSS prop anywhere in that file:

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

- Ultimately, this is compiled to something that looks a bit like this:

```javascript
function SomeComponent() {
  return <div className="css-bp9m3j">This has a hotpink background.</div>
}
```


- 📜 https://emotion.sh/docs/css-prop

# Make HTTP Requests

## Background

- Here's a quick simple example of that API in action:

```javascript
window
  .fetch('http://example.com/movies.json')
  .then(response => {
    return response.json()
  })
  .then(data => {
    console.log(data)
  })
```

- All the HTTP methods are supported as well, for example, here's how you would POST data:

```javascript
window
  .fetch('http://example.com/movies', {
    method: 'POST',
    headers: {
      'content-type': 'application/json',
      // if auth is required. Each API may be different, but
      // the Authorization header with a token is common.
      Authorization: `Bearer ${token}`,
    },
    body: JSON.stringify(data), // body data type must match "content-type" header
  })
  .then(response => {
    return response.json()
  })
  .then(data => {
    console.log(data)
  })
```

- If the request fails with an unsuccessful status code (`>= 400`), then the
`response` object's `ok` property will be false. It's common to reject the
promise in this case:

```javascript
window.fetch(url).then(async response => {
  const data = await response.json()
  if (response.ok) {
    return data
  } else {
    return Promise.reject(data)
  }
})
```

- It's good practice to wrap `window.fetch` in your own function so you can set
defaults (especially handy for authentication). 
- Integrating this kind of thing with React involves utilizing React's `useEffect`
hook for making the request and `useState` for managing the status of the
request as well as the response data and error information.
- 📜 https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch
