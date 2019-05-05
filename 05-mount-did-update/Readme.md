# 01 Mount and Did Update events

What if we want to execute some code when the component gets mounted
and after any update? React.UseEffect has more flavours available..

# Steps

- We will take as starting point sample _00 boilerplate_ copy the conent of the
  project to a fresh folder an execute _npm install_.

```bash
npm install
```

- Let's open the _demo.js_, we will create a parent and child component as
  we did in previous examples.

_./demo.js_

```jsx
import React from "react";

export class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { visible: false };
  }

  render() {
    return (
      <>
        {this.state.visible && <MyChildComponent />}
        <button onClick={() => this.setState({ visible: !this.state.visible })}>
          Toggle Child component visibility
        </button>
      </>
    );
  }
}

const MyChildComponent = () => {
  const [userInfo, setUserInfo] = React.useState({
    name: "John",
    lastname: "Doe"
  });

  return (
    <div>
      <h3>
        {userInfo.name} {userInfo.lastname}
      </h3>
      <input
        value={userInfo.name}
        onChange={e => setUserInfo({ ...userInfo, name: e.target.value })}
      />
      <input
        value={userInfo.lastname}
        onChange={e => setUserInfo({ ...userInfo, lastname: e.target.value })}
      />
    </div>
  );
};
```

- Now comes the interesting part by calling _React.useEffect_ without second
  parameter, the code inside _useEffect_ will be triggered right when the
  component is just mounted and on any update (clean up function will be called
  right before the effect is triggered again).

_./demo.js_

```diff
const MyChildComponent = () => {
  const [userInfo, setUserInfo] = React.useState({
    name: "John",
    lastname: "Doe"
  });

+ React.useEffect(() => {
+    console.log("A. Called when the component is mounted and after every render");
+
+    return () =>
+      console.log(
+        "B. Cleanup function called after every render"
+      );
+  });

  return (
```

- If we start the project and open the browser console we can check the
  expected behavior.