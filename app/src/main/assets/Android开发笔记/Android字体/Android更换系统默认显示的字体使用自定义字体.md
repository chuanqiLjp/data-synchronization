---
title: Android更换系统默认显示的字体使用自定义字体
layout: post
date: 2018-01-30 10:07:58
comments: true
categories:
  - Android
  - Android字体
tags: [Android 自定义字体,自定义系统字体]
keywords: Android 自定义字体,自定义系统字体
description:
---

>我的简书：https://www.jianshu.com/u/c91e642c4d90
我的CSDN：http://blog.csdn.net/wo_ha
我的GitHub：https://github.com/chuanqiLjp
我的个人博客：https://chuanqiljp.github.io/

# 版权声明：转载需要在醒目注明出处


### 序言
上一篇[Android 自定义字体，更换系统默认显示的字体使用自定义字体](https://www.jianshu.com/p/282716d73c6a)有讲到怎样指定控件显示指定字体，怎样整个软件显示指定字体，怎样WebView加载指定字体，但是还留下一个怎样修改整个系统的默认字体，由于内容较多，所以单独抽离出来讲，由于要操作系统文件，因此需要Root权限或系统签名，自己在操作前建议先备份下字体配置文件/system/etc/system_fonts.xml和/system/etc/fallback_fonts.xml，否则操作失败有可能开机后无法进入桌面，此时就需要将备份的system_fonts.xml推送到对应目录下并修改为对应的权限。

### 版权声明：本文可被转载，但需要在醒目位置注明原文出处：https://www.jianshu.com/p/b0b541b94427

# 1、字体加载原理
- ①  Android 系统的字体文件：位于 /system/fonts/ 文件夹下，我们可以到对应的目录下进行查看，可以看出，Android的字体文件都是ttf文件，命令顺序：adb shell ——>cd /system/fonts/ ——>ll,查看结果如下图所示：
![image.png](http://upload-images.jianshu.io/upload_images/4143664-7f7813ef09a1a0b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- ②  在/system/etc/目录下有两个字体配置文件，分别是system_fonts.xml 和 fallback_fonts.xml ，当系统需要加载字体时，会优先从 system_fonts.xml 文件开始查找，如果没有找到再进入 fallback_fonts.xml 查找。


**system_fonts.xml示范文件**
```
<?xml version="1.0" encoding="UTF-8"?><!--    System Fonts    This file lists the font families that will be used by default for all supported glyphs.    Each entry consists of a family, various names that are supported by that family, and    up to four font files. The font files are listed in the order of the styles which they    support: regular, bold, italic and bold-italic. If less than four styles are listed, then    the styles with no associated font file will be supported by the other font files listed.    The first family is also the default font, which handles font request that have not specified    specific font names.    Any glyph that is not handled by the system fonts will cause a search of the fallback fonts.    The default fallback fonts are specified in the file /system/etc/fallback_fonts.xml, and there    is an optional file which may be supplied by vendors to specify other fallback fonts to use    in /vendor/etc/fallback_fonts.xml.-->
<familyset>
    <family>
        <nameset>
            <name>sans-serif</name>
            <name>arial</name>
            <name>helvetica</name>
            <name>tahoma</name>
            <name>verdana</name>
        </nameset>
        <fileset>
            <file>Roboto-Regular.ttf</file>
            <file>Roboto-Bold.ttf</file>
            <file>Roboto-Italic.ttf</file>
            <file>Roboto-BoldItalic.ttf</file>
        </fileset>
    </family>
    <family>
        <nameset>
            <name>sans-serif-light</name>
        </nameset>
        <fileset>
            <file>Roboto-Light.ttf</file>
            <file>Roboto-LightItalic.ttf</file>
        </fileset>
    </family>
    <family>
        <nameset>
            <name>sans-serif-thin</name>
        </nameset>
        <fileset>
            <file>Roboto-Thin.ttf</file>
            <file>Roboto-ThinItalic.ttf</file>
        </fileset>
    </family>
    <family>
        <nameset>
            <name>sans-serif-condensed</name>
        </nameset>
        <fileset>
            <file>RobotoCondensed-Regular.ttf</file>
            <file>RobotoCondensed-Bold.ttf</file>
            <file>RobotoCondensed-Italic.ttf</file>
            <file>RobotoCondensed-BoldItalic.ttf</file>
        </fileset>
    </family>
    <family>
        <nameset>
            <name>serif</name>
            <name>times</name>
            <name>times new roman</name>
            <name>palatino</name>
            <name>georgia</name>
            <name>baskerville</name>
            <name>goudy</name>
            <name>fantasy</name>
            <name>cursive</name>
            <name>ITC Stone Serif</name>
        </nameset>
        <fileset>
            <file>DroidSerif-Regular.ttf</file>
            <file>DroidSerif-Bold.ttf</file>
            <file>DroidSerif-Italic.ttf</file>
            <file>DroidSerif-BoldItalic.ttf</file>
        </fileset>
    </family>
    <family>
        <nameset>
            <name>Droid Sans</name>
        </nameset>
        <fileset>
            <file>DroidSans.ttf</file>
            <file>DroidSans-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <nameset>
            <name>monospace</name>
            <name>courier</name>
            <name>courier new</name>
            <name>monaco</name>
        </nameset>
        <fileset>
            <file>DroidSansMono.ttf</file>
        </fileset>
    </family>
</familyset>
```


**fallback_fonts.xml 示范文件**
```
<?xml version="1.0" encoding="utf-8"?><!--    Fallback Fonts    This file specifies the fonts, and the priority order, that will be searched for any    glyphs not handled by the default fonts specified in /system/etc/system_fonts.xml.    Each entry consists of a family tag and a list of files (file names) which support that    family. The fonts for each family are listed in the order of the styles that they    handle (the order is: regular, bold, italic, and bold-italic). The order in which the    families are listed in this file represents the order in which these fallback fonts    will be searched for glyphs that are not supported by the default system fonts (which are    found in /system/etc/system_fonts.xml).    Note that there is not nameset for fallback fonts, unlike the fonts specified in    system_fonts.xml. The ability to support specific names in fallback fonts may be supported    in the future. For now, the lack of files entries here is an indicator to the system that    these are fallback fonts, instead of default named system fonts.    There is another optional file in /vendor/etc/fallback_fonts.xml. That file can be used to    provide references to other font families that should be used in addition to the default    fallback fonts. That file can also specify the order in which the fallback fonts should be    searched, to ensure that a vendor-provided font will be used before another fallback font    which happens to handle the same glyph.    Han languages (Chinese, Japanese, and Korean) share a common range of unicode characters;    their ordering in the fallback or vendor files gives priority to the first in the list.    Language-specific ordering can be configured by adding a BCP 47-style "lang" attribute to    a "file" element; fonts matching the language of text being drawn will be prioritised over    all others.-->
<familyset>
    <family>
        <fileset>
            <file variant="elegant">DroidNaskh-Regular.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">DroidNaskhUI-Regular.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>DroidSansEthiopic-Regular.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>DroidSansHebrew-Regular.ttf</file>
            <file>DroidSansHebrew-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansThai-Regular.ttf</file>
            <file variant="elegant">NotoSansThai-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansThaiUI-Regular.ttf</file>
            <file variant="compact">NotoSansThaiUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>DroidSansArmenian.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>DroidSansGeorgian.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansDevanagari-Regular.ttf</file>
            <file variant="elegant">NotoSansDevanagari-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansDevanagariUI-Regular.ttf</file>
            <file variant="compact">NotoSansDevanagariUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansTamil-Regular.ttf</file>
            <file variant="elegant">NotoSansTamil-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansTamilUI-Regular.ttf</file>
            <file variant="compact">NotoSansTamilUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansMalayalam-Regular.ttf</file>
            <file variant="elegant">NotoSansMalayalam-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansMalayalamUI-Regular.ttf</file>
            <file variant="compact">NotoSansMalayalamUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansBengali-Regular.ttf</file>
            <file variant="elegant">NotoSansBengali-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansBengaliUI-Regular.ttf</file>
            <file variant="compact">NotoSansBengaliUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansTelugu-Regular.ttf</file>
            <file variant="elegant">NotoSansTelugu-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansTeluguUI-Regular.ttf</file>
            <file variant="compact">NotoSansTeluguUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansKannada-Regular.ttf</file>
            <file variant="elegant">NotoSansKannada-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansKannadaUI-Regular.ttf</file>
            <file variant="compact">NotoSansKannadaUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansKhmer-Regular.ttf</file>
            <file variant="elegant">NotoSansKhmer-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansKhmerUI-Regular.ttf</file>
            <file variant="compact">NotoSansKhmerUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="elegant">NotoSansLao-Regular.ttf</file>
            <file variant="elegant">NotoSansLao-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file variant="compact">NotoSansLaoUI-Regular.ttf</file>
            <file variant="compact">NotoSansLaoUI-Bold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>NanumGothic.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>Padauk-book.ttf</file>
            <file>Padauk-bookbold.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>NotoSansSymbols-Regular.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>AndroidEmoji.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>NotoColorEmoji.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file>DroidSansFallback.ttf</file>
        </fileset>
    </family>
    <family>
        <fileset>
            <file lang="ja">MTLmr3m.ttf</file>
        </fileset>
    </family>    <!-- Note: complex scripts (i.e. those requiring shaping in Harfbuzz) have         a cumulative limit of 64k glyphs. Thus, if they are placed after the         large fonts such as DroidSansFallback, they are likely to render         incorrectly. Please use caution when putting fonts toward the end of         the list.    -->
</familyset>
```

# 2、为系统添加一个默认字体
修改系统默认字体的原理：根据系统字体加载原理可知，我们只需要在路径 /system/fonts/ 下添加我们自定义的ttf字体文件，然后修改 /system/etc/system_fonts.xml  字体配置文件，按照响应的格式添加一个节点，由于需要系统默认使用该字体，因此该节点需要是根节点familyset下的第一个子节点，系统在system_fonts.xml中找到了该字体的配置，故不会去fallback_fonts.xml 寻找，因此也只需要修改这一个配置文件即可，文件修改成功后需要注意已修改文件的读写权限（否则会没有效果），为了方便，我们设置全部用户可读可写。
### ①  判断系统字体库是否已有需要添加的字体文件

```
    /**
     * 检查字体文件是否存在系统目录中
     * @param fontName 字体文件名称
     * @return  ture：已存在
     */
    private boolean checkFontTTFFile(String fontName) {
        File dir = new File(systemFontsDir);
        File[] files = dir.listFiles();   //字库列表
        if (files != null && files.length > 0) {
            for (int i = 0; i < files.length; i++) {
                File file = files[i];
                if (file.exists() && file.getName().equals(fontName)) {
                    return true;
                }
            }
        }
        return false;
    }
```
### ② 若①判断后没有则需要拷贝字体文件到对应路径下
```
   /**
     * 拷贝字体文件
     *
     * @param fontName 字体文件名
     * @param destDir  目标目录
     */
    private void copyFontTTF(String fontName, String destDir) {
        try {
            //将Assets文件夹中的字体文件读出来
            InputStream inputStream = getAssets().open("fonts/" + fontName);
            //将字体文件写入到sd卡中 ,不能直接写入 /system/fonts/下，因为没有写文件的权限
            File file = new File(getCacheDir(), fontName);
            FileOutputStream fos = new FileOutputStream(file);
            int leng = 0;
            byte[] buffer = new byte[1024];
            while (-1 != (leng = inputStream.read(buffer))) {
                fos.write(buffer, 0, leng);
            }
            fos.flush();
            fos.close();
            inputStream.close();
            runRootCommand("cp " + file.getAbsolutePath() + " " + destDir); //使用命令进行文件拷贝
            Log.e(TAG, "copyFontTTF: 字体文件拷贝成功");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

### ③ 判断字体配置文件 system_fonts.xml 是否已有该节点，若有则不进行操作，若无进行步骤④

```
    /**
     * 检查指定的XML文件中是否有节点名称为  refNodeName 的值为  compareNodeValue 的节点
     *
     * @param refNodeName      参考的节点名称
     * @param compareNodeValue 被比较的节点名称下的值
     * @param file             XML文件
     * @return ture存在该节点
     */
    private boolean checkXmlNode(String refNodeName, String compareNodeValue, File file) {
        boolean result = false;
        try {
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document doc = builder.parse(file);   // 解析XML到内存中
            NodeList nodeList = doc.getElementsByTagName(refNodeName);
            for (int i = 0; i < nodeList.getLength(); i++) { //判断有没有该节点
                Node item = nodeList.item(i);
                String itemValue = item.getFirstChild().getNodeValue();
                if (itemValue.equals(compareNodeValue)) {
                    result = true;
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        Log.e(TAG, "checkXmlNode: 检查是否存在该节点　refNodeName=" + refNodeName + ",compareNodeValue=" + compareNodeValue + ",result=" + result);
        return result;
    }
```

###  ④ 若③中没有该节点，则在根节点familyset下的第一位置添加该节点并保存

```
    /**
     * 向system_fonts.xml文件增加一个节点
     *
     * @param nodeValue 节点的值
     */
    private void addSystemFontNote(String nodeValue) {
        try {
            File file = new File(system_fonts);
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document doc = builder.parse(file);   // 解析XML到内存中
            Node nodeFamilyset = doc.getElementsByTagName("familyset").item(0);
            Element elementFamily = doc.createElement("family");
            Element elementNameset = doc.createElement("nameset");
            Element elementName = doc.createElement("name");
            elementName.setTextContent(nodeValue);
            elementNameset.appendChild(elementName);//将name节点设置为nameset的子节点

            Element elementFileset = doc.createElement("fileset");
            Element elementFile = doc.createElement("file");
            elementFile.setTextContent(nodeValue + ".ttf");
            elementFileset.appendChild(elementFile);

            elementFamily.appendChild(elementNameset);
            elementFamily.appendChild(elementFileset);
            Node nodeFamily = doc.getElementsByTagName("family").item(0);
            nodeFamilyset.insertBefore(elementFamily, nodeFamily); //将节点elementFamily插入到节点nodeFamily之前
            //　保存到文件中
            saveXmlFile(doc, system_fonts);
            Log.e(TAG, "addSystemFontNote: 节点添加成功");
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
    /**
     * 保存xml文件到指定路径
     * @param doc      要保存的XML文档对象
     * @param destFile 目标路径
     * @throws TransformerException
     */
    private void saveXmlFile(Document doc, String destFile) throws TransformerException {
        Transformer transformer = TransformerFactory.newInstance().newTransformer();//创建一个用来转换DOM对象的工厂对象并获得转换器对象
        DOMSource domSource = new DOMSource(doc); //定义要转换的源对象
        File temFile = new File(getCacheDir(), "tem.xml");
        StreamResult streamResult = new StreamResult(temFile); //定义要转换到的目标文件
        transformer.transform(domSource, streamResult);  //开始转换  ,
//                runRootCommand("rm " + system_fonts);
        runRootCommand("cat " + temFile.getAbsolutePath() + " > " + destFile);//复制文件内容
        Log.e(TAG, "saveXmlFile: 保存文件成功，" + destFile);
    }

```

### ⑤ 完整的调用流程
```
    private String system_fonts = "/system/etc/system_fonts.xml";
    private String fallback_fonts = "/system/etc/fallback_fonts.xml";
    private String systemFontsDir = "/system/fonts/";//系统存放字体文件的路径
    public void ywsflsjtForSystem(View view) {
        String fontName = "ywsflsjt.ttf";
        runRootCommand("mount -o remount rw /system"); //重新挂载该路径为可读写模式
        //以下操作建议在线程中执行
        if (!checkFontTTFFile(fontName)) {
            copyFontTTF(fontName, systemFontsDir);
        }
        runRootCommand("chmod 777 " + new File(systemFontsDir, fontName).getAbsolutePath());//修改字体文件的权限
        String compareNodeValue = fontName.split("\\.")[0];
        boolean node = checkXmlNode("name", compareNodeValue, new File(system_fonts));
        if (node == false) { //没有该节点，在头部插入该节点
            addSystemFontNote(compareNodeValue);
            runRootCommand("chmod 777 " + system_fonts);
        }
        runRootCommand("reboot"); //要使修改后的字体生效需要重启系统
    }
```

### ⑥ 运行Root指令的方法
```
    /**
     * 请求ROOT权限后执行命令（最好开启一个线程）
     *
     * @param cmd (pm install -r *.apk)
     * @return
     */
    public boolean runRootCommand(String cmd) {
        Process process = null;
        DataOutputStream os = null;
        BufferedReader br = null;
        StringBuilder sb = null;
        try {
            process = Runtime.getRuntime().exec("su");
            os = new DataOutputStream(process.getOutputStream());
            os.writeBytes(cmd + "\n");
            os.writeBytes("exit\n");
            br = new BufferedReader(new InputStreamReader(
                    process.getInputStream()));

            sb = new StringBuilder();
            String temp = null;
            while ((temp = br.readLine()) != null) {
                sb.append(temp + "\n");
                if ("Success".equalsIgnoreCase(temp)) {
                    return true;
                }
            }
            process.waitFor();
        } catch (Exception e) {
            Log.e(TAG, "异常：" + e.getMessage());
        } finally {
            try {
                if (os != null) {
                    os.flush();
                    os.close();
                }
                if (br != null) {
                    br.close();
                }
                process.destroy();
            } catch (Exception e) {
                return false;
            }
        }
        return false;
    }
```


# 3、删除已添加的系统默认字体
和添加字体相对应，需要先删除字体文件，然后再删除 system_fonts.xml和fallback_fonts.xml两文件中的对应节点，由于我们没有修改过fallback_fonts.xml文件因此不需要做删除操作

### ① 删除节点是需要调用checkXmlNode方法坚持节点是否存在，若存在则删除数据
```
    /**
     * 删除 system_fonts.xml文件中的节点名称name值为 nodeValue的节点
     *
     * @param nodeValue
     */
    private void deleteSystemFontNote(String nodeValue) {
        Log.e(TAG, "deleteSystemFontNote: 删除文件" + system_fonts + "中的节点：" + nodeValue);
        try {
            File file = new File(system_fonts);
            DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
            DocumentBuilder builder = factory.newDocumentBuilder();
            Document doc = builder.parse(file);   // 解析XML到内存中
            NodeList nodeListName = doc.getElementsByTagName("name");
            for (int i = 0; i < nodeListName.getLength(); i++) {
                Node item = nodeListName.item(i);
                String value = item.getFirstChild().getNodeValue();
                if (value != null && nodeValue.equals(value)) {
                    Node nodenameset = item.getParentNode();
                    Node nodefamily = nodenameset.getParentNode();
                    Node nodefamilyset = nodefamily.getParentNode();
                    nodefamilyset.removeChild(nodefamily);
                    saveXmlFile(doc, system_fonts);
                    Log.e(TAG, "删除节点成功，nodeValue＝" + nodeValue);
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```

### ② 完整的删除流程

```
    public void deleteYwsflsjtForSystem(View view) {
        String fontName = "ywsflsjt.ttf";
        String nodeName = fontName.split("\\.")[0];
        runRootCommand("mount -o remount rw /system"); //重新挂载该路径为可读写模式
        runRootCommand("rm " + systemFontsDir + fontName); //删除字体文件
        if (checkXmlNode("name", nodeName, new File(system_fonts))) {
            deleteSystemFontNote(nodeName);
            runRootCommand("chmod 777 " + system_fonts);
        }
        runRootCommand("reboot"); //要使修改后的字体生效需要重启系统
    }
```


我的CSDN博客：http://blog.csdn.net/wo_ha/article/details/79202632