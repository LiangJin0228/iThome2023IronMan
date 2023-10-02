# [Day 26] 手把手教你將專案部署在Github Page上面

今天要來教大家如何將專案部署在github上面，github的部署的方式之一，就是創建一個`gh-pages`的branch。

我們現在從一開始手把手教大家。

# **步驟一：登入你的Github賬號**

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542tnha9y3OEc.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542tnha9y3OEc.png)

登入后：

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542uVWgh1P0t5.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542uVWgh1P0t5.png)

# **步驟二：新建一個Repository**

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542KEenQA4K4A.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542KEenQA4K4A.png)

# **（取名一）這邊的倉庫名稱可以用你的賬號+github.io（像是我自己的就是liangjin0228.github.io）**

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542RkTG5yBqeD.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542RkTG5yBqeD.png)

這樣的取名方式是表示這個是你的個人網站，github也會自動偵測到這是一個個人網站。

# **（取名二）或使用另一種正常的取名方式**

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542WrKdcAHkDK.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542WrKdcAHkDK.png)

我的倉庫已經事先將東西上傳上去了，所以裏面會有東西，但如果你是新建的倉庫裏面理應是空的。

# **步驟三：將你的文件上傳。**

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542skdzIcEIIe.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542skdzIcEIIe.png)

根據指示，在你的專案的文件夾使用git指令（你需要事先裝好git）git init的方式在你的專案初始化。

然後依照順序輸入：

git commit -m "first commit"

git branch -M main

git remote add origin [https://github.com/你自己的網址](https://github.com/%E4%BD%A0%E8%87%AA%E5%B7%B1%E7%9A%84%E7%B6%B2%E5%9D%80)

git push -u origin main

這樣你就成功將專案上傳到github的遠端倉庫了！

# **步驟四：一些設置……**

你需要在你的router裏面確認：

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542Fzdt113ksn.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542Fzdt113ksn.png)

你要確認的是你的router的實體是不是使用`createWebHashHistory`而不是`createWebHistory`。

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542igyZgl3iUY.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542igyZgl3iUY.png)

然後如果你的Repository的取名方式不是用`你的賬號+github.io`的方式去取名的話，你需要做出以下設置：

在你的vite.config.js裏面去設置`base: "/你的Repository名稱",`，象是我的專案就是：`base: "/PracticeVuetify-pet",`

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542FJujGnQMKs.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542FJujGnQMKs.png)

# **步驟五：將你的專案run build**

在你的專案的根目錄底下依次輸入：

npm i

npm run build

![https://ithelp.ithome.com.tw/upload/images/20231001/201625425EKKUKjLXy.png](https://ithelp.ithome.com.tw/upload/images/20231001/201625425EKKUKjLXy.png)

它建立完之後會多出一個dist的文件夾

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542TcSCzXggS0.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542TcSCzXggS0.png)

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542FeUwsX3OIu.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542FeUwsX3OIu.png)

# **步驟六：上傳Build好的文件夾（dist）到github上面**

依次輸入：

git add dist -f

git commit -m "updates"

git subtree push --prefix dist origin gh-pages

去到你的github專案,切換到`gh-pages`的branch查看deploy的進度如何：

![https://ithelp.ithome.com.tw/upload/images/20231001/20162542IsxKGLZZqA.png](https://ithelp.ithome.com.tw/upload/images/20231001/20162542IsxKGLZZqA.png)

# **步驟七：給那些deploy失敗的人看**

有時候會失敗是因爲vite建制的一些文件夾的名稱違反了github pages的名稱的原則，這時候我們可以再在vite.config.js裏面加上一些rules：

```tsx
// Pluginsimport vue from '@vitejs/plugin-vue'
import vuetify, { transformAssetUrls } from 'vite-plugin-vuetify'

// Utilitiesimport { defineConfig } from 'vite'
import { fileURLToPath, URL } from 'node:url'

// eslint-disable-next-line no-control-regexconst INVALID_CHAR_REGEX = /[\u0000-\u001F"#$&*+,:;<=>?[\]^`{|}\u007F]/g;
const DRIVE_LETTER_REGEX = /^[a-z]:/i;

// https://vitejs.dev/config/export default defineConfig({
  publicPath: '/',
  plugins: [
    vue({
      template: { transformAssetUrls }
    }),
// https://github.com/vuetifyjs/vuetify-loader/tree/next/packages/vite-plugin
    vuetify({
      autoImport: true,
      styles: {
        configFile: 'src/styles/settings.scss',
      },
    }),
  ],
  define: { 'process.env': {} },
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    },
    extensions: [
      '.js',
      '.json',
      '.jsx',
      '.mjs',
      '.ts',
      '.tsx',
      '.vue',
    ],
  },
  server: {
    port: 3000,
  },
  build: {
    rollupOptions: {
      output: {
// https://github.com/rollup/rollup/blob/master/src/utils/sanitizeFileName.ts
        sanitizeFileName(name) {
          const match = DRIVE_LETTER_REGEX.exec(name);
          const driveLetter = match ? match[0] : "";
// A `:` is only allowed as part of a windows drive letter (ex: C:\foo)// Otherwise, avoid them because they can refer to NTFS alternate data streams.return (
            driveLetter +
            name.slice(driveLetter.length).replace(INVALID_CHAR_REGEX, "")
          );
        },
      },
    },
  },
})

```

不要整個抄上去，只需要截取：

```livescript
const INVALID_CHAR_REGEX = /[\u0000-\u001F"#$&*+,:;<=>?[\]^`{|}\u007F]/g;
const DRIVE_LETTER_REGEX = /^[a-z]:/i;

```

和

```
build: {
    rollupOptions: {
      output: {
        // https://github.com/rollup/rollup/blob/master/src/utils/sanitizeFileName.ts
        sanitizeFileName(name) {
          const match = DRIVE_LETTER_REGEX.exec(name);
          const driveLetter = match ? match[0] : "";
          // A `:` is only allowed as part of a windows drive letter (ex: C:\foo)
          // Otherwise, avoid them because they can refer to NTFS alternate data streams.
          return (
            driveLetter +
            name.slice(driveLetter.length).replace(INVALID_CHAR_REGEX, "")
          );
        },
      },
    },
},

```

這兩個部分就好。

今天的部分就到這邊啦~如果各位有不成功的話可以在底下留言我都可以幫忙看。

本篇終。