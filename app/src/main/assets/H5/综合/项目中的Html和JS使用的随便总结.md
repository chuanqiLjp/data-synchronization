---
title: 项目中的Html和JS使用的随便总结
layout: post
date: 2018-02-08 16:13:58
comments: true
categories:
  - HTML
tags: [html,项目总结]
keywords:  html
description: 
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

### 序言：
由于这段时间公司有个项目需要做一个问卷调查的在线生成，大概需求：可以在线添加问题、删除问题、最后生成问卷，大概的界面是：
![问卷调查模版生成分析.png](http://upload-images.jianshu.io/upload_images/4143664-a2ff2098ba3a5d45.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
公司的web工程师这段时间由比较忙，自己刚好处于学习H5阶段，于是就顺手鲁一个了，先来看一下效果图吧！
![image.png](http://upload-images.jianshu.io/upload_images/4143664-72978c145f1d92d9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我先总结一下这个项目中使用到的知识点，然后有兴趣的同学可以看下我的源代码是怎么实现的（还好没有涉及到公司的利益可以分享出来）


### 1、日志打印
```
console.log("向控制台输出日志");
alert("弹出警告框");
confirm("确认消息内容");  // 确认框，返回布尔值
```

### 2、JS中变量的定义
```
var listQue=new Array()；// 定义一个数组列表
var queItem={};//定义一个对象
var  arr=["ABC","EFG" ];//定义一个数组并初始化
```

### 3、数组列表的操作
```
var listQue=new Array()；// 定义一个数组列表
var length=listQue.length;//获取到列表中的长度
var item=listQue[10];//获取下标为10的元素
listQue.push(queItem);//添加一个元素到列表中
listQue.splice(index,deleteLength);//删除列表中的元素，index：删除开始的下标，deleteLength：删除元素的个数
listQue.splice(0,listQue.length);//清空数组列表
var str = listQue.join("");//转换为字符串
var jsonStr=JSON.stringify(listQue);//将字符串转换为JSON字符串
```

### 4、字符串的操作
```
str.substring(start,end);//截取字符串并返回新的字符串，重载方法：str.substring(start);
```
### 5、标签元素的操作
```
element.getAttribute("属性名")；//获取指定属性名的属性值
element.id;//获取标签的ID值
element.checked;//获取标签是否被选中，true被选中，type="radio"的input标签
element.setAttribute("属性名","属性值");//为指定属性设置属性值
 element.parentNode.removeChild(element); // 让 “要删除的元素” 的 “父元素” 删除 “要删除的元素”
element.insertAdjacentHTML(swhere,stext);//swhere:指定插入html标签语句的地方，有四种值可以用：beforeBegin:插入到标签开始前，afterBegin:插入到标签开始标记后，beforeEnd:插入到标签结束标记前，afterEnd:插入到标签结束标记后； stext:要插入的HTML内容
element.insertAdjacentText(swhere,stext);//与insertAdjacentHTML方法类似，只不过只能插入纯文本，参数相同
document.getElementsByName("name值");//根据name属性的值获取到一个标签列表
document.getElementById("ID值");//根据标签的id找到此标签
document.getElementsByTagName("标签名");//根据标签名称找到一个标签列表
```
### 6、页面的初始化
```
方法一： 在标签上静态绑定onload事件，<body onload="aaa()">等待body加载完成，就会执行aaa()方法。
方法二： 使用window.onload = function(){ console.log("页面加载完成后执行此方法，可以进行数据的填充，执行页面的初始化操作");} 等到整个window加载（页面加载完成）完成执行方法体,可以进行数据的填充。比方法一先执行
```

### 7、单选框
依靠name判断是否为一组的，type取值可以为radio（单选）、checkbox（多选），checked属性的值为true表示默认选中
```
问题类型：
<input type="radio" name="questType0" id="idQuestTypeRadio0" checked="true">单选
<input type="radio" name="questType0" id="idQuestTypeCheckbox0">多选
```

### 8、下拉选择
选项改变会调用属性为onChange的方法，标签option的属性selected="selected"表示默认被选中，JS中var optionValue=select.options[select.options.selectedIndex].value//获取已选的值，
```
                选项个数：
                <select name="optonsNum" id="idOptionNum0" onChange="optionOnChange(this,null)">
                    <option name="name_idOptionNum0_option">请选择</option>
                    <option name="name_idOptionNum0_option">A</option>
                    <option name="name_idOptionNum0_option">AB</option>
                    <option name="name_idOptionNum0_option" selected="selected">ABC</option>
                    <option name="name_idOptionNum0_option">ABCD</option>
                    <option name="name_idOptionNum0_option">ABCDE</option>
                    <option name="name_idOptionNum0_option">ABCDEF</option>
                    <option name="name_idOptionNum0_option">ABCDEFG</option>
                </select>
```

### 9、input标签
inputElement.value;//JS中获取输入框中的内容
placeholder属性：输入框提示内容；
value属性：输入框默认的值
maxlength属性：最长的输入长度
type属性：输入的类型，常见的有：
```
button	定义可点击按钮（多数情况下，用于通过 JavaScript 启动脚本）。
checkbox	定义复选框。
file	定义输入字段和 "浏览"按钮，供文件上传。
hidden	定义隐藏的输入字段。
image	定义图像形式的提交按钮。
password	定义密码字段。该字段中的字符被掩码。
radio	定义单选按钮。
reset	定义重置按钮。重置按钮会清除表单中的所有数据。
submit	定义提交按钮。提交按钮会把表单数据发送到服务器。
text	定义单行的输入字段，用户可在其中输入文本。默认宽度为 20 个字符。
```

```
<input id="idOptionNum0_0" class="class_option" placeholder="输入框提示内容" maxlength="30" type="text" name="name_idOptionNum0_input" type="text" value="输入框默认值"> </input>
```

### 10、内部样式
```
<style type="text/css">
 /* 为class属性为class_option的设置样式，class使用小数点.，id使用#， */
    .class_option{
        margin-left:50px;
        margin-bottom: 20px;
        margin-top:25px;
        margin-left:900px;
        border:1px green solid;  /* 为标签添加边框，边框的宽度为1px，绿色，有立体效果*/
        width: 300px;/* 宽度 */
    }
 /* 为class属性为class_ulOption 下的标签li设置样式 */
    .class_ulOption li{
        line-height: 40px;/*  每一行的高度 */  /* */
        list-style-type: none;/*取消li标签前面的小圆点*/
    }
     /* 为全部input标签设置样式 */
    input{
        color: crimson;
    }
</style>
```

### 11、[Google Chrome调试JavaScript](https://www.cnblogs.com/yuanchaoyong/p/6172034.html)

### 12、CSS （Cascading Style Sheets ：层叠样式表）CSS选择器
选择器优先级：ID选择器>类选择器>标签选择器，当选择器优先级相同的情况下，采用“就近原则”(最后一次出现的选择器)
```
    1.标签选择器
      标签名{
         属性名1:属性值1;
         属性名2:属性值2;
         ...
      }
    2.类选择器(class选择器)
      .类名{
         属性名1:属性值1;
         属性名2:属性值2;
         ...
      }
    3.ID选择器
         #ID名{
         属性名1:属性值1;
         属性名2:属性值2;
         ...
      }
```

### 13、JavaScript函数的声明
```
        function 函数名(xx,xx){
            // 函数体代码
        }
```

### 14、其他操作
```
var intValue = parseInt("123");//将字符串转换为一个整数
&nbsp;：空格； emsp;：一个字符的空格
```

###项目的源代码
代码中都有详尽的注释，就不讲解了，放在这里大家参考下吧，有什么好的意见建议都可以留言指出来
```
<!DOCTYPE html>
<html>
<head>
    <title>生成问卷调查的模版</title>
    <script type="text/javascript">
        var listQue=new Array();//字段：serialNum（题号-1）、content（问题内容）、type（选项类型，ture：单选）、optionNum（选项个数）、options（答案的数组）
        var lastQuestItemID="questionList";//插入问题的最后一次目标问题ID
        /*页面的初始化
            方法一： 在标签上静态绑定onload事件，<body onload="aaa()">等待body加载完成，就会执行aaa()方法。
            方法二： 使用window.onload = function(){} 等到整个window加载（页面加载完成）完成执行方法体,可以进行数据的填充。比方法一先执行
        */
        window.onload=function (){
            console.log("页面加载完成后执行此方法，可以进行数据的填充，执行页面的初始化操作");
            for (var i = 0; i < 10; i++) {
                var queItem={};
                queItem.serialNum=i;
                queItem.content="测试——问题内容——"+i;
                queItem.type=i%2==0?true:false;
                queItem.options=[i+"测试选项A",i+"测试选项",i+"测试选项"];
                queItem.optionNum=queItem.options.length;
                listQue.push(queItem);
            }

            for (var i = 0; i < listQue.length; i++) {
                var queItem = listQue[i];
                console.log("delsteQuest——》queItem="+queItem);
                var insertHtml=generateHtmlForQuestionItem(queItem);
                insertHTML(lastQuestItemID,insertHtml,"afterEnd");
                var select=document.getElementById("idOptionNum"+queItem.serialNum);
                optionOnChange(select,queItem.options);
                lastQuestItemID="list"+(queItem.serialNum);
            }

        }

        //点击添加问题按钮的时候会调用,用于添加单个问题
        function addNextQuest(){
            // alert("添加问题");
            console.log("添加问题");
            //创建一个问题对象，并初始化
            var queItem={};
            queItem.serialNum=listQue.length;//题号
            queItem.content="问题内容——>初始值";//问题内容
            queItem.type=true;//选项类型，ture：单选
            queItem.optionNum=0;//选项个数
            queItem.options=[];//答案的数组
            queItem.optionNum=queItem.options.length;//选项个数
            listQue.push(queItem);//添加到问卷的列表
            var insertHtml=generateHtmlForQuestionItem(queItem);
            insertHTML(lastQuestItemID,insertHtml,"afterEnd");
            lastQuestItemID="list"+(queItem.serialNum);
            console.log(queItem);
        }
        //每道题的选项个数发生变化的时候会调用
        function optionOnChange(select,options){
            //获取已选的值： this.options[this.options.selectedIndex].value
            var optionValue=select.options[select.options.selectedIndex].value//获取已选的值
            if (optionValue=="请选择") {
                return;
            }
            var selectID=select.id;//该select 的id值
            var optionNum =optionValue.length;
            console.log("optionValue="+optionValue+",id="+selectID+",optionNum="+optionNum);
            var element =document.getElementById(selectID+"_list");
            if (element!=null) {
                deleteHtml(selectID+"_list");
            }
            //idOptionNum0
            var insertHtml= generateHtmlForQuestionOption(selectID,optionNum,options);
            console.log(selectID.substring(11));
            insertHTML("delete"+selectID.substring(11),insertHtml,"afterEnd");
        }
        /*删除一道与该删除按钮同一级的问题
        btnDelete ：     按钮的对象
        */
        function delsteQuest(btnDelete){
            // alert("删除问题");delete
            var id=btnDelete.id;
            var index=id.substring(6);
            console.log("删除问题,id="+id+",index="+index);
            deleteHtml("list"+index);
            listQue.splice(index,1);

            var queItemInputList=getQueItemInputValue();
            deleteQuestionItemALL();
            for (var i = 0; i < queItemInputList.length; i++) {
                var queItem = queItemInputList[i];
                console.log("delsteQuest——》queItem="+queItem);
                listQue.push(queItem);
                var insertHtml=generateHtmlForQuestionItem(queItem);
                insertHTML(lastQuestItemID,insertHtml,"afterEnd");
                // idOptionNum0
                var select=document.getElementById("idOptionNum"+queItem.serialNum);
                optionOnChange(select,queItem.options);
                lastQuestItemID="list"+(queItem.serialNum);
            }

        }
        //删除全部问题项
        function deleteQuestionItemALL(){
            var tem=document.getElementsByName("questionItem");
            if (tem!=null&&tem.length>0) {
                console.log("deleteQuestionItemALL——》questionItem  Length="+tem.length);
                for (var i = 0; i < tem.length; ) {// 由于tem数组中值会随着删除而减小，所以此处无须对i进行加加操作；
                    // console.log(i+"_________________________"+tem[0].getAttribute("id"));
                    deleteHtml(tem[i].getAttribute("id"));
                }
                listQue.splice(0,listQue.length);
            }
            if (listQue.length==0) {
                lastQuestItemID="questionList";
            }
        }
        /*生成问卷
        */
        function creatQuest(){
            console.log("生成问卷");
            var queItemInputList=getQueItemInputValue();
            var jsonStr=JSON.stringify(queItemInputList);
            console.log(jsonStr);
        }
        /*获取所有问题输入的数据，得到一个集合，
        字段：serialNum（题号-1）、content（问题内容）、type（选项类型，ture：单选）、optionNum（选项个数）、options（答案的数组）
        */
        function getQueItemInputValue(){
            var queItemInputList=new Array();
            var questionItemList=document.getElementsByName("questionItem");
            if (questionItemList!=null&&questionItemList.length>0) {
                for (var i = 0; i < questionItemList.length; i++) {
                    var queItemID=questionItemList[i].getAttribute("id");
                    var idNum = parseInt(queItemID.substring(4));
                    console.log("idNum="+idNum);
                    var queItem={};
                    queItem.serialNum=i;//题号
                    queItem.content=document.getElementById("questContent"+idNum).value;//问题内容
                    var queTypeList=document.getElementsByName("questType"+idNum);
                    if (queTypeList!=null&& queTypeList.length>0) {
                        if (queTypeList[0].checked==true) {
                            queItem.type=true;//选项类型，ture：单选
                        }else if (queTypeList[1].checked==true) {
                            queItem.type=false;//选项类型，ture：单选
                        }
                    }
                    // name_idOptionNum0_input
                    queItem.optionNum=0;//选项个数
                    queItem.options=[];//答案的数组
                    var optionList=document.getElementsByName("name_idOptionNum"+idNum+"_input");// var optionList=document.getElementsByName("name_idOptionNum0_input");
                    if (optionList!=null && optionList.length>0) {
                        var options=[];
                        for (var j = 0; j < optionList.length; j++) {
                            options.push(optionList[j].value);
                            // console.log(queItem.options[j]);
                        }
                        queItem.options=options;
                        queItem.optionNum=optionList.length;//选项个数
                    }

                    console.log(queItem);
                    queItemInputList.push(queItem);//添加到问卷的列表
                }
            }
            return queItemInputList;
        }
        /** 在目标元素后面插入一段新的HTML并重新渲染
        targetElementId :   在目标元素ID插入HTML代码
        insertHtml :    插入的HTML代码
        insertType :    插入的类型，
        beforeBegin:插入到标签开始前 ；afterBegin:插入到标签开始标记后 ；beforeEnd:插入到标签结束标记前 ；afterEnd:插入到标签结束标记后
        */
        function insertHTML(targetElementId,insertHtml,insertType){
            console.log("在目标元素ID="+targetElementId+"插入HTML代码");
            console.log("插入的HTML代码："+insertHtml);
            document.getElementById(targetElementId).insertAdjacentHTML(insertType,insertHtml);
            // 提供了两个方法来进行添加，insertAdjacentHTML和insertAdjacentText
            //     insertAdjacentHTML方法：在指定的地方插入html标签语句。
            //     insertAdjacentText方法与insertAdjacentHTML方法类似，只不过只能插入纯文本，参数相同
            //     原型：insertAdjacentHTML(swhere,stext)
            //     参数： swhere:指定插入html标签语句的地方，有四种值可以用：
            //     1.beforeBegin:插入到标签开始前
            //     2.afterBegin:插入到标签开始标记后
            //     3.beforeEnd:插入到标签结束标记前
            //     4.afterEnd:插入到标签结束标记后
            //     stext:要插入的内容
        }
        /*删除指定ID的网页片段
        htmlID ：    片段的ID
        */
        function deleteHtml(htmlID){
            console.log("删除指定ID的网页片段,htmlID="+htmlID);
            var element=document.getElementById(htmlID); // 按 id 获取要删除的元素
            if (element!=null) {
                element.parentNode.removeChild(element); // 让 “要删除的元素” 的 “父元素” 删除 “要删除的元素”
            }
        }
        /*生成一道问题的选项的HTML代码片段
            selectID ：  select的ID
            optionNum ： 选项的个数
        */
        function generateHtmlForQuestionOption(selectID,optionNum,options){
            console.log("生成一道问题的选项的HTML代码片段 selectID="+selectID+",optionNum="+optionNum+",options"+options);
            var build =new Array();
            build.push("<ul id=\"");
            build.push(selectID+"_list");
            build.push("\" class=\"class_ulOption\">");
            var optArr=["A","B","C","D","E","F","G"];
            for (var i = 0; i < optionNum; i++) {
                build.push("<li>");
                build.push("<label>");
                build.push(optArr[i]+" . ");
                build.push("</label>");
                build.push("<input id=\"");
                build.push(selectID+"_"+i);
                build.push("\" class=\"class_option\" placeholder=\"");
                build.push("请输入选项"+optArr[i]+"的内容");
                build.push("\" maxlength=\"30\" type=\"text\"  name=\"name_"+selectID+"_input\" type=\"text\"");
                if (options!=null&&options[i]!=null) {
                    build.push(" value=\""+options[i]+"\"");
                }
                build.push("/>");
                build.push("</li>");
            }
            build.push("</ul>");
            var result=build.join("");
            return result;
        }

        //根据指定的问题Item生成一段问题的网页
        function  generateHtmlForQuestionItem(queItem){
            console.log(queItem);
            var build =new Array();

            build.push("<div id=\"");
            build.push("list"+queItem.serialNum);
            build.push("\" class=\"class_questionItem\"  name=\"questionItem\">");
            build.push("<div>");
            build.push("<label>问题编号：");
            build.push((queItem.serialNum+1));
            build.push("</label><br><br> <label>问题内容：</label>&nbsp;&nbsp;");
            build.push("<input name=\"questContent\" class=\"class_quentionContent\" placeholder=\"请输入问题的内容\" maxlength=\"255\" size=\"40\"  id=\"questContent"+queItem.serialNum+"\"  type=\"text\" value=\""+queItem.content+"\"> </input>");
            build.push("&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;")
            build.push("问题类型：");
            build.push("<input type=\"radio\" name=\"questType"+queItem.serialNum+"\" id=\"idQuestTypeRadio"+queItem.serialNum+"\"  checked=\""+queItem.type+"\">")
            build.push("单选");
            if (!queItem.type) {
                build.push("<input type=\"radio\" name=\"questType"+queItem.serialNum+"\" id=\"idQuestTypeCheckbox"+queItem.serialNum+"\" checked=\"true\">");
            }else{
                build.push("<input type=\"radio\" name=\"questType"+queItem.serialNum+"\" id=\"idQuestTypeCheckbox"+queItem.serialNum+"\" >");
            }

            build.push("多选 &nbsp;&nbsp;&nbsp;&nbsp;");
            build.push(" 选项个数：<select name=\"optonsNum\" ");

            build.push(" id=\"");
            build.push("idOptionNum"+queItem.serialNum);
            build.push("\" onChange =\"optionOnChange(this,null)\">");

            var arr=["请选择","A","AB","ABC","ABCD","ABCDE","ABCDEF","ABCDEFG"];
            for (var i = 0; i < 8; i++) {
                // build.push("<option name=\"name_idOptionNum"+queItem.serialNum+"_option\">ABCD</option>");
                var tem=new Array();
                tem.push("<option name=\"name_idOptionNum");
                tem.push(queItem.serialNum);
                tem.push("_option\"");
                // if (i==0 && queItem.optionNum==0) {
                //     tem.push(" selected=\"selected\"");
                // }else if (i != 0 && queItem.optionNum==i) {
                // }
                if (queItem.optionNum==i) {
                    tem.push(" selected=\"selected\"");
                }
                tem.push(">");
                tem.push(arr[i]);
                tem.push("</option>");
                build.push(tem.join(""));

            }

            build.push("</select>");
            build.push(" &emsp;&emsp;");
            build.push("</div>");

            build.push("<button onClick=\"delsteQuest(this)\" id=\"delete");
            build.push(queItem.serialNum);
            build.push("\" class=\"class_btnDelete\">删除问题</button>");

            // build.push("</div>");
            build.push("<br>");
            build.push("</div>");
            var result=build.join("");
            return result;
        }
        /*设置选项的选中值
        serialNum :  问题序号，用于确定select的ID值
        optionNum ： 被选中的下标
        */
        function setOptionsValue(serialNum,optionNum){
            var tem=document.getElementsByName("name_idOptionNum"+serialNum+"_option");
            console.log("设置选项的选中值 serialNum="+serialNum+",optionNum="+optionNum+",Length="+tem.length);
            if (tem!=null&&tem.length>0) {
                for (var i = 0; i < tem.length; i++) {
                    if (i==optionNum) {
                        tem[i].setAttribute("selected","selected");
                    }
                }
            }
        }

    </script>
    <style type="text/css">
    /*//删除按钮的样式*/
    .class_btnDelete{
        margin-top:25px;
        margin-left:900px;
        border:1px red solid;
    }
    /*//每一道问题外部的样式*/
    .class_questionItem{
        margin-left:50px;
        margin-bottom: 20px;
        border:1px green solid;  /* 为标签添加边框，边框的宽度为1px，绿色，有立体效果*/
    }
    .class_option{
        width: 300px;/* 宽度 */
    }
    .class_ulOption li{
        line-height: 40px;/*  每一行的高度 */  /* */
        list-style-type: none;/*取消li标签前面的小圆点*/
    }



    </style>
</head>
<body>
<div class="place">
    <span>位置：</span>
    <ul class="placeul">
        <li><a href="#">问卷管理</a></li>
        <li><a href="#">问卷生成</a></li>
    </ul>
</div>
<div class="formbody">
    <div class="formtitle"><span>基本信息</span></div>
    <div id="head" style="margin-left:50px;border:1px red solid;">
        <br>
        <label>问卷名称：</label>&nbsp;&nbsp;
        <input name="questname" class="dfinput" placeholder="请输入问题名称，不错过25个字符"> </input>&nbsp;&nbsp;&nbsp;&nbsp;
        <label>问卷编号：ON.20180105001</label>&nbsp;&nbsp;&nbsp;&nbsp;
        <label>操作员：李美玲_10086</label>&nbsp;&nbsp;&nbsp;&nbsp;
    </div>
    <br>
    <hr>


    <div id="questionList">
        <!-- 第一个问题的开始 -->
        <!-- <div id="list0" class="class_questionItem" name="questionItem">
            <div>
                <label>问题编号：1</label>
                <br><br>
                <label>问题内容：</label>&nbsp;&nbsp;
                <input name="questContent" class="class_quentionContent" placeholder="请输入问题的内容"
                       maxlength="255" size="40" id="questContent0" type="text"
                       value="测试——问题内容——0"> </input>
                &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;
                问题类型：
                <input type="radio" name="questType0" id="idQuestTypeRadio0" checked="true">单选
                <input type="radio" name="questType0" id="idQuestTypeCheckbox0">多选
                &nbsp;&nbsp;&nbsp;&nbsp;
                选项个数：
                <select name="optonsNum" id="idOptionNum0" onChange="optionOnChange(this,null)">
                    <option name="name_idOptionNum0_option">请选择</option>
                    <option name="name_idOptionNum0_option">A</option>
                    <option name="name_idOptionNum0_option">AB</option>
                    <option name="name_idOptionNum0_option" selected="selected">ABC</option>
                    <option name="name_idOptionNum0_option">ABCD</option>
                    <option name="name_idOptionNum0_option">ABCDE</option>
                    <option name="name_idOptionNum0_option">ABCDEF</option>
                    <option name="name_idOptionNum0_option">ABCDEFG</option>
                </select> &emsp;&emsp;
            </div>
            <button onClick="delsteQuest(this)" id="delete0" class="class_btnDelete">删除问题</button>
            <br>
        </div> -->
        <!-- 第一个问题的结束 -->

    </div>
    <dir>
        <button onClick="addNextQuest()">添加问题</button>
        <input type="submit" name="sub" value="生成问卷" onClick="creatQuest()">
    </dir>
</div>
</body>
</html>
```

我的CSDN博客：http://blog.csdn.net/wo_ha/article/details/79290964