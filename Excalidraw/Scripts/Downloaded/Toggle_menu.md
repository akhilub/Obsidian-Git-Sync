/*
```js*/
const api = ea.getExcalidrawAPI();
const {openMenu} = api.getAppState();
api.updateScene({appState:{openMenu: openMenu?null:"shape"}});