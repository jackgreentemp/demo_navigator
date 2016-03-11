
# Titanium-navigator实例
- 简单的navigator实例
- IOS使用NavigationWindows，Android使用actionbar
- 主页面和次页面，点击主页面中的按钮，打开次页面，次页面左上角有<返回按丢，点击关闭次页面

![navitator](https://github.com/jackgreentemp/practice/blob/master/navigator.gif)
![navitator](https://github.com/jackgreentemp/practice/blob/master/navigator_android.gif)

##步骤
+ 新建工程
+ 创建导航栏
  + 修改index.xml
  ``` xml
  <Alloy>
    <Window id="win" class="container" platform="android">
        <Button id="label" onClick="doClick">Home</Button>
    </Window>
    <NavigationWindow id="nav" platform="ios" class="container">
        <Window id="win" class="container">
            <Button id="label" onClick="doClick">Home</Button>
        </Window>
    </NavigationWindow>
  </Alloy>
  ```
  + 修改index.js
  ```javascript
 
    Alloy.Globals.Navigator = {
    
        open: function(controller, payload){
            var win = Alloy.createController(controller, payload || {}).getView();
            
            if(OS_IOS){
                $.nav.openWindow(win);
            }
            else if(OS_MOBILEWEB){
                $.nav.open(win);
            }
            else {
                
                // added this property to the payload to know if the window is a child
                if (payload.displayHomeAsUp){
                    
                    win.addEventListener('open',function(evt){
                        var activity=win.activity;
                        activity.actionBar.displayHomeAsUp=payload.displayHomeAsUp;
                        activity.actionBar.onHomeIconItemSelected=function(){
                            evt.source.close();
                        };
                    });
                }
                win.open();
            }
        }
    };
    
    function doClick(e) {
       Alloy.Globals.Navigator.open("detail", {displayHomeAsUp:true});
    }
    if(OS_ANDROID) {
        $.win.open();
    } else {
        $.nav.open();
    }

  ```
  + 创建detail.xml
  ``` xml
  <Alloy>
    <Window id="win" class="container" title="index">
        <ActionBar platfor="android" displayHomeAsUp="true" onHomeIconItemSelected="closeWindow" />
    </Window>
  </Alloy>
  ```
  + 创建detail.js
  ``` javascript
    function closeWindow(){
      $.win.close();
    }
  ```

  
  ```

## 使用到的组件以及实现的功能
- ListView
- collections and model
- pull to refresh（基于[nl.fokkezb.pullToRefresh][1]）
- 分页


  [1]: https://github.com/FokkeZB/nl.fokkezb.pullToRefresh