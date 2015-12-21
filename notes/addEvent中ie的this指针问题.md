> IE中使用的事件绑定函数与Web标准的不同，而且this指向也不一样，Web标签中的this指向与传统事件绑定中的this一样，是当前目标，但是IE中事件绑定函数中this指向，通过使用call或apply可以改变this指针的指向。

``` javascript
<!DOCTYPE HTML>
<html lang="en-US">
<head>
    <meta charset="UTF-8">
    <title>attachEvent 中this指向</title>
    <script type="text/javascript" src="kingwell.js"></script>
    <script type="text/javascript">
        
        window.onload = function(){

            var test = document.getElementById('test');
            test.onclick = function(){
                //alert(this);//this指向 h1
            }
            test.attachEvent('onclick',function(){
                //alert(this);// this指向window
            });
            test.addEventListener('click',function(){
                //alert(this);//this指向 h1
            },false);
            //传统的绑定跟Web标准中this指向是正确的，都是所绑定的那个对象，但IE的总是指向window，这是错误的
            //只要稍微调整一个，就可以修正这个错误

            var Event = {};
            Event.addEvent = function(target,eventType,handle){
            
                if(document.addEventListener){
                    Event.addEvent = function(target,eventType,handle){
                        target.addEventListener(eventType,handle,false);
                    }
                }else if(document.attachEvent){
                    Event.addEvent = function(target,eventType,handle){
                        target.attachEvent('on'+eventType,function(){
                            handle.call(target,arguments);//改变this指向
                        });
                    }
                }else{
                    Event.addEvent = function(target,eventType,handle){
                        target['on'+eventType] = handle;
                    }
                }
                Event.addEvent (target,eventType,handle);
            }
            Event.addEvent(test,'click',function(){
                alert('我是正确的，我是：'+this);//现在this指向的是正确的了
            });
        }
    </script>
</head>
<body>
<h1 id="test">attachEvent this</h1>
</body>
</html>

```
