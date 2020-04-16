創建資料夾，移動到目錄，
  
    mkdir lunch
    cd lunch
在目錄之下建立Python虛擬環境，

    python3 -m venv venv/lunch
進入該環境，

    venv\lunch\Scripts\activate.bat
這樣一來就進入環境，command prompt 前面應該會多出`(lunch)`的字樣，  
接著在該環境中安裝Django，  

    pip install django
用這個指令建立專案，  

    django-admin startproject lunch
在所連結的資料庫當中建立表格django_session，

    python manage.py migrate
建立一個app，

    python manage.py startapp stores
startapp完之後記得加入，  

    # lunch/settings/base.py

    INSTALLED_APPS = (
        'stores',
        # ...
    )
把他跑起來，就可以在http://localhost:8000看到，  

    python manage.py runserver
改好app的model之後，一次搞定他的migration，  

    python manage.py makemigrations stores
在settings設定DATABASES密碼的方法在MySQL8.0行不通了！參考這篇：  
https://stackoverflow.com/questions/50469587/django-db-utils-operationalerror-2059-authentication-plugin-caching-sha2-pas  
https://www.jianshu.com/p/0ec807f3cd26  
產生超級使用者在admin頁面使用，

    python manage.py createsuperuser

## 部署到Heroku
https://devcenter.heroku.com/articles/django-app-configuration  
https://github.com/uranusjr/django-tutorial-for-programmers/blob/1.8/24-deploy-to-heroku.md  
設完runtime.txt之後，  

    pip freeze > requirements.txt
然後再改requirements.txt、Procfile、deploy_heroku.py、wsgi.py、.gitignore，  
接著執行指令，

    git init
    git add .
    git commit -m "Make it so"
登入，
    
    heroku login
建立project，

    heroku create <project_name>
建立遠端倉庫，
    
    git remote add yces3108lunch  https://git.heroku.com/yces3108lunch.git
推上雲端，  

    git push heroku master
這篇文章說，只要你的settings改過位置，那manage.py和wsgi.py都要改一下：  
https://stackoverflow.com/questions/38752015/django-deploy-improperlyconfigured-the-secret-key-setting-must-not-be-empty

## 疑難雜症區
    django.core.urlresolvers -> django.urls
    SECRET_KEY setting must not be empty -> 不要忘記開環境啊！也可能是下面這個，manage.py的問題，
https://blog.csdn.net/qq_21583139/article/details/104485102?depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-4&utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-4
    on_delete -> on_delete=models.CASCADE

