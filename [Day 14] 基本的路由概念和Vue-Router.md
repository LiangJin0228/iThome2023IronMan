# [Day 14] 基本的路由概念和Vue-Router

在前端和後端開發中，常見的路由器（router）是指用於管理和處理網絡請求的工具或組件。這些路由器有不同的用途和實現方式，但它們的主要目標都是將請求路由（route）到正確的處理程序或頁面。

### **前端路由器：**

在前端開發中，前端路由器通常用於創建單頁應用程序（SPA）。SPA 是一種在加載頁面時不會重新加載整個頁面的應用程序，而是通過 JavaScript 動態地更新頁面內容。前端路由器負責管理這些頁面切換和 URL 的路由。常見的前端路由器包括：

- React Router： 用於React應用程序的常用路由器。
- Vue Router： 用於Vue.js應用程序的官方路由器。
- Angular Router： 用於Angular應用程序的官方路由器。

這些前端路由器允許開發人員定義路由規則，以便在用戶導航或訪問不同 URL 時加載不同的組件或視圖。

### **後端路由器：**

在後端開發中，後端路由器用於將不同的 HTTP 請求（如 GET、POST、PUT、DELETE 等）路由到相應的處理程序或控制器。這有助於組織和管理應用程序的路由邏輯。常見的後端路由器包括：

- Express.js Router： 用於Node.js的Express框架中的路由器。
- Django URLconf： 用於Django Web框架的URL配置。
- Spring Boot Router： 用於Java Spring Boot應用程序的路由器。

後端路由器允許開發人員定義不同的路由端點，以便根據請求的路徑和方法執行相應的操作。

總之，前端和後端路由器在不同的應用層次上執行路由功能，前者負責客戶端的路由和導航，而後者負責服務器端的路由和請求處理。這些路由器都是構建現代Web應用程序的重要組件。

在常見的前後端分離的架構中，我們可以從相對比較簡單的RESTful API架構開始説起：

**RESTful API 架構：**

這是一種常見的前後端分離架構，其中前端和後端通過 RESTful API 進行通信。前端是一個獨立的客戶端應用程序，通常使用 JavaScript 框架（如 React、Vue.js、Angular）構建，它通過 HTTP 請求與後端服務器進行通信。後端服務器提供 RESTful API，以處理請求並提供數據和服務。這種架構使得前端和後端可以彼此獨立開發，並且允許多個前端應用程序共享相同的後端服務。

是不是聽的糊里糊塗？沒關係，我們可以直接看Code：

這裏是在router.js裏面：

```tsx
import { createRouter, createWebHashHistory } from "vue-router";

const routes = [
  {
    path: "/",
    redirect: "/index",
  },
  {
    path: "/index",
    component: () => import("@/layouts/sandwich/SandwichLayout.vue"),
    children: [
      {
        path: "",
        name: "Home",
// route level code-splitting// this generates a separate chunk (about.[hash].js) for this route// which is lazy-loaded when the route is visited.
        component: () =>
          import(/* webpackChunkName: "home" */ "@/views/Home.vue"),
      },
    ],
  },
  {
    path: "/resume",
    component: () => import("@/layouts/sandwich/SandwichLayout.vue"),
    children: [
      {
        path: "",
        name: "Resume",
        component: () =>
          import(/* webpackChunkName: "resume" */ "@/views/Resume.vue"),
      },
    ],
  },
  {
    path: "/about",
    component: () => import("@/layouts/sandwich/SandwichLayout.vue"),
    children: [
      {
        path: "",
        name: "About",

        component: () =>
          import(/* webpackChunkName: "about" */ "@/views/About.vue"),
      },
    ],
  },
  {
    path: "/contact",
    component: () => import("@/layouts/sandwich/SandwichLayout.vue"),
    children: [
      {
        path: "",
        name: "Contact",
        component: () =>
          import(/* webpackChunkName: "contact" */ "@/views/Contact.vue"),
      },
    ],
  },
];

const router = createRouter({
  history: createWebHashHistory(process.env.BASE_URL),
  routes,
});

export default router;

```

我們先通過`import { createRouter, createWebHashHistory } from "vue-router";`將兩個東西import進來，createRouter應該好理解，就是create一個Router本身，而createWebHashHistory這邊就有點東西要注意：

1. 我們有兩種方法，一種是這個範例上面的`createWebHashHistory`，另外一種是`createWebHistory`
2. createWebHashHistory是在沒有後端的情況下才會使用的，它會在我們的網址後面加上一個hash#（eg. www.example.com/#/index ）
3. 如果要使用createWebHistory，要另外在服務器上面做其他的設定（ [https://router.vuejs.org/guide/essentials/history-mode.html#Example-Server-Configurations](https://router.vuejs.org/guide/essentials/history-mode.html#Example-Server-Configurations) ）

當我們創建出一個router之後，我們再在main.js去注冊：`app.use(router)`

這樣我們就可以在組件内使用`<RouterView></RouterView>` 和 `<RouterLink to=""></RouterLink>`的方式去切換頁面了~是不是很簡單。

本篇終。