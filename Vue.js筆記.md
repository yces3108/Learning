## 概念
View : 透過 HTML 及模板語法渲染出來的畫面。  
View Model : 使用 Vue.js 所建立起來的實體。  
Model : 後端程式中的商業邏輯。  
## 引入
一般的專案中通常都是使用 Webpack 來引入，但也可以使用 <script> 元素來引入。

    <!DOCTYPE html>
    <html>
    <head>
        ...
        <script src="https://unpkg.com/vue"></script>
    </head>
    <body>
        ...
    </body>
    </html>
## 建立實體

    <script>
        var vm = new Vue({
            ...
        });
        ...
    </script>
## 第一個例子

    var vm = new Vue({
        el: '#app',
        data: {
            message: "This is local data.",
        },
        methods: {
            getRemoteMessage() {
                Promise.resolve("Get remote data.")
                    .then((res) => {
                        this.message = res;
                    });
            },
        },
    });
這樣可以將vue實例綁定在<code>id</code>為<code>app</code>的元素上。  
