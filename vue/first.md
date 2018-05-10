# Vue

* [참고 URL](https://kr.vuejs.org/v2/guide/installation.html)
* 구조
```
$ cd src/
.
├── App.vue
├── assets
│   └── logo.png
├── components
│   └── TodoPage.vue
├── main.js
└── router
    └── index.js
```

* components
 - 실제 vue 코드들
````
<template> - html
<script> - js 코드
<style> - css
````
* router
 - urls와 비슷

```
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
 
Vue.use(Router)
export default new Router({
  routes: [
    {
      path: '/',
      name: 'Hello',
      component: HelloWorld
    }
  ]
})
```


## Release Note
