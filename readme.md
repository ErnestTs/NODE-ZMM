##学习总结
2016/6/28
###OmniMarkupPreviewer插件安装
你可以在 Sublime Text 内通过 Package Control 输入 Install Package 来安装 OmniMarkupPreviewer, 也可以从其 Github主页 [下载压缩包](http://theo.im/OmniMarkupPreviewer/)，解压到 Sublime Text 的 Packages 目录即可完成安装。
或者
自定义快捷键
直接在浏览器中预览效果的话，可以自定义快捷键：点击 Preferences --> 选择 Key Bindings User，输入：
{ "keys": ["alt+m"], "command": "markdown_preview", "args": {"target": "browser", "parser":"markdown"} },
保存后，直接输入快捷键：Alt + M 就可以直接在浏览器中预览生成的HTML文件了。

###MongoDB的安装和配置
####安装插件
1.下载plugins插件，放入C:\Users\Zoe\.WebStorm2016.1\config\plugins
2.在WebStorm的setting中添加路径
3.重启WebStorm右上方出现mongo explorer
3. 选择数据库，打开mongo explorer,添加数据库
```
1.下载地址<http://www.mongodb.org/downloads>
2.解压到文件夹(默认是C:\Program Files\MongoDB)
3.创建文件夹dbpath和logpath，分别用来安装db和日志文件，在log文件夹下创建一个日志文件MongoDB.log，
4.运行cmd.exe进入命令界面，执行下列命令
	cd C:\Program Files\MongoDB\Server\3.2\bin
	mongod -dbpath "E:\Web\db"
运行通过，打开http://localhost:27017/

```
####配置
```
安装服务命令
mongod --dbpath "E:\Web\db" --logpath "E:\Web\logpath\MongoDB.log" --install --serviceName "MongoDB"
出现了一些问题
措施：
1.建立一个shell的文件夹
2.建立一个startmongodb.bat的文件，内容：mongod  --dbpath E:\Web\db
3.在系统path添加路径E:\Web\shell.
```
###Mongoose部分语法
####名词解释
Schema  ：  一种以文件形式存储的数据库模型骨架
Model   ：  由Schema发布生成的模型，具有抽象属性和行为的数据库操作对
Entity  ：  由Model创建的实体，他的操作也会影响数据库
例子：
```
var mongoose = require('mongoose');
var config = require('../config')

console.log('建立mongoose连接...');

mongoose.connect(config.db.url);

mongoose.connection.on('connected', function(){
  console.log('mongoose default connection open to:' + config.db.url);
})


mongoose.connection.on('error', function(err){
  console.log('mongoose 连接错误' + err);
})

mongoose.connection.on('disconnected', function(){
  console.log('mongoose 断开连接...');
})


process.on('SIGNIT', function() {
  mongoose.connection.close(function() {
    console.log('mongoose default connection disconnected through the app termination');
    process.exit(0);
  })
})

module.exports = mongoose;
```
####新增(如果是Entity，使用save方法，如果是Model，使用create方法)
```
//使用Entity来增加一条数据
    var krouky = new PersonModel({name:'krouky'});
    krouky.save(callback);
    //使用Model来增加一条数据
    var MDragon = {name:'MDragon'};
    PersonModel.create(MDragon,callback);
```
####删除
和新增一样，Entity和Model都使用remove方法
####查询
find通常会遍历查询多条记录，findone是查询一条记录
#####直接查询
在查询时带有回调函数的，称之为直接查询，查询的条件往往通过API来设定，例如：
```
PersonModel.findOne({'name.last':'dragon'},'some select',function(err,person){
      //如果err==null，则person就能取到数据
    });
```
#####链式查询
不带回调，而查询条件通过API函数来制定，例如：
```
 var query = PersonModel.findOne({'name.last':'dragon'});
    query.select('some select');
    query.exec(function(err,pserson){
    //如果err==null，则person就能取到数据
  });
```

###Markdown的一些常见语法
####标题
```
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
####代码添加
```
'字符'
or
`Tab`或四个空格（大段文字添加代码框，每行前添加）
```
`#include <stdio.h>`
`int main(){`
`	int a;`
`	int b;`
`	return 0;`
`}	`
####图片添加
```
![名称](链接地址)
如![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png)
```
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png)
####引用
```
>a
>b
>c
or
>a
>b
>>c
```
>a
>b
>c

>a
>b
>>c
####链接
```
[参考网页](http://www.jianshu.com/p/21d355525bdf)
or
<链接>
```
[参考网页](http://www.jianshu.com/p/21d355525bdf)
####小型文本
```
<small>文本内容</small>
```
<small>文本内容</small>



###理解HTTP之常见的状态码
[参考网页](https://segmentfault.com/a/1190000005338367?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)
RFC2616定义的状态码，由3位数字和原因短信组成。
数字中的第一位指定了响应类别，后两位无分类。响应类别有以下5种：

| Type  |Reason-phrase|	Note                                          |
| ------|:-----------:| ---------------------------------------------:|
| 1XX	|Informational|	信息性状态码，表示接受的请求正在处理          |
|2XX	|Success	  |成功状态码，表示请求正常处理完毕               |
|3XX	|Redirection  |	重定向状态码，表示需要客户端需要进行附加操作  |
|4XX	|Client Error |	客户端错误状态码，表示服务器无法处理请求      |
|5XX	|Server Error |	服务器错误状态码，表示服务器处理请求出错      |


