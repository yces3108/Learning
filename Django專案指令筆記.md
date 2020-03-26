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
改好app的model之後，一次搞定他的migration，  

    python manage.py makemigrations stores



