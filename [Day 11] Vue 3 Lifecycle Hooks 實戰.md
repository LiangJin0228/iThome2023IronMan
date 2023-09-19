# [Day 11] Vue 3 Lifecycle Hooks 實戰

在"[Day 7] Vue 3 生命周期與基礎語法"這篇中已經向各位介紹過生命周期的觀念了，這裏補充一張官方的圖片，讓各位更加瞭解生命周期的運作過程：

![https://ithelp.ithome.com.tw/upload/images/20230916/20162542HzsEPO9fdl.png](https://ithelp.ithome.com.tw/upload/images/20230916/20162542HzsEPO9fdl.png)

以created爲例，有時候我們希望在程式應用創建的時候，或是在掛載到DOM元素的時候，就像後端請求一些資料，又或者是像是可能你在進入首頁之前，你會希望使用者點進來可以馬上呈現一些佈局，然後直到往下滑的時候再去加載一些信息（就像你滑IG滑倒最底下會再加載新的內容那樣），以下是一個簡單的範例：

```
<template>
  <div>
    <p>{{ message }}</p>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      message: '',
    };
  },
  created() {
    axios.get('https://api.example.com/data')
      .then(response => {
        this.message = response.data.message;
      })
      .catch(error => {
        console.error('資料請求錯誤：', error);
      });
  },
};
</script>

```

在這個範例我們使用created，在Vue實體創建後馬上向遠程後端發送API請求，再將資料顯示在畫面上。

或是有時候我們的一些事件可能會放置了計時器，那我們可以在unmounted的時候去進行手動取消：

```xml
<template>
  <div>
    <button @click="startCountdown">Start Countdown</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      countdownInterval: null,
    };
  },
  methods: {
    startCountdown() {
      this.countdownInterval = setInterval(() => {
// 倒數計時邏輯
      }, 1000);
    },
  },
  beforeUnmount() {
// 在組件卸載前取消事件監聽器或清理操作
    clearInterval(this.countdownInterval);
  },
};
</script>

```

然後在使用Lifecycle Hooks的時候，可能有幾個細節要注意：

- created: 已經完成-響應式數據、計算屬性、方法和偵聽器。但是因爲還沒挂載到DOM上，因此 $el 屬性還不可用。
- updated: 任意 DOM 的更新後都會調用update裏面的步驟，這些更新可能是由不同的狀態變更導致的。如果你需要在某個特定的狀態更改後訪問更新後的 DOM，要使用 nextTick() 作為替代。
- unmounted: 用來清理一些副作用

---

# **昨天的補充**

關於computed，method和watch，可能會有些人有些疑惑，這裏做成表格向大家説明：

| computed | watch |
| --- | --- |
| 簡單的邏輯運算 | 耗時的操作和遠程api加載 |
| 可以直接在HTML模板使用 | 不能在HTML模板使用 |
| 響應data數據變化 | 響應數據變化 |
| 會return東西 | 沒有return (void) |
| get set methods取得和修改data的value（不推薦） | 可以修改data的value |

| methods | watch |
| --- | --- |
| 可以在watch裏面被調用 | 不能調用 |
| 可以直接在html模板使用 | 不能直接在HTML模板使用 |
| 響應data的數據變化 | 響應data的數據變化 |
| 可以有return，也可以沒有 | 沒有return |

另外有一個我自己的筆記：

![https://ithelp.ithome.com.tw/upload/images/20230916/20162542XlTCsFnVSX.png](https://ithelp.ithome.com.tw/upload/images/20230916/20162542XlTCsFnVSX.png)

今天有點晚，就先這樣了。

本篇終。