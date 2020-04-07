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
在所連結的資料庫當中建立表格，

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



