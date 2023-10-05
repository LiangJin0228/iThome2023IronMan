# [Day 29] 前後端API串接之Vue和Laravel

在使用Laravel和Vue.js創建前後端分離架構時，通常使用API來進行前後端數據交互。下面是一些步驟，使用API來串接Laravel和Vue.js：

1. 創建API路由：在Laravel中使用routes/api.php文件來定義API路由（你也可以使用web.php,看你的架構和需求使用的是什麽）。創建路由以處理從Vue.js應用程序發送的請求，例如GET、POST、PUT和DELETE請求。

```css
Route::get('blogIndex', [BlogController::class, 'index']);
Route::group(['prefix' => 'blogs', 'as' => 'blogs.', 'middleware' => 'auth'], function () {
    Route::view('', 'blogs');
    Route::post('newBlog', [BlogController::class, 'store'])->name('submitNewBlog');
    Route::get('manageBlogIndex', [BlogController::class, 'manageBlogIndex'])->name('manageBlogIndex');
    Route::put('updateBlog', [BlogController::class, 'updateBlog'])->name('updateBlog');
    Route::delete('deleteBlog', [BlogController::class, 'delete'])->name('deleteBlog');
});

```

1. 創建API控制器：在Laravel項目中創建API控制器來處理API路由中的請求。在控制器中執行相應的操作並返回JSON響應。

```xml
<?php

namespace App\Http\Controllers;

use App\Models\Tag;
use App\Models\Blog;
use Illuminate\Http\Request;
use App\Http\Requests\BlogRequest;
use Illuminate\Support\Facades\Auth;

class BlogController extends Controller
{
    public function index()
    {
        $user = Auth::user();
        $blogs = Blog::with(['user', 'comments.user'])
            ->where(function ($query) use ($user) {
// Rule 1: 獲取 public
                $query->where('privacy', 'public');
// Rule 2: 獲取 protectedif ($user) {
                    $query->orWhere(function ($query) use ($user) {
                        $query->where('privacy', 'protected');
                    });
                }
// Rule 3: 獲取 privateif ($user) {
                    $query->orWhere(function ($query) use ($user) {
                        $query->where('privacy', 'private')
                            ->where('user_id', $user->id);
                    });
                }
            })
            ->get();
        return ['blogs' => $blogs];
    }

    public function store(BlogRequest $request)
    {
        $user = Auth::user();
        $blog = $user->blogs()->create($request->only(['title', 'content', 'privacy']));

        $content = $blog->title . ' ' . $blog->content;
        $pattern = '/#(\p{L}+)/u';// 正則表達式,使用Unicode（不然會讀不到中文），使用 \p{L} 來匹配任何Unicode字母字符，/u表示啟用Unicode模式
        preg_match_all($pattern, $content, $matches);
// 匹配的hashtags// 它的資料結構大概是長這個樣子：{ {[0]=>#abc, [1]=>#123}，{[0]=>abc, [1]=>123} },我們只要他的'#'後面的字串就好，所以是$matches[1]
        $hashtags = array_unique($matches[1]);
        foreach ($hashtags as $title) {
            $tag = Tag::firstOrCreate(['title' => $title]);
            $blog->tags()->syncWithoutDetaching($tag->id);
        }
        $blog->load('user');
        return ['success' => '新增完成!', 'blog' => $blog];
    }

    public function manageBlogIndex(Request $request)
    {
        $user = Auth::user();
        $blogs = $user->blogs()->with(['user'])->get();
        return ['blogs' => $blogs];
    }

    public function updateBlog(BlogRequest $request)
    {
        $blog = Blog::findOrFail(request('id'));
        $blog->fill($request->only(['title', 'content', 'privacy']));
        $blog->save();

        $content = $blog->title . ' ' . $blog->content;
        $pattern = '/#(\p{L}+)/u';
        preg_match_all($pattern, $content, $matches);

        $newTags = array_unique($matches[1]);
        $newTagIDs = [];
        foreach ($newTags as $title) {
            $tag = Tag::firstOrCreate(['title' => $title]);
            $newTagIDs[] = $tag->id;
        }
        $blog->tags()->sync($newTagIDs);

        return ['success' => '更新完成!'];
    }

    public function delete(Request $request)
    {
        $blog = Blog::findOrFail(request('id'));
        $blog->tags()->detach();
        $blog->delete();
        return ['success' => '文章已刪除!'];
    }
}

```

1. 前端發送請求：在Vue.js應用程序中，使用Axios或其他HTTP客戶端庫來發送請求到Laravel API路由。

```kotlin
mounted() {
    axios.get('blogIndex')
        .then(response => {
            this.blogs = response.data.blogs;
        });
    axios.get('isLogin')
        .then(response => {
            this.userExist = response.data;
        });
},

```

像上面的例子就是在組件挂載上去的時候取得資料，或是可以像下面的例子去發送儲存或新建資料的請求：

```kotlin
saveItem() {
    axios.post("/blogs/newBlog", {
        title: this.title,
        content: this.content,
        privacy: this.privacy,
    })
        .then((response) => {
            this.message = response.data.success;
            this.item = response.data.blog;
        })
        .catch((error) => {
            if (error.response) {
                this.message = error.response.data.message;
            }
        });
},

```

```kotlin
saveItem() {
    axios.put(this.updateAPI, this.selectedItem).then((response) => {
        this.message = response.data.success;
        this.updateItem();
    });
},

```

這只是 Laravel 和 Vue.js 前後端分離架構的基本概述。根據你的項目需求，你可以擴展此基礎架構並添加更多功能，例如表單驗證、錯誤處理和安全性措施。

好了，今天就到這裏。

本篇終。