參考自：  
https://medium.com/@samy_raps/simple-movies-web-app-with-vue-vuetify-and-django-part-1-setup-6351c02327a5  

## 前端準備
Vue — The progressive framework for building user interfaces.  
Vuetify — The material design framework for Vue  
Axios — Promise based HTTP client for the browser and node.js  
Sweet Alert 2 — For beautiful Alert messages  
Vue Session — For storing user sessions (on the front end)  
Vuex — For managing the state. For more about state management in Vue, read this awesome post.  
## 後端準備
Django (1.11 and above. I have not tested this with lower versions)  
Django Rest Framework ( Django REST framework is a powerful and flexible toolkit for building Web APIs)  
Django CORS Headers — ( A Django App that adds CORS (Cross-Origin Resource Sharing) headers to responses.)  
Django Filter — ( Django-filter is a reusable Django application allowing users to declaratively add dynamic QuerySet filtering from URL parameters.)  
Django Rest Framework JWT — ( JSON Web Token Authentication support for Django REST Framework)  
## 建立後端
1. 建立環境
先建立環境，

    python -m venv venv
再進入環境，

    venv\Scripts\activate.bat
此時指令列前面應該有環境名稱。
2. 建立資料夾

    ex. MyProject
記得cd進去。
3. 安裝Django

    pip install Django
4. 建立後端資料夾(Server)

    django-admin startproject server
5. 安裝一些套件
如果已經有requirements.txt，裡面寫好待安裝的東西的話：

    pip install -r requirements.txt
反過來說，如果已經安裝，但想建立requirements.txt文件的話：

    pip freeze > requirements.txt
6. 建立後端應用

    django-admin startapp movie
## 建立前端
1. 建立前端應用(Client)

    vue create client
    cd client
    vue add vuetify
2. 安裝一些套件

    npm install --save axios sweetalert2 vue-session vuex






https://stackoverflow.com/questions/53993265/failed-to-download-repo-vuetifyjs-webpack-response-code-404-not-found
