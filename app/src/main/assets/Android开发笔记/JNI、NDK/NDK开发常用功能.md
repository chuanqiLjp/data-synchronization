
# 1.LOG日志的打印
增加头文件和宏定义
```
#include <android/log.h>
#define LOG_TAG "System.out.c"
#define LOGD(...) __android_log_print(ANDROID_LOG_DEBUG, LOG_TAG, __VA_ARGS__)
#define LOGI(...) __android_log_print(ANDROID_LOG_INFO, LOG_TAG, __VA_ARGS__)
#define LOGW(...) __android_log_print(ANDROID_LOG_WARN, LOG_TAG, __VA_ARGS__)
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, LOG_TAG, __VA_ARGS__)
```
使用
```
    LOGD("TAGD,a=%d,b=%d",a,b);
    LOGI("TAGI,a=%d,b=%d",a,b);
    LOGW("TAGW,a=%d,b=%d",a,b);
    LOGE("TAGE,a=%d,b=%d",a,b);
```

# 2.C中调用Java的方法（利用反射）
在java中写好方法

```
package com.ljp.learnandroidadvanced;
import android.util.Log;
public class DataProvider {
    private static final String TAG = "DataProvider";
    //C调用java空方法
    public void helloFromJava(){
        Log.e(TAG, "helloFromJava: hello from java" );
​
    }
​
    //C调用java中的带两个int参数的方法
    public int Add(int x,int y){
        int result = x+y;
        Log.e(TAG, "Add: java result: " + result );
        return result;
    }
​
    //C调用java中参数为string的方法
    public void printString(String s){
        Log.e(TAG, "printString: java " + s);
    }
    //3.调用一个静态的java方法
    public static void printStaticStr(String s){
        Log.e(TAG, "printStaticStr: java static " + s);
    }
    //让c代码调用对应的java代码
    public native void callmethod1(); //C调用java空方法
    public native void callmethod2();//C调用java中的带两个int参数的方法
    public native void callmethod3(); //C调用java中参数为string的方法
    //调用一个静态的c代码
    public native void callmethod4();
}
````

在C中调用 步骤
```
extern "C"
JNIEXPORT void JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_call_1dp_1methodTest
        (JNIEnv *env, jobject obj) {
//     1.获取字节码对象
    jclass  clazz=env->FindClass("com/ljp/learnandroidadvanced/DataProvider");
//    2.获取Method对象
    jmethodID  methodId=env->GetMethodID(clazz,"getString","(Ljava/lang/String;)V");
//    3.通过字节码对象创建一个实例object（native方法和所调用的java代码不在同一个类中时才需要,否则调用方法时的jobject就使用obj）
    jobject  jobjInstance=env->AllocObject(clazz);
//    4.设置暴力破解
//    5.调用对象的方法
    env->CallVoidMethod(jobjInstance,methodId,env->NewStringUTF("Hello World !"));
}

