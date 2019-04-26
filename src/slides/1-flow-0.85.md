## [v0.85.0](https://github.com/facebook/flow/releases/tag/v0.85.0)

[Asking for Required Annotations](https://medium.com/flow-type/asking-for-required-annotations-64d4f9c1edf8), Oct 30, 2018

[Upgrading Flow Codebases](https://medium.com/flow-type/upgrading-flow-codebases-40ef8dd3ccd8), Apr 10, 2019

---

![https://twitter.com/mweststrate/status/1121035541561192448](https://i.imgur.com/kgNn3h8.png)

---

> All my connects are broken

---

> Missing type annotation for `SP`. `SP` is a type parameter declared in function type [1] and was implicitly instantiated
at call of `connect` [2].

- `connect()`
- `createContext()`
- `createReducer()`
- ... hocs

---

### Fixing `createContext`

```diff
type ContextType = {
  theme: string,
  setTheme: mixed => void,
};

- export const ThemeContext = React.createContext({
+ export const ThemeContext = React.createContext<ContextType>({
  theme: 'default',
  setTheme: () => {},
});
```

---

> But don't do that, to `connect`, why?

---

### A less important reason: It's subject to Flow-Typed

```jsx
export default connect<
  Props,
  OwnProps, // <- take out props fed in by connect
  _,
  _,
  _,
  _,
>(
  mapState,
  mapDispatch
)(MyHappyComponentNow);
```

---

### But mainly because... check [this](https://flow.org/try/#0JYWwDg9gTgLgBAKjgQwM5wEoFNkGN4BmUEIcA5FDvmQNwBQdA9I3DMgNZbrJy4mQA7LAPgwAFsngATCFzgCI8CQDcscMMTDlkZOMAF6YdGAE8wagO7BxAQQAKm9AF44AbxQAuOKhhR9AczgAHzhlCGApOABfegIAVwF8YAgDK1sAHjsvVyiAPgAKAHUoZDBzKQBhfhThGC9sPBgAOhsAIx8S-CrwGpFM3IBKLzoG-Bb230buwVr0gBIAEWACAkyAGjg0sXtHXNy4Olc6ODhKGDioA3yNCDBUL0Xl1bsNrZ3b1EG4J33jk7h0sVSuVpr14MgnK4yDoom4mvCbndYYxcvQogxTOZNtYxAAhBwfb5uOCtLwdALBULhSIxOjxRIwZKpHG4zLZPJFEplLCVapCET1KjNNodKZ82Z2QbDUbCiadGCg-kweZLFbrbHifG7X5HE5nC5XRH3OCPNUvDV4gl3L4-A7-AFA7m8npKkmQsitMiw1zwppG5Goujo4xmNQAWRMitqAHkLAIrc43H8AB5k3wBIMMZisMRqPgu2o5yR6dAWLnlC1wGzeMQQOIAG0iKjUeFwXHQMAg2jIIaxEajIkKOPedyJ-fFIlj8cccAAZBaR6h6HwBD44ABVACSQ9sROuji844Lg+HCYG3326SkwGUuVcRqayCi6UY19vdBXa8d5WrLje6SPGYT1sBMCi3HdtgGegmBYVBQGAetkCgesTErXFe3DSMJxgCCtUJFxALBKcEznC08LuZcUjXcCWT3I1Dyw48cJZM8LwBN87wfVpn1fG9cg-Kj4G-HlcSJLZWUIpVcNA-IaM1KCsxYatkAESJ0MxTCB2Y2wbFU8jE0kmM4xI+c3hMsiE0o1d4Dk7ZdKkUSXH3D4GK0iCbHs-Tz1tK8+L+e9HEfdETgCj4mm4-zGAQJAH1wOAsGTcx8HiqBiCgRBGHRF8OKYaL4sSrBkqwVLoFLHE4EM4C7L0kithzNRazilJ6rgessAIeAEEYATrLgYSpHcgAVXMBEcsiAMYoDtMtXZ8n-Srpo8mrZtspaHIGBTPyE8seVw4bhF-BcJrc09ZvE47sPczyZNWzyNqstcw2AVA4IEfxhPSv9h1k7dhzu6CEsgWA4CkdrkAbeBnLuVzsOIxxvMvGUmgAMRKfwQFqfiAEhAR2qRq2TJwACJlgtOt4AADRrOtGxStKibgFE-hOXHgREuBCZJggybiSnqYbSJivpxn+PtVmnSGkbRM50mtnJuAqdQWsBbp6AGaZsX+r2kaCeJ2WcXlxXldpoW1ZFv4sdcKKkEBwr4FN9KAOe173p29KZe5uXeYV-mTZKqB1f2LqssYJHUeQdHMZoIA) out

```jsx
export default compose(
  withA,
  withB,
  withC,
  connect(mapState, mapDispatch)
)(FlowIsUnhappyAboutMyComponentAgain);
```

---

> We can fix this error by annotating the return type of the function call ([Try Flow](https://flow.org/try/#0GYVwdgxgLglg9mABAJwKbADwBUB8AKASgC5EBvRANwEMAbEVErAHzBBpsQF8yAoRFVFBDIk5anQaJW7LgG4enHgFs4AEzaoAdKgAeABzjIoAZ0QBeRHjTBCJMbXoljUZDDABzFmw6cCsoA)) or by providing an explicit type argument to the function call ([Try Flow](https://flow.org/try/#0GYVwdgxgLglg9mABAJwKbADwBUB8AKASgC5EBvRANwEMAbEVErAHzBBpsQF8yAoRFVFBDIk5anQaJW7LgG4enHgFs4AEzaoAdKgAeABzjIoAZ0QBeAZmNRkMMAHN8BWUA)).
---
# 👀

---

### Fixing `connect`

```diff
type PropTypes = {
  // already annotated
};
const MyComponent = (props: PropTypes) => <div>{props.a}</div>;
- export default compose(connect(mapState, mapDispatch), withA, withB)(MyComponent);
+ export default (
+   compose(
+     connect(mapState, mapDispatch)(MyComponent),
+     withA,
+     withB,
+   ): React.AbstractComponent<PropTypes>
+ );
```

---

![](https://media.giphy.com/media/xULW8qoq0EgTL1E316/giphy.gif)

---

> We don't need to specify the props for each layer of hocs

---

> The actual process of fixing them

730 😳, 8xx 😱, 6xx 😵, 3xx 😰, 100 🤨, 80 🙄, 13 😲, ...

No errors!

---

![](https://res.cloudinary.com/practicaldev/image/fetch/s--ki30knsX--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://i.imgur.com/K3z30Fo.png)

---

> Delayed evaluation of type inference

---

> Try to properly annotate hocs

- [docs](https://flow.org/en/docs/react/hoc/)

---

> Work on smaller packages first

- build simple cases on [Try Flow](https://flow.org/try)

---

> Tests are good sources to learn 🤷🏻‍♀️, [#7493](https://github.com/facebook/flow/issues/7493)

- Flow-Typed tests
- Flow source code tests

---

> Really grateful it passed... with the help from awesome folks

- Thanks Nut for fixing everything in RN
- Thanks Lihau for helping with environment setup, truly non-trivial work in this situation indeed

---

## References 

- [Asking for Required Annotations](https://medium.com/flow-type/asking-for-required-annotations-64d4f9c1edf8)
- [Errors Encountered Upgrading Flow 0.85](https://lihautan.com/errors-encountered-upgrading-flow-0.85/)
- [Making Flow Happy After 0.85](https://dev.wgao19.cc/2019-04-17__making-flow-happy-after-0.85/)
- [What is the underscore type](https://github.com/facebook/flow/issues/7493#issuecomment-466078886)

---

