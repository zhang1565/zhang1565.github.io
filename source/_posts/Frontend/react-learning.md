---
title: React
top_img: false
tags:
  - React
  - 前端开发
categories:
  - 前端开发 - React
abbrlink: cc1b9611
date: 2026-09-8 00:00:00
cover: /img/react.jpeg
---

---

### 1. React 和 Vue 的核心区别是什么？

**答案**：
1. **响应式原理**：Vue 基于 `Proxy` 自动追踪变化（数据可变）；React 依赖 `setState` + 不可变数据（Immutable），需手动触发更新。
2. **模板语法**：Vue 使用基于 HTML 的模板语法；React 使用 JSX（本质是 `React.createElement` 的语法糖，在 JS 中写 HTML）。
3. **数据流**：Vue 支持 `v-model` 双向绑定；React 严格单向数据流（父传子，子通过回调函数修改父数据）。
4. **生态哲学**：Vue 是“大而全”的框架（官方提供 Router/Pinia）；React 是“小而美”的核心库，路由、状态管理依赖社区方案。


### 2. `useState` 的惰性初始化是什么？为什么要用它？

**答案**：`useState` 可以接收**函数**作为参数，该函数只在**组件首次渲染**时执行一次。用于避免每次重渲染都执行昂贵计算（如读取 `localStorage`、`JSON.parse` 大对象、复杂数学运算）。

```jsx
// ❌ 每次渲染都执行（点击按钮导致重渲染，也会重新执行）
const [theme] = useState(localStorage.getItem('theme'));

// ✅ 只在首次挂载执行一次
const [theme] = useState(() => localStorage.getItem('theme'));
```

> **💡 用户曾困惑**：“惰性初始化的这个useState我还是没有理解，可以用实际项目中的实际案例来给我讲解一下吗？”
> **大白话解释**：你可以把 `useState(() => expensive())` 理解为一个“懒汉模式”——就像去餐厅点菜，服务员（React）只在第一次有人点餐时才进厨房做菜（执行昂贵计算），后续再点同样的菜就直接端上来，不用重新做。而 `useState(expensive())` 则是“不管有没有人吃，先做好再说”——即使组件因为其他原因重新渲染，也会重复做菜，浪费性能。


### 3. `useEffect` 的清理函数（Cleanup）在什么时候执行？

**答案**：
1. 组件**卸载**时。
2. **下一次 effect 执行之前**（即依赖项变化时，会先清理上一次的 effect，再执行新的 effect）。

```jsx
useEffect(() => {
  const timer = setInterval(() => {}, 1000);
  return () => {
    clearInterval(timer); // ① 卸载时 ② 依赖变化重新执行前
  };
}, [dependency]);
```


### 4. 什么是 `useEffect` 的闭包陷阱？如何解决？

**问题本质**：`useEffect` 内部函数捕获了**创建时**的 state 值，如果依赖数组为空，该值永不更新。

```jsx
// ❌ 错误：count 永远从 0 变成 1，然后卡住
useEffect(() => {
  setInterval(() => {
    setCount(count + 1); // count 永远是最初的 0
  }, 1000);
}, []);

// ✅ 解决方案1：函数式更新（推荐，不依赖外部变量）
useEffect(() => {
  setInterval(() => {
    setCount(prev => prev + 1);
  }, 1000);
}, []);

// ✅ 解决方案2：正确声明依赖
useEffect(() => {
  setInterval(() => {
    setCount(count + 1);
  }, 1000);
}, [count]);
```

> **💡 用户曾困惑**：“我没有看懂这个demo主要是闭包的useEffect内部函数捕获了创建时的state这句话，我没有理解。”
> **大白话解释**：你可以把 React 组件的每一次渲染（每一次执行函数），想象成**拍了一张“快照照片”**。在这张照片里，所有的变量（包括 state）都被定格在了那一刻。`useEffect` 里的定时器（内部函数）就像一台 1 秒前制造的“老相机”，它手里死死攥着那张旧照片（`count = 0`）。即使 1 秒后界面变成了 `1`，这台老相机看的还是旧照片，所以它永远只会 `0 + 1 = 1`，不会变成 2、3、4……这就是“捕获了创建时的 state”。


### 5. `useMemo` 和 `useCallback` 的区别及使用场景？

**答案**：
- **`useMemo`**：缓存**计算结果（值）**。依赖不变，不重新计算。适用于数组过滤、复杂数据转换。
- **`useCallback`**：缓存**函数本身（引用）**。依赖不变，函数地址不变。适用于把函数传给被 `React.memo` 包裹的子组件。