#include <stdio.h>
#include <jni.h>
​
#include <android/log.h>
//include  D:\android-ndk-r7b\platforms\android-8\arch-arm\usr\include\android下的log.h这个目录
#define LOG_TAG "C_Out"
#define LOGD(...) __android_log_print(ANDROID_LOG_DEBUG, LOG_TAG, __VA_ARGS__)
#define LOGI(...) __android_log_print(ANDROID_LOG_INFO, LOG_TAG, __VA_ARGS__)
#define LOGE(...) __android_log_print(ANDROID_LOG_ERROR, LOG_TAG, __VA_ARGS__)
extern "C"
//C调用java空方法
JNIEXPORT void JNICALL Java_com_ljp_learnandroidadvanced_DataProvider_callmethod1
        (JNIEnv *env, jobject obj) {
​
    //在c代码里面调用java代码里面的方法
    // java 反射
    //1 . 找到java代码的 class文件
    //    jclass      (*FindClass)(JNIEnv*, const char*);
    jclass dpclazz = env->FindClass("com/ljp/learnandroidadvanced/DataProvider");
    if (dpclazz == 0) {
        LOGE("find class error");
        return;
    }
​
    LOGE("find class ...");
​
//2 寻找class里面的方法
//   jmethodID   (*GetMethodID)(JNIEnv*, jclass, const char*, const char*)
    jmethodID method1 = env->GetMethodID(dpclazz, "helloFromJava", "()V");
    if (method1 == 0) {
        LOGE("find method1 error..");
        return;
    }
​
    LOGE("find method1 ...");
​
//3 .调用这个方法
//    void        (*CallVoidMethod)(JNIEnv*, jobject, jmethodID, ...);
    env->CallVoidMethod(obj, method1);
}
extern "C"
//C调用java中的带两个int参数的方法
JNIEXPORT void JNICALL Java_com_ljp_learnandroidadvanced_DataProvider_callmethod2
        (JNIEnv *env, jobject obj) {
​
    jclass dpclazz = env->FindClass("com/ljp/learnandroidadvanced/DataProvider");
    if (dpclazz == 0) {
        LOGE("find class error");
        return;
    }
​
    LOGE("find class ...");
​
    jmethodID method2 = env->GetMethodID(dpclazz, "Add", "(II)I");
    if (method2 == 0) {
        LOGE("find method2 error..");
        return;
    }
​
    LOGE("find method2 ...");
​
// 3 调用这个方法
//    jint        (*CallIntMethod)(JNIEnv*, jobject, jmethodID, ...);
    int result = env->CallIntMethod(obj, method2, 3, 5);
    LOGE("c code Result = %d", result);
​
}
extern "C"
//C调用java中参数为string的方法
JNIEXPORT void JNICALL Java_com_ljp_learnandroidadvanced_DataProvider_callmethod3
        (JNIEnv *env, jobject obj) {
    jclass dpclazz = env->FindClass("com/ljp/learnandroidadvanced/DataProvider");
    if (dpclazz == 0) {
        LOGE("find class error");
        return;
    }
​
    LOGE("find class ...");
​
    jmethodID method3 = env->GetMethodID(dpclazz, "printString", "(Ljava/lang/String;)V");
    if (method3 == 0) {
        LOGE("find method3 error..");
        return;
    }
​
    LOGE("find method3 ...");
​
    env->CallVoidMethod(obj, method3, env->NewStringUTF("hello in c"));
}
extern "C"
//3.调用一个静态的java方法
JNIEXPORT void JNICALL Java_com_ljp_learnandroidadvanced_DataProvider_callmethod4
        (JNIEnv *env, jobject obj) {
​
    jclass dpclazz = env->FindClass("com/ljp/learnandroidadvanced/DataProvider");
    if (dpclazz == 0) {
        LOGE("find class error");
        return;
    }
​
    LOGE("find class ...");
​
//2 寻找class里面的方法
//   jmethodID   (*GetMethodID)(JNIEnv*, jclass, const char*, const char*);
// 注意 :如果要寻找的方法是静态的方法 那就不能直接去获取methodid
//jmethodID method4 = (*env)->GetMethodID(env,dpclazz,"printStaticStr","(Ljava/lang/String;)V");
//    jmethodID   (*GetStaticMethodID)(JNIEnv*, jclass, const char*, const char*);
    jmethodID method4 = env->GetStaticMethodID(dpclazz, "printStaticStr", "(Ljava/lang/String;)V");
    if (method4 == 0) {
        LOGE("find method4 error..");
        return;
    }
​
    LOGE("find method4 ...");
​
//3.调用一个静态的java方法
//    void        (*CallStaticVoidMethod)(JNIEnv*, jclass, jmethodID, ...);
    env->CallStaticVoidMethod(dpclazz, method4, env->NewStringUTF("static haha in c"));
}
extern "C"
JNIEXPORT void JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_call_1dp_1method1
        (JNIEnv *env, jobject obj) {
​
    jclass dpclazz = env->FindClass("com/ljp/learnandroidadvanced/DataProvider");
    if (dpclazz == 0) {
        LOGE("find class error");
        return;
    }
​
    LOGE("find class ...");
​
    jmethodID method5 = env->GetStaticMethodID(dpclazz, "printStaticStr", "(Ljava/lang/String;)V");
    if (method5 == 0) {
        LOGE("find method5 error..");
        return;
    }
​
    LOGE("find method5 ...");
​
//3 .调用这个方法(用于解决native方法和所调用的java代码不在同一个类中的情况..)
//    void        (*CallVoidMethod)(JNIEnv*, jobject, jmethodID, ...);
//    jobject     (*NewObject)(JNIEnv*, jclass, jmethodID, ...);
//  jobject     (*AllocObject)(JNIEnv*, jclass);
    jobject dpobject = env->AllocObject(dpclazz);
    env->CallStaticVoidMethod(dpclazz, method5,
                              env->NewStringUTF("not int a class ,static hello jni"));
}
```

# 3.使用javap获取方法的签名
先进入到字节码文件所在目录cd C:\Android\AD\LearnAndroidAdvanced\app\build\intermediates\classes\debug，然后javap -s com.ljp.learnandroidadvanced.MainActivity

# 4.查看C语言类型中的类型所占的字节
 sizeof 计算的是字符串的实际长度，包含\0
 strlen 计算的是字符串的实际有效长度，不包含\0
 ```
    LOGE("bool-----size=%d (byte)", sizeof(bool)); //  size=1
    LOGE("char-----size=%d (byte)", sizeof(char)); //  size=1
    LOGE("unsigned char-----size=%d (byte)", sizeof(unsigned char));//size=1
    LOGE("signed char-----size=%d (byte)", sizeof(signed char));// size=1
    LOGE("short-----size=%d (byte)", sizeof(short));// size=2
    LOGE("int-----size=%d (byte)", sizeof(int));// size=4
    LOGE("long-----size=%d (byte)", sizeof(long));//size=4
    LOGE("float-----size=%d (byte)", sizeof(float));// size=4
    LOGE("double-----size=%d (byte)", sizeof(double));// size=8
    LOGE("long double-----size=%d (byte)", sizeof(long double));// size=10
    LOGE("string0-----size=%d (byte)", strlen("12345"));// size=5 ,  #include "string"
    LOGE("string1-----size=%d (byte)", sizeof("12345"));// size=6
    char s[] = "12345\0ABCDE";
    LOGE("string2-----size=%d (byte)", sizeof(s));// size=12     sizeof 计算的是字符串的实际长度，包含\0
    LOGE("string3-----size=%d (byte)", strlen(s));// size=6     strlen 计算的是字符串的实际有效长度，不包含\0
