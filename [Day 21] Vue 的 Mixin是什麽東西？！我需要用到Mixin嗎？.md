# [Day 21] Vue 的 Mixin是什麽東西？！我需要用到Mixin嗎？

還記得我們昨天做了瀏覽商品的頁面和功能嗎？對，我們今天沒有要繼續。

不知不覺已經21天了，我們接下來的進度會是繼續完成我們的專案。然後，我們會嘗試著將它掛載到[github page](https://pages.github.com/)上面,並且我們會爲我們的專案簡單加個後端，教大家怎麼樣前後端去做連接。

在這之前，要把一些有的沒的的概念教會大家，其中一個就是 **狀態管理**。

Vue 3 的狀態管理是一種用於管理應用程序狀態的方法（？講廢話）。它允許在不同組件之間共享和管理數據，以便在應用程序中保持一致的狀態。在 Vue 3 中，有幾種插件來實現狀態管理，其中兩個主要的library是 Pinia 和 Vuex。

### **Pinia:**

Pinia 使用了 Vue 3 的 Composition API 來管理狀態。它提供了一種類型安全的狀態管理方式，允許定義強類型的狀態、操作和 getter。它支持模塊化和插件系統，使得狀態管理變得更加靈活。Pinia 的優勢在於它的性能和 TypeScript 支持。

### **Vuex:**

Vuex 是 Vue 2 中最常用的狀態管理庫，但也可以在 Vue 3 中使用。

Vuex 提供了一個集中式的存儲來管理應用程序的狀態，包括狀態、操作、getter 和 mutation。

Vuex 的使用相對簡單，適用於小到中型的應用程序。對於大型應用程序，Vuex 可能需要更多的設置和維護。

### **推薦使用哪個庫取決於項目需求和偏好：**

如果你正在使用 Vue 3 並且希望充分利用 Composition API 和 TypeScript，那麼 Pinia 是一個強大的選擇。

如果你正在遷移一個已經使用 Vuex 的 Vue 2 項目到 Vue 3，那麼繼續使用 Vuex 可能更容易。

對於小型項目，也可以考慮使用 Vue 3 的內置響應式系統，而不引入額外的狀態管理庫。

### **那麼我們今天要説的的Mixin是什麼呢？**

Mixin 是 Vue 中的一種特性，允許將一些組件選項混合到多個組件中，這可以用來共享代碼和功能。

但是，Mixin 有一些問題，例如命名沖突和難以追蹤數據的來源。因此，在 Vue 3 中，官方更推薦使用 Composition API 來代替 Mixin。

Mixin 實際上是一種將可覆用的組件的選項（option）混合（或稱合並）到其他 Vue 組件中的技術。Mixin 允許在多個組件之間共享和重用功能、數據、方法和生命周期鉤子。

不過在使用Mixin的時候，有幾項需要注意的點：

1. mixin和component的data，computed，method……之類的選項（option）若是其中的命名有衝突，會以component爲主。eg:

```haskell
//Mixin
data（）{
    return {
    text： "Hello World"
    }
}

```

```kotlin
//ComponentAdata（）{
    return {
    text： "ComponentA"
    }
}

```

```arduino
//App.vue
<template>
    {{ text }}
//這邊會顯示ComponentA，因爲Component的優先級大於Mixin
</template>

```

1. 若是生命周期的hook，那會先執行Mixin，才執行Component

---

要創建一個 Mixin，你可以簡單地定義一個普通的 JavaScript 物件，該物件包含你希望混合到組件中的選項。例如：

```jsx
// myMixin.jsexport const myMixin = {
  data() {
    return {
      count: 0
    };
  },
  methods: {
    increment() {
      this.count++;
    }
  }
};

```

然後我們在局部注冊我們的Mixin到Component裏面：

```html
// MyComponent.vue
<template>
  <div>
    <button @click="increment">Increment</button>
    <p>Count: {{ count }}</p>
  </div>
</template>

<script>
import { myMixin } from './myMixin';

export default {
  mixins: [myMixin],
};
</script>

```

可以看到我們沒有在Component裏面寫入increment的method，也沒有count的data，但是可以直接在MyComponent裏面去做使用。

另外，注意到我説的是局部注冊，代表這個Mixin只有在被注入的組件通用，我們其實還有另一種全局注冊的方式。

創建一個 Mixin 文件，就像之前的示例一樣：

```tsx
// myGlobalMixin.jsexport const myGlobalMixin = {
  data() {
    return {
      globalData: "This is global data"
    };
  },
  methods: {
    globalMethod() {
      console.log("This is a global method");
    }
  }
};

```

在你的 Vue 應用程序的入口文件（通常是 main.js）中使用 app.mixin 方法來全局註冊 Mixin。例如：

```tsx
import { createApp } from 'vue';
import App from './App.vue';
import { myGlobalMixin } from './myGlobalMixin';

const app = createApp(App);

// 全局註冊 Mixin
app.mixin(myGlobalMixin);

app.mount('#app');

```

然後你可以在組件中像使用局部 Mixin 一樣使用全局 Mixin。例如：

```tsx
// 在組件中使用全局 Mixinexport default {
  mounted() {
    console.log(this.globalData);// 訪問全局數據this.globalMethod();// 調用全局方法
  },
// 組件的其他選項
};

```

那爲什麽我們突然要學這個東西呢？因爲……我們在之後會用到……吧。大家敬請期待。不過其實對於Vue3來説，Pinia和Vuex才是主流，現在應該不流行，甚至有人會覺得不應該使用Mixin，因爲它使我們的代碼變得難以追蹤，而且專案越變越大的時候會變得很混亂，以我自己來説我也是支持不使用Mixin那一派的。但是我覺得任何的事情也沒有絕對，主要還是看專案需求，我們要滿足當前的需求而不是過度設計，若是適合使用Mixin那稍微用下也無妨。

本篇終。