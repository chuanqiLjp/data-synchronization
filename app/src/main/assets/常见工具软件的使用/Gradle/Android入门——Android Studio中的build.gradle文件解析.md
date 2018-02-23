#首先看工程中根路径下的build.gradle文件
```
apply plugin: 'com.android.application' //表示是一个应用程序的模块，可独立运行
//apply plugin: 'com.android.library' //表示是一个依赖库，不能独立运行
android {
    compileSdkVersion 25   //指定项目的编译版本
    buildToolsVersion "25.0.1"//指定项目构建工具的版本
    defaultConfig {
        applicationId "com.hhqy.learnndk2" //指定包名
        minSdkVersion 14//指定最低的兼容的Android系统版本
        targetSdkVersion 25//指定你的目标版本，表示你在该Android系统版本已经做过充分的测试
        versionCode 1   //版本号
        versionName "1.0"   //版本名称
    }
    buildTypes { //指定生成安装文件的配置，常有两个子包:release,debug，注：直接运行的都是debug安装文件
        release { //用于指定生成正式版安装文件的配置
            minifyEnabled false     //指定是否对代码进行混淆，true表示混淆
            //指定混淆时使用的规则文件，proguard-android.txt指所有项目通用的混淆规则，proguard-rules.pro当前项目特有的混淆规则
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
dependencies { //指定当前项目的所有依赖关系：本地依赖、库依赖、远程依赖
    compile fileTree(dir: 'libs', include: ['*.jar'])//本地依赖
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:25.0.1'//远程依赖，com.android.support是域名部分，appcompat-v7是组名称，25.0.1是版本号
    compile project(':hello')//库依赖
    testCompile 'junit:junit:4.12'  //声明测试用列库
    compile 'com.android.support.constraint:constraint-layout:1.0.0-alpha7'
}
```
#某个Mode中的build.gradle文件
```
apply plugin: 'com.android.application' //表示是一个应用程序的模块，可独立运行
//apply plugin: 'com.android.library' //表示是一个依赖库，不能独立运行
android {
    compileSdkVersion 25   //指定项目的编译版本
    buildToolsVersion "25.0.1"//指定项目构建工具的版本
    defaultConfig {
        applicationId "com.hhqy.learnndk2" //指定包名
        minSdkVersion 14//指定最低的兼容的Android系统版本
        targetSdkVersion 25//指定你的目标版本，表示你在该Android系统版本已经做过充分的测试
        versionCode 1   //版本号
        versionName "1.0"   //版本名称
    }
    buildTypes { //指定生成安装文件的配置，常有两个子包:release,debug，注：直接运行的都是debug安装文件
        release { //用于指定生成正式版安装文件的配置
            minifyEnabled false     //指定是否对代码进行混淆，true表示混淆
            //指定混淆时使用的规则文件，proguard-android.txt指所有项目通用的混淆规则，proguard-rules.pro当前项目特有的混淆规则
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }
}
dependencies { //指定当前项目的所有依赖关系：本地依赖、库依赖、远程依赖
    compile fileTree(dir: 'libs', include: ['*.jar'])//本地依赖
    androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    compile 'com.android.support:appcompat-v7:25.0.1'//远程依赖，com.android.support是域名部分，appcompat-v7是组名称，25.0.1是版本号
    compile project(':hello')//库依赖
    testCompile 'junit:junit:4.12'  //声明测试用列库
    compile 'com.android.support.constraint:constraint-layout:1.0.0-alpha7'
}
```
#CSDN博客地址：http://blog.csdn.net/wo_ha/article/details/54095190