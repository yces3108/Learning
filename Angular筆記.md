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
動作完成才算數哦
在app.module.ts將TodoListModule放進來import, imports  
然後在TodoListModule的將TodoListComponent也放去exports  
想要做什麼就在Component.ts裡面的export做  
這樣可以建立class，那個--type是用來加後綴的  

    ng generate class todo-list/todo --type model
## Service
Component 只負責處理畫面、資料綁定，而 Service 則負責像是取得資料、表單驗證等等的邏輯處理  
DI （Dependency Injection，依賴注入）是 IoC （Inversion of Control，控制反轉）的一種，用來減低程式碼之間的耦合度  
Service 的裝飾器：@Injectable  
把服務的內容餵給需要的component  
Service 會因註冊在不同的 Provider 中，而有著不一樣的實體
三種註冊法，首先註冊在自己的裝飾器  

    @Injectable({
      providedIn: 'root'
    })
第二種是在某個module  

    @NgModule({
      providers: [
        BackendService,
        Logger
      ],
      ...
    })
第三種是在某個component

    @Component({
      selector:    'app-hero-list',
      templateUrl: './hero-list.component.html',
      providers:  [ HeroService ]
    })
## Routing
Routing讓SPA也可以用很多網頁  
這樣打就對了  

    ng new HelloAngularRouting --routing
它會製造出一個AppRoutingModule  
app.module.ts也把AppRoutingModule給import了進去
可以發現app.component.html最下面多了一個

    <router-outlet></router-outlet>
然後用 CLI 建立兩個 Component：

    ng generate component home
    ng generate component about
然後在app-routing.module.ts把下面的東西import進去

    import { AboutComponent } from './about/about.component';
    import { HomeComponent } from './home/home.component';
再把裡面的routes改成這樣

    const routes: Routes = [
      {
        path: 'home',
        component: HomeComponent
      },
      {
        path: 'about',
        component: AboutComponent
      }
    ];
意思是，當有人要瀏覽 localhost:4200/home 這個網址的時候，我們想讓他看到的是 HomeComponent ；當有人要瀏覽 localhost:4200/about 這個網址的時候，我們想讓他看到的是 AboutComponent
另外，因為讓子網頁後面有前綴/#/比較好，所以把下面module改一下

    @NgModule({
      imports: [RouterModule.forRoot(routes, {
        enableTracing: true, // 在控制台自動追蹤
        useHash: true // 自動加井字號
      })],
      exports: [RouterModule]
    })
這個的意思是，就算沒有輸入，也直接給我首頁的那個component

    { 
      path: '',
      component: HomeComponent
    }
接著就可以把兩個route放進html裡了
    
    <ul>
      <li><a routerLink="/home">Home</a></li>
      <li><a routerLink="/about">About</a></li>
    </ul>
    
    



