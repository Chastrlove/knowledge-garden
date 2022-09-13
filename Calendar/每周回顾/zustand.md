---
date created: 2022-09-05
date modified: 2022-09-05
title: zustand
---

```typescript
import { useSyncExternalStoreWithSelector } from 'use-sync-external-store/shim/with-selector';

function createStore(createState: Function) {
  let state: any;
  const listeners = new Set<Function>();
  const setState = (partial: any, replace: any) => {
    const nextState = typeof partial === 'function' ? partial(state) : partial;
    if (nextState !== state) {
      const previousState = state;
      state = replace ? nextState : Object.assign({}, state, nextState);
      listeners.forEach((listener) => listener(state, previousState));
    }
  };
  const getState = () => state;
  const subscribe = (listener: any) => {
    listeners.add(listener);
    return () => listeners.delete(listener);
  };
  const destroy = () => listeners.clear();
  const api = { setState, getState, subscribe, destroy };
  state = createState(setState, getState, api);
  return api;
}

function useStore(api: any, selector: any, equalityFn: any) {
  return useSyncExternalStoreWithSelector(
    api.subscribe,
    api.getState,
    api.getState,
    selector || api.getState,
    equalityFn,
  );
}

export function create(createState: Function) {
  const api = createStore(createState);
  const useBoundStore = (selector?: any, equalityFn?: any) => {
    return useStore(api, selector, equalityFn);
  };
  Object.assign(useBoundStore, api);
  return useBoundStore;
}

```