```jsx
// useMemo：缓存值
const filtered = useMemo(() => {
  return hugeData.filter(item => item.name.includes(keyword));
}, [keyword]);

// useCallback：缓存函数引用
const handleClick = useCallback(() => {
  console.log('点击', count);
}, [count]);
```

> **💡 用户曾困惑**：“我现在有点混淆了，React.memo和useMemo以及useCallback到底分别是什么？”
> **大白话解释**：
> - **`React.memo`（保安）**：守卫**整个组件**。作用是：“如果外面给我（组件）的快递（props）没变化，我就不让车间（组件函数）重新开工（重渲染）。”
> - **`useMemo`（计算员）**：守卫**一个具体的计算结果**。作用是：“只有原材料（依赖项）变了，我才重新算一遍，否则直接拿上次算好的结果（缓存值）。”
> - **`useCallback`（固定工牌）**：守卫**一个函数本身的地址**。作用是：“不管车间开工（渲染）多少次，我发给别人的这张函数工牌（引用），永远都是同一张。”


### 6. `useReducer` 是干什么的？一般在什么场景使用？

**答案**：`useReducer` 是 `useState` 的升级版，用于**状态包含多个子值且联动更新**的场景。它通过 `dispatch` 发送指令，由 `reducer` 纯函数集中处理逻辑。

**适用场景**：
1. 购物车（增删改 + 算总价）。
2. 数据请求（`{ data, loading, error }`）。
3. 复杂表单校验。

```jsx
const reducer = (state, action) => {
  switch (action.type) {
    case 'ADD': return { ...state, items: [...state.items, action.payload] };
    case 'REMOVE': return { ...state, items: state.items.filter(i => i.id !== action.payload) };
    default: return state;
  }
};
const [state, dispatch] = useReducer(reducer, { items: [] });
dispatch({ type: 'ADD', payload: { id: 1, name: '苹果' } });
```

> **💡 用户曾困惑**：“Q5的useReducer我也没有理解，它这么写的好处到底是什么？我需要实际落地项目中的实际案例，然后需要用dispatch才能调用吗？”
> **大白话解释**：
> - **好处**：把“修改状态的逻辑”从组件内部搬到外面去了。当你需要同时修改多个状态（比如加商品的同时算总价、改 loading），用 `useReducer` 可以把这些逻辑全部收拢到 `reducer` 函数里，组件里只负责 `dispatch`，代码瞬间清晰。
> - **必须用 `dispatch` 才能调用吗？** **是的，绝对必须！** `useReducer` 返回的第二个值就是 `dispatch` 函数，它是修改状态的**唯一入口**。你不能直接写 `state.items.push(newItem)`，因为 React 不允许直接修改 state。所有修改必须通过 `dispatch({ type: '...', payload: ... })` 发送指令。
> - **类比**：`useReducer` = 请了一个专业会计（reducer）帮你管账。你不能自己直接去翻账本改数字（直接修改 state），你必须口头告诉会计（`dispatch`）“我要加一笔 100 块的支出”（指令），会计收到后，算出新账本还给你。


### 7. `useRef` 和 `useState` 的核心区别是什么？如何用 `useRef` 模拟 `componentDidUpdate`？

**答案**：
- **`useState`**：改变数据会**触发**重新渲染，用于 UI 展示的数据。
- **`useRef`**：改变 `current` 属性**不会触发**重新渲染，在多次渲染间保持同一引用。用于 DOM 引用、定时器 ID、渲染标记。

**模拟 `componentDidUpdate`**：利用 `useRef` 标记“是否首次挂载”，配合 `useEffect` 区分首次和更新。

```jsx
function Demo() {
  const [count, setCount] = useState(0);
  const isFirstRender = useRef(true);

  useEffect(() => {
    if (isFirstRender.current) {
      isFirstRender.current = false;
      return; // 首次挂载跳过
    }
    console.log('更新了（相当于 componentDidUpdate）', count);
  }, [count]);
}
```

> **💡 用户曾困惑**：“这个地方其实我有点没有理解，可能是我对这个useRef的理解还不够透彻。”
> **大白话解释**：`useRef` 是一个“打不死的小强盒”——它在组件的整个生命周期里**永远只有一个，永远不会被重新创建**。修改 `isFirstRender.current = false` 时，React **不会**触发组件重新渲染。所以我们可以利用它做“首次挂载标记”：首次执行 `useEffect` 时，把盒子从 `true` 改成 `false` 并 `return` 跳过更新逻辑；后续组件重新渲染时，盒子里的 `false` 依然存在，于是完美跳过首次逻辑，只执行更新逻辑。


### 8. `React.memo`、`useMemo`、`useCallback` 三者的区别？为什么 `React.memo` 对引用类型 Props 会失效？

