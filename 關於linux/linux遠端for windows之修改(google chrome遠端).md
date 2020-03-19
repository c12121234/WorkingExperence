# linux遠端for windows之修改(google chrome遠端)

#### 概略

主要是直接使用時，linux會新開一session(和windows不同)

要修改同一畫面的話，要再修改script檔


#### 內容

停止 Chrome Remote Desktop:

`/opt/google/chrome-remote-desktop/chrome-remote-desktop --stop`

Backup the original configuration:

`sudo cp /opt/google/chrome-remote-desktop/chrome-remote-desktop /opt/google/chrome-remote-desktop/chrome-remote-desktop.orig`

Edit the config file (sudo vim, gksudo gedit, etc):

`sudo vim /opt/google/chrome-remote-desktop/chrome-remote-desktop`

Find DEFAULT_SIZES and amend to the remote desktop resolution. For example:

`DEFAULT_SIZES = "1920x1080"`
Set the X display number to console (0):

`FIRST_X_DISPLAY_NUMBER = 0`

Comment out sections that look for additional displays:

```python
#while os.path.exists(X_LOCK_FILE_TEMPLATE % display):
#  display += 1
```
Reuse the existing X session instead of launching a new one. 

Alter launch_session() by commenting out launch_x_server() and launch_x_session() and 

instead setting the display environment variable, 

so that the function definition ultimately looks like the following:

```python
def launch_session(self, x_args):
self._init_child_env()
self._setup_pulseaudio()
self._setup_gnubby()
#self._launch_x_server(x_args)
#self._launch_x_session()
display = self.get_unused_display_number()
self.child_env["DISPLAY"] = ":%d" % display
```

Save and exit the editor. Start Chrome Remote Desktop:

`/opt/google/chrome-remote-desktop/chrome-remote-desktop --start`

#### 參考資料：

http://robotslam.blogspot.com/2018/03/ubuntu-chrome-remote-desktop-setting.html

