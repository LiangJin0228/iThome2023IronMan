# [Day 2] 3種方法快速建立 Vue/Vuetify 專案，手把手帶你進入Vue的世界

# **目前建立Vue專案的幾種主流方式**

# **方法1. 新建Vuetify(Vue3)專案(本系列采用方式)**

如果是一開始就知道自己會使用Vuetify作爲UI框架，那麽最推薦新手或是不想這麽麻煩的人直接根據官網 `npm create vuetify`的方式來進行安裝。

### **流程：**

在你想要開始專案的任何地方或文件夾的任意空白處點擊滑鼠右鍵，打開terminal：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542twSsyfRXd5.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542twSsyfRXd5.png)

然後在上面輸入`npm create vuetify`，（若是跳出提示問你是否安裝XX東西，輸入'y'選擇安裝）會跳出對話框問你專案名稱：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542ml6OeBdRB5.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542ml6OeBdRB5.png)

直接在`vuetify-project`的位置輸入你想要的專案名稱，`不要`使用中文。

之後就會問你要選擇什麽組合（上下箭頭調整，enter選擇）：如果你不知道自己會不會用到pinia，或是準備使用vuex，或是你是新手什麽都不懂，那就選擇Base(Vuetify,Vuerouter)，不然有要使用pinia的可以直接選Essentials組合：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542HAgHG6C11i.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542HAgHG6C11i.png)

再來問你一些有的沒有的問題，如果看得懂就自己選擇，看不懂的話可以依照我的配置來進行安裝：

![https://ithelp.ithome.com.tw/upload/images/20230906/201625426RL2RHDtrZ.png](https://ithelp.ithome.com.tw/upload/images/20230906/201625426RL2RHDtrZ.png)

到這裏，你就算是把一個專案新建出來了，你可以直接在剛剛terminal的裏面輸入 `cd [你的project名稱]`，然後執行`npm run dev`（若是跳出提示問你是否安裝XX東西，輸入'y'選擇安裝），他會跳出一個build成功的提示還有一個網址，按住鍵盤的`ctrl`鍵點擊那個網址，就會打開你的默認瀏覽器進入你的專案頁面：

![https://ithelp.ithome.com.tw/upload/images/20230906/201625428NL0BBsCRy.png](https://ithelp.ithome.com.tw/upload/images/20230906/201625428NL0BBsCRy.png)

進入這個頁面就算成功：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542EtXfcsR4Mn.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542EtXfcsR4Mn.png)

若是要停止你的後臺server一直運行，可以在剛剛的terminal界面按`q`：

![https://ithelp.ithome.com.tw/upload/images/20230906/201625424Z2fclfuPV.png](https://ithelp.ithome.com.tw/upload/images/20230906/201625424Z2fclfuPV.png)

到這裏爲止，整個專案就算是創建成功！

# **方法2. 新建Vue3專案**

如果沒有使用到Vuetify，可以使用Vue3官網的教程來安裝：

### **流程：**

在你想要開始專案的任何地方或文件夾的任意空白處點擊滑鼠右鍵，打開terminal：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542twSsyfRXd5.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542twSsyfRXd5.png)

然後在上面輸入`npm create vue@latest`，（若是跳出提示問你是否安裝XX東西，輸入'y'選擇安裝）會跳出對話框問你專案名稱、包名稱、一堆有的沒的……如果看不懂那些是什麽，可以照著我的選擇來進行配置：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542tNDgyzeANH.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542tNDgyzeANH.png)

創建完以後直接在terminal輸入`cd [你的project名稱]`，然後運行`npm install`

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542BBl4er1Amk.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542BBl4er1Amk.png)

運行結束以後，可以輸入`npm run dev`（若是跳出提示問你是否安裝XX東西，輸入'y'選擇安裝），他會跳出一個build成功的提示還有一個網址，按住鍵盤的`ctrl`鍵點擊那個網址，就會打開你的默認瀏覽器進入你的專案頁面：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542bHGy5AXB1O.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542bHGy5AXB1O.png)

進入到這個頁面就算成功：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542JD3DVNNqSt.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542JD3DVNNqSt.png)

若是要停止你的後臺server一直運行，可以在剛剛的terminal界面按`q`：

