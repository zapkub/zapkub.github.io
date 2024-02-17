---
date: 2022-05-09
categories:
  - typescript
  - programming
  - typesafe
comments: true
---
### TLDR;
for someone who have a limit of time
```ts
// instead of this
(window as any).MY_GREETING_MESSAGE = 'Hi Earth!'
// do this
declare global {
    interface Window { 
         MY_GREETING_MESSAGE: string
    }
}
window.MY_GREETING_MESSAGE = 'Hi Earth!'
```
<!-- more -->

---

You guys typescripter might came across the situation that you need to put something into a global scope variable like `window`

For `javascript` you may easily put something into it without any complicated step like.

```js
// running in browser
window.MY_GREETING_MESSAGE = 'Hi Earth!'
```

but as Typescript which will do some type checking if any object attribute you are working on is really existed in the view of the static type language. you will be forbidden to do this directly.

```ts
window.MY_GREETING_MESSAGE = 'Hi Earth!'
// ^^
// Property 'MY_GREETING_MESSAGE' does not exist on type 'Window & typeof globalThis'.ts(2339)
```

## Why?
The answer is pretty straight, because the interface `Window` which declare for this global `window` variable does not have `MY_GREETING_MESSAGE` property

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ac82op96fs304x891atp.png)

### Here come the `window as any`
for most of new Typescript developer may unable to solve this issue and put this dirty workaround like explicitly cast `window` to `any` which means we will access `window` without any type safety and no type checking in the runtime!

```ts
(window as any).MY_GREETING_MESSAGE = 'Hi Earth!'
```

### Bring back the type
As `window` has been declared by the standard Ambient type from Typescript library. The better approach is to extend this type like this

```ts
declare global {
    interface Window { 
         MY_GREETING_MESSAGE: string
    }
}
window.MY_GREETING_MESSAGE = 'Hi Earth!'
```
and this will let Typescript recognize the property you want to put into the ambient `window` object

![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/a3ihxfj94q4ktf4508r9.png)
 
> noted that you need to make sure the value has been assigned before use, or you may make it as an optional property


Happy Coding!


