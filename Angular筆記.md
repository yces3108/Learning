src/app/app.component.html是放內容的  
src/app/app.component.ts是放介面的  
在app.component.ts裡面的
    export class AppComponent {
    }
放任何想呈現在app.component.html的東西  
還可以把函式寫在裡面！  
## Angular CLI 常用指令
### ng new
--style 改成CSS之外的 SASS、SCSS 或 Less  
--routing 每個頁面有自己專屬的網址
--prefix  改參數的前綴字  
### ng generate
新增元件
    ng generate <schematic> [options]
常用的幾種schematic：

    class
    component
    directive
    enum
    guard
    interface
    module
    pipe
    service
### ng serve
--open 輸入完順便幫你開  
--port 改你想在localhost:XXXX看
如果你每次都想打ng serve --port 5487 --proxy-config proxy.config.json --open
那就直接改在根目錄的package.json就好，把下面這個改掉：
    
    "scripts": {
      "ng": "ng",
      "start": "ng serve", //改這裡
      "build": "ng build",
      "test": "ng test",
      "lint": "ng lint",
      "e2e": "ng e2e"
    },
然後以後都改打這個

    npm start
### ng build
發布雲端版本
--prod 發布正式成品，檔案變很小！  
--source-map 出來會含有source map  
## Todo list
下面這個會在src/app幫忙生產資料夾  

    ng g module todo-list
下面這個會在src/app把component需要的東西都生好
還會幫忙改module.ts 

    ng g component todo-list
