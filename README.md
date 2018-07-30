# Renaox

[![NPM version][npm-image]][npm-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/renaox.svg?style=flat-square
[npm-url]: https://npmjs.org/package/renaox
[download-image]: https://img.shields.io/npm/dm/renaox.svg?style=flat-square
[download-url]: https://npmjs.org/package/renaox

a boring state management

## Install

```bash
npm install --save renaox
```

## Example

### 一个 Contro 的时候

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Store, Provider, orm } from 'renaox';

const Count = props => {
  return (
    <div>
      <div>count : {props.count}</div>
      <button onClick={props.addCount}>count + 1</button>
      <button onClick={props.asyncAddCount}>async count + 1</button>
      <button onClick={props.subtractCount}>count - 1</button>
    </div>
  );
};

const mapState = state => ({
  count: state.count,
});

const mapMethods = methods => ({
  addCount: methods.addCount,
  subtractCount: methods.subtractCount,
  asyncAddCount: methods.asyncAddCount,
});

const CountContainer = orm(mapState, mapMethods)(Count);

const state = {
  count: 0,
};

const methods = {
  syncs: {
    addCount(state) {
      state.count = state.count + 1;
    },
    subtractCount(state) {
      state.count = state.count - 1;
    },
  },
  asyncs: {
    asyncAddCount() {
      setTimeout(this.addCount, 1e3);
    },
  },
};

const store = new Store({ state, methods });

ReactDOM.render(
  <Provider store={store}>
    <CountContainer />
  </Provider>,
  document.getElementById('root')
);
```

### 多个 Contro 的时候

```js
import React from 'react';
import ReactDOM from 'react-dom';
import { Store, Provider, orm } from 'renaox';
const Count = props => {
  return (
    <div>
      <div>count A: {props.countA}</div>
      <div>
        <button onClick={props.addCountA}>countA + 1</button>
        <button onClick={props.asyncAddCountA}>async countA + 1</button>
        <button onClick={props.subtractCountA}>countA - 1</button>
      </div>
      <div>count B: {props.countB}</div>
      <div>
        <button onClick={props.addCountB}>countB + 1</button>
        <button onClick={props.asyncAddCountB}>async countB + 1</button>
        <button onClick={props.subtractCountB}>countB - 1</button>
      </div>
    </div>
  );
};

const mapState = state => ({
  countA: state.controA.count,
  countB: state.controB.count,
});

const mapMethods = methods => ({
  addCountA: methods.controA.addCount,
  subtractCountA: methods.controA.subtractCount,
  asyncAddCountA: methods.controA.asyncAddCount,
  addCountB: methods.controB.addCount,
  subtractCountB: methods.controB.subtractCount,
  asyncAddCountB: methods.controB.asyncAddCount,
});

const CountContainer = orm(mapState, mapMethods)(Count);

const controA = {
  state: {
    count: 0,
  },
  syncs: {
    addCount(state) {
      state.count = state.count + 1;
    },
    subtractCount(state) {
      state.count = state.count - 1;
    },
  },
  asyncs: {
    asyncAddCount() {
      setTimeout(this.addCount, 1e3);
    },
  },
};
const controB = {
  state: {
    count: 0,
  },
  syncs: {
    addCount(state) {
      state.count = state.count + 1;
    },
    subtractCount(state) {
      state.count = state.count - 1;
    },
  },
  asyncs: {
    asyncAddCount() {
      setTimeout(this.addCount, 1e3);
    },
  },
};
const store = new Store({ controA, controB });

ReactDOM.render(
  <Provider store={store}>
    <CountContainer />
  </Provider>,
  document.getElementById('root')
);
```
