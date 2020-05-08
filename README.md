## 一、Git

### 1. 用户                

#### 1.1 用户切换（https方式）

需要用其他git用户登陆clone代码的时候，可以通过修改git配置中的用户和邮箱来切换用户：

```js
git config --global user.name "YOURUSERNAME" 
git config --global user.email "YOUREMAIL"
```

尝试切换之后却一直没能成功，一直用的是原来的git用户来访问，猜测应该是有其他的默认验证方式，查阅了一下原来windows上有凭据工具叫做**windows凭据**，git使用这个凭据对应的属性是credential.helper，我们设置这个值为：

```js
git config credential.helper manager
```

manager指的就是windows凭据工具，在git需要账户密码的时候，会先从指定的凭据管理器中查找凭据，如果查找不到，会弹框要求输入并记录，第二次访问的时候就可以使用相应的凭据，所以在切换账户之后要记得清理本地缓存（这里对应的就是缓存凭据）。

Refer to: [git多用户切换](https://blog.csdn.net/lqlqlq007/article/details/80613272?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-1)

## 二、Android

### 1. build

#### 1.1 troubleshooting

- build pjsid的时候一直报错Could not resolve all files for configuration ':classpath'![can not resolve classpath](C:\Users\jenny.xu\Desktop\can not resolve classpath.png)解决方案是把buildgradle里面的maven源全部换成阿里源。

```
// Make sure you have gradle 2.14.1 ,if you don't run following commands
// curl -s "https://get.sdkman.io" | bash
// source "$HOME/.sdkman/bin/sdkman-init.sh"
// sdk install gradle 2.14.1


buildscript {
    repositories {
        // jcenter()
        // mavenCentral()
        maven {
            url 'http://maven.aliyun.com/nexus/content/repositories/google'
        }
        maven {
            url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'
        }
        maven {
            url 'https://maven.fabric.io/public'
        }
        // google()
    }

    dependencies {
        classpath 'com.android.tools.build:gradle:3.1.0'
        classpath 'com.github.dcendents:android-maven-gradle-plugin:1.4.1'
    }
}

apply plugin: 'com.android.library'
apply plugin: 'com.github.dcendents.android-maven'


version = "2.3.3" // Note: The last number is the PSS-specific number. Increase it when new fix/customization is made.
group = "com.patientsafesolutions.apjsip"

android {
    compileSdkVersion 26

    defaultConfig {
        minSdkVersion 24
        targetSdkVersion 26
        versionCode 1
        versionName = version
    }

     lintOptions {
          abortOnError false
      }

    sourceSets {
          main {
            manifest.srcFile 'AndroidManifest.xml'
            java.srcDir 'src'
            resources.srcDir 'src'
            aidl.srcDir 'src'
            renderscript.srcDir 'src'
            res.srcDir 'res'
            assets.srcDir 'assets'
            jniLibs.srcDir 'libs'
          }
    }
}

repositories {
    // google()
    // jcenter()
    maven {
        url 'http://maven.aliyun.com/nexus/content/repositories/google'
    }
    maven {
        url 'http://maven.aliyun.com/nexus/content/repositories/jcenter'
    }
    maven {
        url 'https://maven.fabric.io/public'
    }
}

```

## 三、Maven

### 1.1 简介

Maven是一个软件项目管理和综合工具，在我目前用的范围就是一个包管理工具，类似于npm，但其实它还可以完成项目的构建配置工作。

  Maven提供了开发人员管理：

  - Builds
  - Documentation
  - Reporting
  - SCMs
  - Releases
  - Distribution
  - mailing list

概括的说，maven标准化了项目构建，简化了项目建设过程。

### 1.2 Maven仓库简介和搜索顺序

对开发者来说，maven有三大仓库：

1. 本地资源库
2. 中央存储库
3. 远程存储库

1.2概念很容易理解，远程存储库就是有些在中央存储库缺失的，可以在项目中配置url查询，pom文件中写明的依赖，maven会按照1、2、3依次查找

#### 1.2.1 远程存储库的配置方法

在pom.xml文件中配置：

```
 <repositories>
 	<repository>
 		<id>java.net</id>
 		<url>https://maven.java.net/content/repositories/public/</url>
 	</repository>
 </repositories>
```

#### 1.2.2 如何定制库到Maven本地资源

有两种情况需要自定义库到本地资源：

1. 要使用的jar不存在于Maven的中心存储库中，例如一些流行库
2. 自己创建了一个jar，而另一个maven项目需要使用

一个例子：

例如kaptcha，它是一个很流行的第三方java库，被用于生成“验证码图片”，但不在中心库中，我们要做的：

1. mvn安装

   下载 “kaptcha”，将其解压缩并将 kaptcha-version.jar 复制到其他地方，比如：C盘。发出下面的命令：

   ```
   mvn install:install-file -Dfile=c:\kaptcha-{version}.jar -DgroupId=com.google.code -DartifactId=kaptcha -Dversion={version} -Dpackaging=jar
   ```

2. pom.xml

   安装完毕后，就在 pom.xml 中声明 kaptcha 的坐标。

   ```xml
   <dependency>
         <groupId>com.google.code</groupId>
         <artifactId>kaptcha</artifactId>
         <version>2.3</version>
    </dependency>
   ```