**答案**：
- **`React.memo`**：守卫**整个组件**，Props 不变就不重新渲染。
- **`useMemo`**：守卫**计算结果（值）**。
- **`useCallback`**：守卫**函数引用**。

**失效原因**：`React.memo` 进行的是**浅比较**。对于对象/数组/函数，比较的是**内存地址**。父组件每次渲染都重新创建引用，地址变了，`memo` 判断为“变化”，导致子组件重渲染。必须配合 `useMemo`/`useCallback` 固定地址。

```jsx
// ❌ 失效：每次渲染新建对象，地址变了
<Child user={{ name: '张三' }} />

// ✅ 有效：用 useMemo 固定地址
const user = useMemo(() => ({ name: '张三' }), []);
<Child user={user} />
```

> **💡 用户曾困惑**：“我理解你的这个意思是，如果传递的是引用对象，尽管他的值没有改变，但是因为引用对象的存放是放在堆里面，这个时候他的指针变了，所以它还是会重新渲染？意思是我用React.memo包裹了子组件也没用？”
> **大白话解释**：你的理解**完全正确**！`React.memo` 的浅比较看的确实就是**指针（内存地址）**。地址变了，它就认为变了，就会重新渲染。但**不是所有情况都失效**：
> - 传字符串 `"张三"`（基本类型）：值不变 → `memo` 有效 → 不渲染。
> - 传对象 `{ name: '张三' }`（引用类型）：每次都是新地址 → `memo` 失效 → 依然渲染。


### 9. 父组件重新渲染，子组件一定会重新渲染吗？

**答案**：
- **默认情况**：**会！** 父组件渲染，子组件无条件跟着渲染（这是 React 的默认机制）。
- **用了 `React.memo` 包裹子组件**：如果传入的 props 经过浅比较没有变化，则**不会**重新渲染。
- **注意**：子组件是否在内部使用了该 props，与 `memo` 的判断无关。只要传入的值/地址变了，就会渲染。


### 10. React 中的组件通信方式有哪些？（对比 Vue）

**答案**：
| 场景 | Vue 写法 | React 写法 |
| :--- | :--- | :--- |
| 父传子 | `props` | `props` |
| 子传父 | `$emit` | 父传**回调函数** `props.onXxx`，子组件调用 |
| 父调用子方法 | `ref`（自动暴露） | `forwardRef` + `useImperativeHandle`（手动暴露） |
| 跨层级（爷孙） | `provide` / `inject` | `createContext` + `useContext` |
| 全局状态 | Vuex / Pinia | Zustand / Redux Toolkit |

> **💡 用户曾困惑**：“一定要用forwardRef和useImperativeHandle才能让父组件拿到子组件的方法吗？不能像vue里面一样只通过ref就可以拿到吗？”
> **大白话解释**：这是 React 和 Vue 设计哲学的核心差异。Vue 的 `ref` 默认会**自动暴露**组件实例上的所有 `data`、`computed` 和 `methods`（自动挡）。React 函数组件没有“实例”这个概念，且默认**禁止**父组件通过 `ref` 直接渗透子组件的内部（防御性设计，保持封装性）。所以必须手动“挖洞”——`forwardRef` 允许子组件接收父组件的 `ref`，`useImperativeHandle` 则像守门员一样，只暴露你想让父组件调用的方法。


### 11. `forwardRef` 和 `useImperativeHandle` 是做什么的？

**答案**：
- **`forwardRef`**：允许父组件通过 `ref` 穿透到子组件内部（默认函数组件不接受 `ref`）。
- **`useImperativeHandle`**：在子组件中限制父组件通过 `ref` 能调用的方法（不暴露整个 DOM 或内部状态，保持封装性）。

```jsx
const Modal = forwardRef((props, ref) => {
  useImperativeHandle(ref, () => ({
    open: () => setIsOpen(true),
    close: () => setIsOpen(false),
  }));
  // ...
});
// 父组件：modalRef.current.open() 只能调用 open/close
```


### 12. Zustand / Redux 和 Pinia / Vuex 的核心区别是什么？

**答案**：
- **Pinia/Vuex**：基于 Vue 的 `Proxy` 响应式系统，数据**可变**（可直接修改 `state.count++`）。
- **Redux/Zustand**：基于**不可变数据**（Immutability），必须通过 `dispatch(action)` 或 `set` 函数生成新状态来替换旧状态。

| 库 | 特点 |
| :--- | :--- |
| Pinia | Vue 生态首选，响应式，可变 |
| Redux Toolkit | React 大型项目标准，模板代码多，强规范 |
| Zustand | React 中小型项目首选（2026 最流行），极简 API |


