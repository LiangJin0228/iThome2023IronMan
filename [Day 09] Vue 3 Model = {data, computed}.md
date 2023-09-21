# [Day 9] Vue 3 Model => {data, computed}

在Vue3裏面，當我們使用option api的寫作風格來開發的時候，我們會有幾個主要常用的option（選項），包括{data(),computed,method,watch,lifecycle hooks},我簡單把這些option分成兩類，一類叫`屬性`一類叫`方法`，今天我就來介紹我自己會把它們分類到屬性這一類的：data和computed（要注意！不是真的依照這樣來分類，這是我爲了讓各位更好理解自己想出來的分類方法）

---

### **data()：**

data 用於定義組件內部的數據。

你可以將數據以鍵值對的形式定義在 data 中，每個鍵代表一個數據屬性，而值則是該屬性的初始值。

這些數據屬性可以在組件的模板中使用，並且當這些數據屬性的值發生變化時，模板中相應的內容也會自動更新。

```haskell
export default {
  data() {
    return {
      message: 'Hello, Vue 3!',
      count: 0
    };
  }
}

```

我們除了基本的資料型別如string,int,bool之外，還可以用array，object(類似json)的格式來寫：

```haskell
//這裏是抄官網的範例
export default {
  data() {
    return {
      author: {
        name: 'John Doe',
        books: [
          'Vue 2 - Advanced Guide',
          'Vue 3 - Basic Guide',
          'Vue 4 - The Mystery'
        ]
      }
    }
  }
}

```

### **computed：**

computed 用於定義計算屬性。

計算屬性是依賴於其他data的值計算得到的屬性，當依賴的data發生變化時，計算屬性會自動重新計算。

與 data 相同，computed 返回的是一個值。

這對於需要根據多個data計算某些值的情況非常有用，並且可以幫助保持代碼的可讀性和可維護性。

```kotlin
export default {
  data() {
    return {
      radius: 5
    };
  },
  computed: {
    area() {
//A = π r²return Math.PI * this.radius * this.radius;
    }
  }
}

```

我們可以看到，data()後面有'()'，computed裏面的東西（area）的後面也是有'()'，他們同樣都是return一些資料回去，所以我們才可以在view上面直接使用。

你可以在view的部分將你的data和computed顯示出來

```
<template>
  <div>
    <p>{{ message }}</p>
    <p>Count: {{ count }}</p>

    //computed
    <p>Circle Area: {{ area }}</p>
  </div>
</template>

```

這邊要十分注意的是，computed不應該有任何的副作用（side effect）。這句話是什麽意思呢？就是説，我們會使用到computed的時候，理應都是想要取得某些資料的一部分（或是運算），但是又不想直接操作原有的資料，所以在寫computed的時候不要去更改data的值或是一些狀態。

最常用到的功能，就拿filter來説：

假設我今天取得了一大筆的資料，裏面記錄了1000本書籍的資料（裏面有書名，作者，出版商……）

我今天想要知道同一個作者到底有幾本書，我就可以用computed去過濾我已有的data

```groovy
export default {
  data() {
    return {
      books: [
        { title: 'Book 1', author: 'Author A' },
        { title: 'Book 2', author: 'Author B' },
        { title: 'Book 3', author: 'Author A' },
        { title: 'Book 4', author: 'Author C' },
// ... 其他書籍
      ],
      selectedAuthor: 'Author A'// 選擇的作者名字
    };
  },
  computed: {
    filteredBooks() {
      return this.books.filter(book => book.author === this.selectedAuthor);
    }
  }
}

```

這樣，我就可以直接在view上面顯示過濾出來的結果：

```
<ul>
  <li v-for="(book, index) in filteredBooks" :key="index">{{ book.title }}</li>
</ul>

```

另外computed還有一個特性，那就是它的計算結果會被緩存下來。這句話是什麽意思呢？簡單來説，當我的computed裏面依賴的data的值若是沒有更動，他就不會引發重新計算；相反，若是data的值一直在變，他就會一直去計算。

以上面的範例爲例子：

```groovy
export default {
  data() {
    return {
      books: [
        { title: 'Book 1', author: 'Author A' },
        { title: 'Book 2', author: 'Author B' },
        { title: 'Book 3', author: 'Author A' },
        { title: 'Book 4', author: 'Author C' },
      ],
      selectedAuthor: 'Author A'// 選擇的作者名字
    };
  },
  computed: {
    filteredBooks() {
      return this.books.filter(book => book.author === this.selectedAuthor);
    }
  }
}

```

如果我今天新增了一本書{ title: 'Book 5', author: 'Author D' },我下面的computed就會重新算過一次，每一次data裏面的值有任何的變動都會引發computed重新計算。

---

# **Composition API裏，data和computed的寫法**

### **data 的使用：**

在 Composition API 中，你可以使用 ref 和 reactive 函數來創建響應式的資料，而不是像在option API 中那樣使用 data 屬性。

- ref：ref 用於創建一個包裝基本資料型別（如int、string、boolean等）的響應式引用。ref 創建的variable需要通過 .value 來訪問和修改它們的值。通常用於創建單一的響應式資料。

```
import { ref } from 'vue';

const count = ref(0);// 創建一個響應式資料
console.log(count.value);// 讀取值
count.value++;// 修改值
```

- reactive：reactive 用於創建一個包裝物件的響應式引用。reactive 創建的變量可以直接訪問和修改其屬性，而不需要額外的 .value。通常用於創建包含多個屬性的響應式資料物件。

```tsx
import { reactive } from 'vue';

const person = reactive({
  name: 'John',
  age: 30
});

console.log(person.name);// 直接讀取屬性
person.age++;// 直接修改屬性
```

不過如果最近有關注到{Laravel x Vue}Conf Taiwan 2023 ( [https://laravelconf.tw/](https://laravelconf.tw/) )的各位，可以去yt看一些youtuber采訪尤雨溪（vue的創作者）的影片，就會知道說其實會比較推薦使用到ref而不是reactive，如果有感興趣的話可以自己去看官方文檔或自行查找資料。

### **computed 的使用：**

在 Composition API 中，你可以使用 computed 函數來創建計算屬性。與 data()裏的ref 不同，computed 不需要 .value，而是直接返回計算結果。

```
//這裏是抄官網的
<script setup>
import { reactive, computed } from 'vue'

const author = reactive({
  name: 'John Doe',
  books: [
    'Vue 2 - Advanced Guide',
    'Vue 3 - Basic Guide',
    'Vue 4 - The Mystery'
  ]
})

// 一個計算屬性 refconst publishedBooksMessage = computed(() => {
  return author.books.length > 0 ? 'Yes' : 'No'
})
</script>

<template>
  <p>Has published books:</p>
  <span>{{ publishedBooksMessage }}</span>
</template>

```

這樣你們可以依照你們喜歡的方式來寫option api 或是 composition api，建議新手從option api寫起，會比較容易理解。

但是我之後的會帶大家進行的專案（個人形象網站），會是以composition api的方式撰寫，這點需要注意。那麽今天就到這邊。

本篇終。