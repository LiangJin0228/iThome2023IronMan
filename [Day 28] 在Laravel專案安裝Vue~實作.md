# [Day 28] 在Laravel專案安裝Vue~實作

今天要來教大家實作的部分，但是在開始之前，你需要：

確保你已經成功安裝 Laravel 並設置好你的開發環境，包括 PHP、Composer、Node.js 和 npm。如果尚未完成這些步驟，請先完成它們。

如果你還沒有 Laravel 專案，可以使用以下命令創建一個新的 Laravel 專案：

![https://ithelp.ithome.com.tw/upload/images/20231003/20162542D0rMhv0wQa.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162542D0rMhv0wQa.png)

```sql
composer create-project laravel/laravel MyProject "8.*"

```

- composer create-project: 這部分告訴 Composer要創建一個新的專案。
- laravel/laravel: 這是要使用的 Laravel 應用程式的模板專案。具體來說，它告訴 Composer 去下載 Laravel 的官方模板。
- MyProject: 這是你將為你的新 Laravel 專案指定的名稱。你可以將其更改為你想要的任何名稱。
- "8.*": 這是 Laravel 版本的指定。在這個命令中，"8.*" 意味著你要安裝 Laravel 8 的最新版本，'*' 表示安裝 Laravel 8 的最新次要版本和修補版本。（因爲我自己開發的經驗使用的是laravel 8，但是你用新版的laravel應該也是沒有什麽差別的）

然後在你的vscode裏面打開這個專案：

![https://ithelp.ithome.com.tw/upload/images/20231003/20162542vLxVgP3X5k.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162542vLxVgP3X5k.png)

打開terminal，依次輸入：

1. `composer require laravel/ui`
2. `php artisan ui vue`
3. `php artisan ui vue --auth`
4. `npm install`
5. `npm run dev`

我們可以看到composer.json和package.json都新增了一些東西：

![https://ithelp.ithome.com.tw/upload/images/20231003/20162542NnzgMmZXVn.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162542NnzgMmZXVn.png)

![https://ithelp.ithome.com.tw/upload/images/20231003/20162542M9TXRETE5q.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162542M9TXRETE5q.png)

執行`php artisan serve`看一下畫面：

![https://ithelp.ithome.com.tw/upload/images/20231003/20162542tOw4eAVXhz.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162542tOw4eAVXhz.png)

這時候可以看到還是一般的畫面。

接下來把welcome.blade.php裏面的東西全部換掉：

```xml
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>

<body>
    <div id="app">
        <example-component></example-component>
    </div>
    <script src="../js/app.js"></script>
</body>

</html>

```

然後我們再執行一次`npm run dev`,就會發現我們的頁面被換成`/resource/js/components/ExampleComponent.vue`了。

![https://ithelp.ithome.com.tw/upload/images/20231003/20162542uRlJDzRDQL.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162542uRlJDzRDQL.png)

![https://ithelp.ithome.com.tw/upload/images/20231003/20162542rOGFN16jOj.png](https://ithelp.ithome.com.tw/upload/images/20231003/20162542rOGFN16jOj.png)

我們如果去追溯代碼的話，可以從welcome.blade.php的`<script src="../js/app.js"></script>`去找這個js檔案：

```livescript
/**
 * First we will load all of this project's JavaScript dependencies which
 * includes Vue and other libraries. It is a great starting point when
 * building robust, powerful web applications using Vue and Laravel.
 */require('./bootstrap');

window.Vue = require('vue').default;

/**
 * The following block of code may be used to automatically register your
 * Vue components. It will recursively scan this directory for the Vue
 * components and automatically register them with their "basename".
 *
 * Eg. ./components/ExampleComponent.vue -> <example-component></example-component>
 */// const files = require.context('./', true, /\.vue$/i)
// files.keys().map(key => Vue.component(key.split('/').pop().split('.')[0], files(key).default))

Vue.component('example-component', require('./components/ExampleComponent.vue').default);

/**
 * Next, we will create a fresh Vue application instance and attach it to
 * the page. Then, you may begin adding components to this application
 * or customize the JavaScript scaffolding to fit your unique needs.
 */const app = new Vue({
    el: '#app',
});

```

本來一開始的laravel專案的app.js裏面只有一句`require('./bootstrap');`，但是現在裏面增加了幾個内容：

它添加了了一個Vue的程式碼進來`window.Vue = require('vue').default;`，并且這個Vue注冊了一個Component`Vue.component('example-component', require('./components/ExampleComponent.vue').default);`，然後再通過：

```
const app = new Vue({
    el: '#app',
});

```

去綁定div id=app。

然後我們再去觀察看webpack.mix.js文件：

```less
mix.js('resources/js/app.js', 'public/js')
    .vue()
    .sass('resources/sass/app.scss', 'public/css');

```

原本的postcss被替換掉了，改成sass，并且多了一個vue的打包。

之後如果要增加頁面，可以透過在不同的blade模板綁定不同的Vue實體，或是統一一個入口，前端的路由交由vue-router去執行，在laravel的router(web.php)裏面去設置所有不認識的路由交給前端處理，這也是一種方式。

明天會教大家完全前後端分離要怎麽處理，今天先到這邊。

本篇終。