### 13. React 19 有哪些核心新特性？（2026 必考）

**答案**：
1. **React Compiler（编译器）**：自动进行 memo 优化，未来可完全抛弃手写 `useMemo`/`useCallback`。
2. **Actions**：原生支持异步函数，结合 `useTransition` 自动管理 `pending` 加载状态。
3. **`use` API**：在组件中直接 `await` 异步数据源（无需 `useEffect`）。
4. **Server Components（RSC）稳定**：组件默认在服务端运行，减少客户端 JavaScript 体积。


### 14. 受控组件和非受控组件的区别？各自适用什么场景？

**答案**：
- **受控组件**：表单数据由 **React state** 管理。每次输入触发 `onChange` 更新 state，state 再驱动视图。**优点**：可实时校验、格式化、联动禁用按钮。**适用 95% 场景**。
- **非受控组件**：表单数据由 **DOM 自身**管理。需要取值时通过 `ref` 去 DOM 里拿。**优点**：无需实时渲染，省性能。**适用场景**：`<input type="file">`（文件上传）及极少数性能要求极高的巨型表单。

```jsx
// 受控
<input value={value} onChange={e => setValue(e.target.value)} />
// 非受控
<input ref={inputRef} defaultValue="张三" />
```

> **💡 用户曾困惑**：“受控组件和非受控组件到底是个什么意思？其实我不是很能理解，真正的实际开发中会有比较多的这个吗？表单？”
> **大白话解释**：**数据归谁管，就是谁控。**
> - **受控组件**：数据归 **React 的 state** 管（Vue 里的 `v-model` 就是典型的受控）。用户输入 → 触发 `onChange` → 更新 state → state 驱动 input 显示新值。**优点**：你可以随时校验、格式化、联动禁用提交按钮。
> - **非受控组件**：数据归 **DOM 节点自己** 管（原生 `input` 的 `value` 默认自己存着）。你需要的时候，用 `ref` 去 DOM 里把值“捞”出来。**优点**：不需要实时渲染，省性能。
> - **实际开发中多吗？** **极多！** 几乎所有的业务表单都是受控的。只有 `<input type="file" />`（文件上传，受控无法赋值）等极少数情况用非受控。


### 15. `useLayoutEffect` 和 `useEffect` 的区别？

**答案**：
- **`useEffect`**：在浏览器**绘制（Paint）后**异步执行，不阻塞视觉更新。适用于绝大多数副作用（数据请求、定时器、日志）。
- **`useLayoutEffect`**：在浏览器**绘制前**同步执行，会阻塞渲染。适用于需要**测量 DOM 尺寸**或**同步修改 DOM 防止闪烁**的场景（如获取元素宽高后立即调整样式）。


### 16. 为什么列表循环渲染时，`key` 不能使用数组索引 `index`？

**答案**：当列表发生**增删或排序**时，元素的 `index` 会变化。React 使用 `key` 来识别哪些元素改变了（复用 DOM）。如果 `key` 是变化的索引，React 会错误地复用 DOM，导致**组件状态错乱**（如复选框勾选跑偏、输入框内容错位）。必须使用**唯一且稳定**的 `id` 作为 `key`。


### 17. 什么是 React Portal？有什么用？

**答案**：`ReactDOM.createPortal(child, container)` 允许将子节点渲染到**父组件 DOM 结构之外**的 DOM 节点中。适用于**模态框、全局提示、下拉菜单**。核心好处是防止被父容器的 `overflow: hidden` 或 `z-index` 限制，确保浮层能覆盖全屏。


### 18. 什么是错误边界（Error Boundary）？如何捕获错误？

**答案**：错误边界是 React 中的**类组件**，使用 `static getDerivedStateFromError` 和 `componentDidCatch` 捕获**子组件树**渲染时的 JS 错误，显示备用 UI，防止整个应用白屏。

**注意**：函数组件不能直接写错误边界，需使用第三方库 `react-error-boundary`。错误边界不捕获事件处理器和异步代码中的错误（这些需要用 `try-catch`）。


### 19. `useTransition` 和 `useDeferredValue` 是干什么的？

**答案**：它们是 React 18 引入的**并发特性**，用于优化用户体验。
- **`useTransition`**：将状态更新标记为**“非紧急”**。让 React 优先处理用户输入等紧急任务，非紧急更新可被中断。适用于**大列表搜索过滤**。
- **`useDeferredValue`**：延迟更新某个值。输入时旧值先显示，新值在后台计算完再替换（类似防抖 + 去重）。

```jsx
const [isPending, startTransition] = useTransition();
startTransition(() => {
  setResult(hugeList.filter(item => item.includes(keyword))); // 不阻塞键盘输入
});
```