# django環境架設(windows)

*1.* 安裝python3之後的版本(或安裝visual studio時一起安裝)

*2.* 以admin身分開啟cmd 輸入py version 若無反應則將python路徑加到環境變數path中

*3.* 安裝python pip-->cmd指令 **[curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py]** 安裝完後 指令[python get-pip.py]

*4.* 安裝python venv虛擬環境，先創建一資料夾，並cd移動到該資料夾。輸入cmd指令[python -m venv 你的venv名稱] 
[圖片1]

*5.* 啟動venv 指令[py 你的venv名稱/Scripts/activate]

*6.* 在venv環境安裝django 指令[py -m pip install django] 會自動安裝最新版本

*7.* 創建一個django專案 指令[django-admin startproject 你的專案名稱]

*8.* 可以觀察在你的資料夾下多了專案 cd移動到你的專案內，指令[py manage.py migrate]產出db檔

*9.* 指令[py manage.py runserver] 啟動server 可以在local host觀察到頁面 ctrl+c可以關閉server

*10.* 在專案資料夾下 指令[py manage.py startapp 你要新增的app功能] 可增加app
