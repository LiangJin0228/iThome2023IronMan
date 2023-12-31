# [Day 1] Vue 起手式——入門篇

# **引言**

# **關於Vue**

> Vue (發音為 /vjuː/，類似 view) 是一款用於構建用戶界面的 JavaScript 框架。它基於標準 HTML、CSS 和 JavaScript 構建，並提供了一套聲明式的、組件化的編程模型，幫助你高效地開發用戶界面。無論是簡單還是覆雜的界面，Vue 都可以勝任。
> 

根據官網的介紹，可以瞭解Vue是一個前端的框架，在正式開始之前，我希望第一次接觸到Vue的人可以有一點瞭解什麽是框架，爲什麽我們需要使用框架？不使用框架可以嗎？學了Vue之後工作好找嗎？什麽是聲明式？組件化又是什麽？

### **關於框架**

我們都知道前端就是由HTML作爲骨幹，CSS作爲皮膚，而JS就是一個網站的靈魂，基本上2023年你會去到的所有的網站，基本上都會用到JS。而當我們的一個網站的功能越來越多，對於用戶體驗的優化越來越好，我們的整個網站的專案會越來越大且複雜。想象一下，如果沒有一些規範，教我們如何去定義每個文件存放的路徑，每個文檔的寫法，那可能就是每一家公司都有自己的做法，當一名前端工程師跳到另外一家公司後，又要重新熟悉那一家公司的規劃。但是我們可以知道的是，不管是哪一家公司，他們做出來的東西若是功能相似，很大概率做的方法都差不多，只是一些命名規則，或是路徑的規劃會有所不同，於是框架出現了。

框架就是把一些很常用的東西，將他們定義好，并且你必須依照他的規則來配置一些文件。所以對於工程師來説，框架是一個束縛，因爲它讓我們不能想幹嘛就幹嘛，但是若是你能夠去熟悉他，他會成爲你的好夥伴，因爲你只要跟著框架“造好的路”走，你就可以實現你想要的功能，并且依照框架的指示走，你還能避免一些安全的隱患，或是一些前期沒有規劃好導致後期維護困難，或是一些莫名其妙的報錯。

所以，我們要愉快的學習Vue3 框架

# **工欲善其事，必先利其器**

### **在正式開始之前，我們需要安裝的東西有：**

1. Node.js ([https://nodejs.org/en)](https://nodejs.org/en))
2. VS Code ([https://code.visualstudio.com/)](https://code.visualstudio.com/))
3. ESLint (VS Code 插件)
4. Volar (VS Code 插件)
5. Prettier (VS Code 插件)
6. Live Server (VS Code 插件)

具體的安裝細節可以自行上網參考其他人的教學，node.js在安裝的時候可以不用選擇安裝chocolatey，vs code插件直接在安裝vs code之後在左面選擇插件的頁面搜索對應名字就好了。