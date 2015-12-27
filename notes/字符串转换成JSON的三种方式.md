### 字符串转换成JSON的三种方式
1，eval方式解析，恐怕这是最早的解析方式了。
```javascript
function strToJson(str){
     var json = eval('(' + str + ')');
     return json;
}
```
>记得str两旁的小括号哦。
