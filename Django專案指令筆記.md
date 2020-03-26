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
