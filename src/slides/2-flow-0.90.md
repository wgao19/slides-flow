## [v0.90.0](https://github.com/facebook/flow/releases/tag/v0.90.0)

---

> Cannot call `${X}` because the parameter types of an unknown function `${position}` are unknown

- [remove any generated when refining function from mixed](https://github.com/facebook/flow/commit/2ac7e1bb79ed3cb329cbfca378d71187b9fdfcb1)
- [better error messages for mixed-> function refinements](https://github.com/facebook/flow/commit/55c52d6e82908767d94e08731fb8d8f481301457)

---

```jsx
function fun(x: mixed) {
  if (typeof x === "function") {
    // x previously cast to `any`
    // not anymore
  }
}
```

---

> An actual example, [try flow](https://flow.org/try/#0C4TwDgpgBA8gRgKwgY2AEQPYQM4DkPADCGAdsAIYCWJAggE51QC8UA3gFBRQC25IcEGrQZ8A-AC4elAB4QAJgBp2AXwDc7dhGlgMdYFGSls+8tmwBXbjnp0+zKAAoMiSfCSpMOfEVIVqNgEpmAD42TihKADNHZwQoADJ4qFiAOl5+QWFbEASk0EgMaNT0gSEbPhTqOS0YaKZ6qABySPMSVEpSRqCOLi5ivlKsiqqayIdGgAsIABtpjC71LmUVIA)

---

> Another.... [pitfall?](https://flow.org/try/#0C4TwDgpgBAKg8gJwOIQHYQQQ2BAzjAHhgD4oBeKACkoEpzSY6AfWAbgCh2AzAV1QGNgASwD2qKMEwBrPIhTosOXHC4xwEIuuKVgALlhy0GbHkJrIxOgG92UKEK5VQkEY+DkyFAOS8BwsV7WtnZQ-GK4IgA2EAB0kSIA5jq0NBx2AL5QEJG40DYhoeFRsfFJwKnB6ezpQA)


---

## References 

- [v0.90.0](https://github.com/facebook/flow/releases/tag/v0.90.0)
- [remove any generated when refining function from mixed](https://github.com/facebook/flow/commit/2ac7e1bb79ed3cb329cbfca378d71187b9fdfcb1)
- [better error messages for mixed-> function refinements](https://github.com/facebook/flow/commit/55c52d6e82908767d94e08731fb8d8f481301457)