```

# 5.C如何在一个文件里调用另一个源文件中的函数
当程序大了代码多了之后，想模块化开发，不同文件中存一点，是很好的解决办法，那我们如何做才能让各个文件中的代码协同工作呢？我们知道，main函数是程序入口，我们希望把不同的功能写在不同的函数中，并把这些函数统一放到另外一个文件里，以便main函数显得太长，main函数可以在用到某方法的时候调用来处理。为了实现这个步骤，我们这样做。首先定义一个c代码的头文件，如function.h,在里面声明将要实现的函数，如int add（int a，int b）; ，然后新建一个源文件为function.c,在function.c的开头#include "function.h",然后下面写头文件中已声明的函数的实现。这样写完了之后，main函数如果要调用这个源文件中的函数，只需要在main函数的开头部分加入#include，如此这般，main函数调用相应函数的时候就会自动找到程序的实现部分代码了。
文件    function.h
```
# include<stdio.h>
int add(int a,int b);
```
文件    function.c
```
 #include<function.h>
 int add(int a,int b){
    return a+b;
 }
 ```
文件    main.c
```
 # include<stdio.h>
 # include<function.h>
 int main() {
    int a = 1,b =2;
    int c = add(a,b);   //这里是对function.c中的add函数的调用
    printf("c=%d",c);
    return 0;
 }
