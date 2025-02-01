---
# You can also start simply with 'default'
theme: default
# some information about your slides (markdown enabled)
title: Welcome to Slidev
info: hello
# apply unocss classes to the current slide
class: text-center
# https://sli.dev/features/drawing
drawings:
  persist: false
# slide transition: https://sli.dev/guide/animations.html#slide-transitions
transition: fade
# enable MDC Syntax: https://sli.dev/features/mdc
mdc: true
---

# XState Introduction
Presentation slides for developers

---

# What is XState?

XState 是一個用於狀態管理的 JS 函式庫

利用有限狀態機（FSM）和狀態圖（Statecharts）

來描述與管理應用中的各種狀態與轉換，使邏輯更加明確且易於維護

- 可視化工具 ⭐
- 框架無關性
- 強大的擴充性

能讓開發者更輕鬆地追蹤狀態轉變的過程

還可降低程式因條件混亂或多層 if-else 判斷而導致的錯誤

<img 
  class="absolute w-80 left-145 top-65"
  src='./images/xstate-logo.jpg'
/>

---

# Finite State Machine

有限狀態機（FSM），是一種用來描述系統行為的數學模型

- 有限狀態：系統的所有可能狀態是有限且固定的，每次系統僅能處於其中一個狀態
- 狀態轉換：根據外部輸入或事件，系統會從當前狀態轉換到另一個狀態
- 輸入條件：轉換的觸發條件通常由外部輸入決定，不同的輸入對應不同的轉換路徑
- 動作或輸出：在狀態轉換過程中，系統可能會執行特定動作或產生相應輸出

<img 
  class="m-auto w-42%"
  src='./images/light.png'
/>

---

# Quick Start

````md magic-move {lines: true}
```ts
const toggleMachine = createMachine({
  id: 'toggle',
  initial: 'Inactive',
  states: {
    Inactive: {
      on: { toggle: 'Active' },
    },
    Active: {
      on: { toggle: 'Inactive' },
    },
  },
});
```

```ts
// Create an actor that you can send events to.
// Note: the actor is not started yet!
const actor = createActor(toggleMachine);

// Subscribe to snapshots (emitted state changes) from the actor
actor.subscribe((snapshot) => {
  console.log('Value:', snapshot.value);
});

// Start the actor
actor.start(); // logs 'Inactive'

// Send events
actor.send({ type: 'toggle' }); // logs 'Active'
actor.send({ type: 'toggle' }); // logs 'Inactive'
```

```tsx
import { useMachine } from '@xstate/react';
import { toggleMachine } from './toggleMachine';

const App = () => {
  const [state, send] = useMachine(toggleMachine);

  return (
    <div>
      <div>Value: {state.value}</div>
      <button onClick={() => send({ type: 'toggle' })}>Toggle</button>
    </div>
  );
};
```

```ts {*|3|12-14}
const toggleMachine = createMachine({
  id: 'toggle',
  context: { count: 0 },
  initial: 'Inactive',
  states: {
    Inactive: {
      on: { 
        toggle: 'Active' 
      },
    },
    Active: {
      entry: assign({
        count: ({ context }) => context.count + 1,
      }),
      on: { toggle: 'Inactive' },
    },
  },
});
```

```ts {*|3-6|12}
const toggleMachine = createMachine({
  id: 'toggle',
  context: ({ input }) => ({
    count: 0,
    maxCount: input.maxCount,
  }),
  initial: 'Inactive',
  states: {
    Inactive: {
      on: {
        toggle: {
          guard: ({ context }) => context.count < context.maxCount,
          target: 'Active',
        },
      },
    },
    Active: {
      entry: assign({
        count: ({ context }) => context.count + 1,
      }),
      on: { toggle: 'Inactive' },
    },
  },
});
```
````

---

# State Chart Demo

以達哥 Codebase 中，會出現的各種主要狀態做範例

- home
- directChat
- assistantChat
- tmaChat
  - enter assistantChat first

要 Simulate 請到 [Stately Editor](https://stately.ai/registry/editor/embed/e6c9c269-a65c-475b-8afd-09010866d350?mode=Design&machineId=a6ae1d42-68f3-4e84-b4c3-382f249904bb) 看

<img 
  class="absolute w-125 left-100 top-32"
  src='./images/chart.png'
/>

---

# Extensions

---

# Todos

1. 未來可在重要的 xxx ，嘗試
2. 與 Jotai 的[結合](https://jotai.org/docs/extensions/xstate)
