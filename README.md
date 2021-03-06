## Gradle依赖统一管理

项目根目录新建config.gradle文件，配置参考如下：
```gradle
def supportVersion = "23.4.0"
def rxBindingVersion = "0.4.0"
def greenDAOVersion = "2.2.1"
def retrofitVersion = "2.1.0"
def stethoVersion = "1.3.1"
def butterknifeVersion = "8.2.1"
def leakCanaryVersion = "1.4-beta2"
def dagger2Version = "2.5"

ext {

    android = [
            compileSdkVersion: 23,
            buildToolsVersion: "23.0.3",
            applicationId    : "your package name",
            minSdkVersion    : 14,
            targetSdkVersion : 23,
            versionCode      : 1,
            versionName      : "1.0"
    ]

    //官方库
    supportV4 = "com.android.support:support-v4:${supportVersion}"
    supportAppcompatV7 = "com.android.support:appcompat-v7:${supportVersion}"
    supportDesign = "com.android.support:design:${supportVersion}"
    supportCardView = "com.android.support:cardview-v7:${supportVersion}"
    supportRecyclerView = "com.android.support:recyclerview-v7:${supportVersion}"
    supportGridLayout = "com.android.support:gridlayout-v7:${supportVersion}"
    supportAnnotations = "com.android.support:support-annotations:${supportVersion}"
    supportMultidex = "com.android.support:multidex:1.0.+"

    //基础项目
    basicProject = "com.classic.core:classic:2.1"

    //动画
    nineoldandroids = "com.nineoldandroids:library:2.4.0"

    //图片加载
    glide = "com.github.bumptech.glide:glide:3.7.0"
    fresco = "com.facebook.fresco:fresco:0.10.+"
    picasso = "com.squareup.picasso:picasso:2.5.2"

    //json解析
    fastjson = "com.alibaba:fastjson:1.2.12"
    fastjsonAndroid = "com.alibaba:fastjson:1.1.52.android"

    //view注入
    dagger = "com.squareup.dagger:dagger:1.2.5"
    butterknife = "com.jakewharton:butterknife:${butterknifeVersion}"
    butterknifeCompiler = "com.jakewharton:butterknife-compiler:${butterknifeVersion}"

    //Rx家族，响应式编程
    rxJava = "io.reactivex:rxjava:1.1.7"
    rxAndroid = "io.reactivex:rxandroid:1.2.1"
    rxBinding = "com.jakewharton.rxbinding:rxbinding:${rxBindingVersion}"
    rxBindingSupportV4 = "com.jakewharton.rxbinding:rxbinding-support-v4:${rxBindingVersion}"
    rxBindingSupportAppcompatV7 = "com.jakewharton.rxbinding:rxbinding-appcompat-v7:${rxBindingVersion}"
    rxBindingSupportDesign = "com.jakewharton.rxbinding:rxbinding-design:${rxBindingVersion}"
    rxBindingSupportRecyclerView = "com.jakewharton.rxbinding:rxbinding-recyclerview-v7:${rxBindingVersion}"
    rxBindingLeanbackV17 = "com.jakewharton.rxbinding:rxbinding-leanback-v17:${rxBindingVersion}"

    //google开源的异步框架
    agera = "com.google.android.agera:agera:1.1.0"

    //网络请求
    retrofit = "com.squareup.retrofit2:retrofit:${retrofitVersion}"
    gsonForRetrofit = "com.squareup.retrofit2:converter-gson:${retrofitVersion}"
    rxJavaForRetrofit = "com.squareup.retrofit2:adapter-rxjava:${retrofitVersion}"
    okhttp = "com.squareup.okhttp3:okhttp:3.4.1"

    //facebook出品的网络调试神器
    stetho = "com.facebook.stetho:stetho:${stethoVersion}"
    stethoOkhttp = "com.facebook.stetho:stetho-okhttp3:${stethoVersion}"
    stethoUrlConnection = "com.facebook.stetho:stetho-urlconnection:${stethoVersion}"
    stethoJsRhino = "com.facebook.stetho:stetho-js-rhino:${stethoVersion}"

    //基于LRU的磁盘缓存
    diskLruCache = "com.jakewharton:disklrucache:2.0.2"

    //数据库
    sqlbrite = "com.squareup.sqlbrite:sqlbrite:0.7.0"
    greenDAO = "de.greenrobot:greendao:${greenDAOVersion}"
    greenDAOGenerator = "de.greenrobot:greendao-generator:2.2.0"

    //二维码扫描
    zxing = "com.google.zxing:core:3.2.1"

    //Material Design向下兼容库(Android 2.2+)
    carbon = "tk.zielony:carbon:0.13.0"
    //通用适配器
    commonAdapter = "com.classic.adapter:commonadapter:1.2"
    //方便的切换到：加载中视图、错误视图、空数据视图、网络异常视图、内容视图。
    mutipleStatusView = "com.classic.common:multiple-status-view:1.2"

    //检测内存泄漏
    leakCanaryDebug = "com.squareup.leakcanary:leakcanary-android:${leakCanaryVersion}"
    leakCanaryRelease = "com.squareup.leakcanary:leakcanary-android-no-op:${leakCanaryVersion}"
    //检测UI卡顿
    blockCanary = "com.github.moduth:blockcanary-android:1.2.1"

}
```

然后在项目根目录的build.gradle文件中添加配置文件的引用
```gradle
//应用配置文件
apply from: "config.gradle"

buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.5.0'
    }
}

allprojects {
    repositories {
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

开始使用，在module的build.gradle文件中直接引用
```gradle
apply plugin: 'com.android.application'

android {
    compileSdkVersion rootProject.ext.android.compileSdkVersion
    buildToolsVersion rootProject.ext.android.buildToolsVersion

    defaultConfig {
        applicationId rootProject.ext.android.applicationId
        minSdkVersion rootProject.ext.android.minSdkVersion
        targetSdkVersion rootProject.ext.android.targetSdkVersion
        versionCode rootProject.ext.android.versionCode
        versionName rootProject.ext.android.versionName
    }
    ...
}

dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile rootProject.ext.supportV4
    compile rootProject.ext.supportAppcompatV7

    compile rootProject.ext.okhttp
    compile rootProject.ext.rxjava
    compile rootProject.ext.glide
    compile rootProject.ext.retrofit
    ...
}
```

## 参考资料
[Gradle依赖的统一管理 stormzhang](http://mp.weixin.qq.com/s?__biz=MzA4NTQwNDcyMA==&mid=402733201&idx=1&sn=052e12818fe937e28ef08331535a179e#rd)