```
# 6.C语言的字符串与Java的字符串的互相转换
```
JNIEXPORT jstring JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_getStr
        (JNIEnv *env, jobject jobject1, jstring word) {
​
    //将C中的字符串转换为Java中的字符串
    char result[] = "hello world from cpp";
    jstring jResult = env->NewStringUTF(result);

    //方法一：将Java中的字符串转换为C语言中的char*
    const char *p_word = env->GetStringUTFChars(word, NULL);
    LOGE("----------------1word=%s", p_word);
    env->ReleaseStringUTFChars(word, p_word);//回收，防止内存泄漏
​
    //方法二：将Java中的字符串转换为C语言中的char数组
    int wordSize=env->GetStringLength(word); //得到字符串的长度
    char c_word[wordSize];
    env->GetStringUTFRegion(word,0,wordSize,c_word); //把jstring数据word进行一个一个的拷贝，然后装到上面创建的字符串数组str里面去。从下标为0 的地方开始装。装 wordSize 个。就是把字符串拷贝到c语言空间的数组里面
    LOGE("----------------2word=%s",c_word);
​
    return jResult;
}
jstring转换为char*使用JNIEnv的const char*  GetStringUTFChars(JNIEnv*, jstring, jboolean*)
​
JNIEnv env=//传入参数 ;  jstring name=//传入参数 ;
​
const char *nameStr=(*env)->GetStringUTFChars(env,name,NULL);

​
调用完GetStringUTFChars后必须调用JNIEnv的void ReleaseStringUTFChars(JNIEnv*, jstring, const char*)释放新建的字符串。
​
(*env)-> ReleaseStringUTFChars(env,name, nameStr);

​
char*转换为jstring使用JNIEnv的jstring  NewStringUTF(JNIEnv*, const char*);
​
jstring newArgName=(*env)->NewStringUTF(env, nameStr);

​
调用完NewStringUTF后必须调用JNIEnv的void DeleteLocalRef(JNIEnv*, jobject);释放新建的jstring。
​
(*env)-> DeleteLocalRef(env, newArgName);
```
# 7.C语言中的常见字符串的操作
```
//    1.字符串的连接——strcat（字符数组1，字符数组2）：函数调用后得到的是字符数组1的地址，因此字符数组1必须足够大，以便能容纳连接后的新字符串，故char str1[]定义会有问题
    char str1[100] = "I am the Hello.";
    char str2[] = "I am the Word.";
    char *str_new = strcat(str1, str2);
    LOGE("字符串的连接——》%s", str_new);
​
//    2.字符串复制——strcpy(字符串数组1，字符串数组2) ：将字符串2复制到字符串数组1中，字符串1的长度不能小于字符串2的长度
    char str3[20] = "123456789012345678";
    char str4[] = "Hello world!";
    strcpy(str3, str4);
    LOGE("字符串复制——》%s", str3);
​
//    3.字符串复制前Ｎ个字符——strncpy(str1,str2,num) : 将str2的前num个字符复制到str1中替换str1中原有的最前面的num个字符,慎重使用很容易出错
    char str5[5] = "1234";
    char str6[] = "Hello world!";
    strncpy(str5, str6, 4);
    LOGE("字符串复制前Ｎ个字符——》%s", str5);
​
//    4.字符串比较——strcmp(str1,str2) ：将str1与str2自左至右逐个字符按ASCII码值比较，直到出现不同字符或遇到\0为止，函数值：0：两字符串相同；正整数：str1>str2 ；负整数：str1
    char str7[] = "0123456";
    char str8[] = "01234568";
    int cmpResult = strcmp(str7, str8);
    LOGE("字符串比较——》%d", cmpResult);
​
//    5.计算字符串的长度——strlen(str) ：计算字符串的实际长度，不包含'\0';
    char str9[10] = "123456\0";
    LOGE("计算字符串的长度——》strlen=%d,sizeof=%d", strlen(str9), sizeof(str9));//strlen=6,sizeof=10
