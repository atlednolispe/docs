Android Studio第一次配置
========================

```
# /home/atlednolispe/AndroidStudioProjects/HelloWorld/build.gradle
#
#
#
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    
    repositories {
        google()
//        mavenCentral()
        maven { url 'http://maven.google.com' }
//        maven { url 'http://maven.aliyun.com/nexus/content/groups/public/' }  // 这里用ali是第一次成功的
        jcenter()
    }
//    repositories {
//        jcenter()
//        mavenCentral()
//        maven { url 'http://maven.aliyun.com/nexus/content/repositories/releases/' }
//        google()
//    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.0.0'  // 好像是必须和studio版本一致,使用studio3.0.0, SDK7.28
        

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        google()
        jcenter()
//        maven { url 'http://maven.google.com' }
        maven {url 'https://dl.bintray.com/jetbrains/anko'}  // 这个是解决Could not resolve all files for configuration ':app:debugCompileClasspath'.
        maven {url "https://maven.google.com"}
//        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
//        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```