![https://ithelp.ithome.com.tw/upload/images/20230906/201625424Z2fclfuPV.png](https://ithelp.ithome.com.tw/upload/images/20230906/201625424Z2fclfuPV.png)

到這裏爲止，整個專案就算是創建成功！

# **方法3. 使用Vite建立**

這個也是很多人使用的方式：

### **流程：**

在你想要開始專案的任何地方或文件夾的任意空白處點擊滑鼠右鍵，打開terminal：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542twSsyfRXd5.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542twSsyfRXd5.png)

一樣在terminal輸入`npm create vite@latest`,如果有需要安裝package的話就輸入'y'：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542ODEaeHYKJj.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542ODEaeHYKJj.png)

除了project name 和 package name，其他的都可以參考我的配置：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542wQLmPD069b.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542wQLmPD069b.png)

創建完以後直接在terminal輸入`cd [你的project名稱]`，然後運行`npm install`：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542kNvuRSrJ84.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542kNvuRSrJ84.png)

運行結束以後，可以輸入`npm run dev`（若是跳出提示問你是否安裝XX東西，輸入'y'選擇安裝），他會跳出一個build成功的提示還有一個網址，按住鍵盤的`ctrl`鍵點擊那個網址，就會打開你的默認瀏覽器進入你的專案頁面：

![https://ithelp.ithome.com.tw/upload/images/20230906/20162542xqHs35vBa5.png](https://ithelp.ithome.com.tw/upload/images/20230906/20162542xqHs35vBa5.png)

進入到這個頁面就算成功：

![https://ithelp.ithome.com.tw/upload/images/20230906/201625422K9oLLi6NN.png](https://ithelp.ithome.com.tw/upload/images/20230906/201625422K9oLLi6NN.png)

# **爲什麽不使用vue cli**

應該有不少人在上網找資料的時候看到使用vue cli來安裝，目前應該還是有很多人包括很多教程使用的都是vue cli，但是依照目前的趨勢來看，vite會逐漸取代vue cli成爲主流（甚至我覺得目前主流已經是使用vite了）

# **爲什麽要直接安裝Vuetify？我不能先安裝Vue3，再使用npm install vuetify的方式安裝嗎？**

不是不行，但是目前我這系列文章希望從基礎開始慢慢的加深難度，如果一開始就因爲環境設置的問題而讓人卻步，我覺得沒有起到一個好的開頭的作用（萬事起頭難嘛，我覺得我將各位引入門，帶你們上手，剩下的你們就可以自己去看官方文檔説明，因爲雖然官方的document很完整，但是如果沒有經驗或完全沒有頭緒的人看了還是不知道要幹嘛）

### **如果是要將vuetify加入已有的專案，可以去看官方説明：**

官方資料([https://vuetifyjs.com/en/getting-started/installation/#manual-steps)](https://vuetifyjs.com/en/getting-started/installation/#manual-steps))

如果是使用`npm`,那就將官網的`yarn add vuetify@^3.3.15`命令換成`npm install vuetify`，并且在`src`的目錄底下創建一個`plugins`文件夾，在裏面創建一個`vuetify.js`文件，將以下代碼放進去：

```jsx
import { createApp } from 'vue'
import App from './App.vue'

// Vuetifyimport 'vuetify/styles'
import { createVuetify } from 'vuetify'
import * as components from 'vuetify/components'
import * as directives from 'vuetify/directives'

const vuetify = createVuetify({
  components,
  directives,
})

createApp(App).use(vuetify).mount('#app')

```

然後，記得去修改你的main.js：

```livescript
import vuetify from './plugins/vuetify.js'

.
.
.
app.use(vuetify)

app.mount(#app)
```

再到`vite.config.js`加入：

```css
vuetify({autoImport: true})

```

具體步驟還要再看它的log哪裏報錯再去除錯，然後你還要另外安裝scss，vuetify icon……

我自己是覺得十分麻煩，只是練習的話我們先一步一步慢慢來……

作爲這個系列的開頭文章，我希望從最簡單的開始一步一步帶著大家，但是到了後面的步驟，我希望各位可以養成自己去看官方文檔的習慣，我不會每個部分都説的很詳細，但是我會跟你説哪些東西的哪些用法可以在官網的文檔的哪一部分找到，畢竟，授人以魚不如授人以漁。

本篇終。