​
//    6.将字符串转换为小写
    char str10[20] = "AbCDEfgXyZ";
    int len_str10 = strlen(str10);
    for (int i = 0; i < len_str10; ++i) {
        str10[i] = (str10[i] >= 65 && str10[i] <= 90) ? (str10[i] + 32) : str10[i];
    }
    LOGE("将字符串转换为小写——》%s", str10);
​
//    7.将字符串转换为大写
    char str11[20] = "abCDEfgXyz";
    int len_str11 = strlen(str11);
    for (int i = 0; i < len_str11; ++i) {
        str11[i] = (str11[i] >= 97 && str11[i] <= 122) ? (str11[i] - 32) : str11[i];
    }
    LOGE("将字符串转换为大写——》%s", str11);
```
# 8.C语言的文件操作

```
//向文件中写字符串
extern "C"
JNIEXPORT jint JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_writeFileStr
        (JNIEnv *env, jobject obj, jstring desPath, jstring content) {
    LOGE("向文件中写字符串");
    int len = env->GetStringLength(content); //得到要写入文件内容的长度
    char c_content[len + 1];
    env->GetStringUTFRegion(content, 0, len, c_content);//将Java的字符串转换为C语言的字符串
    c_content[len] = '\0';//防止后面有脏数据
    LOGE("size=%d,content=%s", len, c_content);
    const char *p_desPath = env->GetStringUTFChars(desPath, NULL); //将Java的字符串转转为char*
    /**
r 打开只读文件，该文件必须存在。
r+ 打开可读写的文件，该文件必须存在。
w 打开只写文件，若文件存在则文件长度清为0，即该文件内容会消失。若文件不存在则建立该文件。
w+ 打开可读写文件，若文件存在则文件长度清为零，即该文件内容会消失。若文件不存在则建立该文件。
a 以附加的方式打开只写文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾，即文件原先的内容会被保留。
a+ 以附加方式打开可读写的文件。若文件不存在，则会建立该文件，如果文件存在，写入的数据会被加到文件尾后，即文件原先的内容会被保留。
     */
    FILE *stream = fopen(p_desPath, "w+");//打开文件   (文件的路径，打开文件的模式)
    int result = fputs(c_content, stream);//将字符串写入文件
    fclose(stream);
​
    FILE *fd = fopen(p_desPath, "a+");
    for (int i = 0; i < len; ++i) {
        fputc(c_content[i], fd); //每次写入一个字符到文件
    }
    fclose(fd);
    return result;
}
​
//向文件中写原始字节
extern "C"
JNIEXPORT jint JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_writeFileByte
        (JNIEnv *env, jobject obj, jstring desPath, jbyteArray content) {
    const char *p_desPath = env->GetStringUTFChars(desPath, NULL);
    int len = env->GetArrayLength(content);
    LOGE("desPath=%s,len=%d", p_desPath, len);
    jbyte *p_contentB = env->GetByteArrayElements(content, NULL); //将Java的Byte数组转换为 C语言的signed char
​
    FILE *stream = fopen(p_desPath, "wb+");//打开文件   (文件的路径，打开文件的模式)
    for (int i = 0; i < len; ++i) {
        fputc(p_contentB[i], stream);
    }
    fclose(stream);
​
    FILE *fd = fopen(p_desPath, "w");
    /**
     * 定义函数 size_t fwrite(const void * ptr,size_t size,size_t nmemb,FILE * stream);
     * 函数说明 fwrite()用来将数据写入文件流中。
     * 参数ptr 指向欲写入的数据地址，
     * 总共写入的字符数以参数size*nmemb来决定。
     * Fwrite()会返回实际写入的nmemb数目。参数stream为已打开的文件指针，
     * 返回值 返回实际写入的nmemb数目。
     */
    int result = fwrite(p_contentB, len, 1, fd);  //实际写入的长度=(内容地址，读写的字节数size，要读写多少个size的字节数，文件指针)
    fclose(fd);
​
    return result;
}
​
//读取文件的字符串
extern "C"
JNIEXPORT jstring JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_readFileStr
        (JNIEnv *env, jobject obj, jstring desPath) {
    const char *p_desPath = env->GetStringUTFChars(desPath, NULL); //将Java的字符串转转为char*
    char *p_result;
​
    FILE *fd = fopen(p_desPath, "r");
    fgets(p_result, 10, fd);//读取文件中的10个字符串,读取的数量不能超过文件的实际字符数，否则会报错
    fclose(fd);
​
//    FILE *fd2 = fopen(p_desPath, "r");
//    char a;
//    int count = 0;
//    LOGE("文件%s的长度为%d", p_desPath, count);
//    while ((a = fgetc(fd2)) != EOF) {
//        count++;
//    }
//    LOGE("文件%s的长度为%d", p_desPath, count);
//    p_result = (char *) malloc(sizeof(char) * (count + 1));
//    memcpy(p_result, fd2, sizeof(char) * (count + 1));   //由文件向动态内存中拷贝；
//    p_result[count] = '\0';
​
    LOGE("file content=%s", p_result);
    return env->NewStringUTF("1234567890");
}
​
//读取文件的原始字节
extern "C"
JNIEXPORT jbyteArray JNICALL Java_com_ljp_learnandroidadvanced_MainActivity_readFileByte
        (JNIEnv *env, jobject obj, jstring desPath) {
    const char *p_desPath = env->GetStringUTFChars(desPath, NULL); //将Java的字符串转转为char*
    FILE *fd1 = fopen(p_desPath, "rb");
​
    jbyteArray result = env->NewByteArray(1);
    return result;
}
```
# 9.Android系统目前支持的CPU架构
```
ARMv5，ARMv7 (从2010年起)
x86 (从2011年起)
MIPS (从2012年起)
ARMv8，MIPS64和x86_64 (从2014年起)
```

每一个CPU架构对应一个ABI

```
CPU架构           ABI
ARMv5   --->    armeabi
ARMv7   --->    armeabi-v7a
x86     --->    x86
MIPS    --->    mips
ARMv8   --->    arm64-v8a
MIPS64  --->    mips64
x86_64  --->    x86_64

