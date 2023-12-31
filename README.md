# @jamsocket/javascript

JavaScript/TypeScript, and React libraries for interacting with session backends and the Jamsocket platform.

```sh
npm install @jamsocket/javascript
```

```js
import { init } from '@jamsocket/javascript/server'

const spawnBackend = init({
  account: '[YOUR ACCOUNT]',
  token: '[YOUR TOKEN]',
  service: '[YOUR SERVICE]',
})

// this spawns a backend via the Jamsocket API and returns a URL you can
// use to connect to your backend - or pass the spawnResult to a SessionBackendProvider
// and use Jamsocket's React hooks to interact with your session backend
const spawnResult = await spawnBackend()

// you may also pass in any of the following spawn options
// learn more about spawn options at https://docs.jamsocket.com/api-docs/#spawn-a-service
const spawnResult = await spawnBackend({
  lock: 'my-lock',
  tag: 'my-tag',
  env: { MY_ENV_VAR: 'foo' },
  gracePeriodSeconds: 300,
  requireBearerToken: true,
})
```

```jsx
import { SessionBackendProvider } from '@jamsocket/javascript/react'

// wrap a component in a SessionBackendProvider so the child components
// can use Jamsocket hooks for interacting with the session backend (like useEventListener, useSend, etc)
<SessionBackendProvider spawnResult={spawnResult}>
  <MyComponent />
</SessionBackendProvider>
```

```js
import { useEventListener, useReady, useSend } from '@jamsocket/javascript/react'

function MyComponent() {
  const sendEvent = useSend()
  function onMouseMove() {
    // send an event message to your session backend over websockets
    sendEvent('some-event', someValue)
  }

  useEventListener('another-event', (args) => {
    // do something when receiving an event message from your session backend...
  })
}
```