armeabi：默认选项，将创建以基于ARM* v5TE 的设备为目标的库。 具有这种目标
的浮点运算使用软件浮点运算。 使用此ABI（二进制接口）创建的二进制代码将可以
在所有 ARM*设备上运行。所以armeabi通用性很强。但是速度慢
​
armeabi-v7a：创建支持基于ARM* v7 的设备的库，并将使用硬件FPU指令。
armeabi-v7a是针对有浮点运算或高级扩展功能的arm v7 cpu。
​
mips：MIPS是世界上很流行的一种RISC处理器。MIPS的意思是“无内部互锁流水级
的微处理器”(Microprocessor without interlocked piped stages)，其机
制是尽量利用软件办法避免流水线中的数据相关问题。
​
x86：支持基于硬件的浮点运算的IA-32 指令集。x86是可以兼容armeabi平台运行
的，无论是armeabi-v7a还是armeabi，同时带来的也是性能上的损耗，另外需要
指出的是，打包出的x86的so，总会比armeabi平台的体积更小。
```

总结:
```
如果项目只包含了 armeabi，那么在所有Android设备都可以运行；
如果项目只包含了 armeabi-v7a，除armeabi架构的设备外都可以运行；
如果项目只包含了 x86，那么armeabi架构和armeabi-v7a的Android设备是无法
运行的；
如果同时包含了 armeabi，armeabi-v7a和x86，所有设备都可以运行，程序在运
行的时候去加载不同平台对应的so，这是较为完美的一种解决方案，同时也会导致
包变大。
```

NDK更多学习
麦子学院的文档：http://www.maiziedu.com/wiki/component/switch/
C语言的文件操作：http://www.cnblogs.com/likebeta/archive/2012/06/16/2551780.html
           http://www.jb51.net/article/37688.htm

