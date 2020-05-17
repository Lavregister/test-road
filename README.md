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

Refer to: [Maven教程](https://www.yiibai.com/maven)

## 四、Spring Boot

### 1.1 简介

### 1.2 第一个SpringBoot程序

#### 1.2.1 创建项目

用IDEA创建一个springboot程序：File -> New -> Project -> Spring Assistant.

- troubleshooting:

如果找不到Spring Assistant，在File -> Setting -> Plugins 搜索 Spring Assistant.

我在这个步骤中安装了还是没有显示Spring Assistant选项，猜想IDEA版本太老，重装了最新版就ok了。

#### 1.2.2 编写一个Rest端点

在Spring Boot主类添加一个Hello World Rest端点：

1. 在类顶部添加 @RestController 注释
2. 使用 @RequestMapping 注释编写Request URI方法，这个方法返回Hello World字符串
3. 类文件如下：

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@SpringBootApplication
@RestController
public class SptingBootTestApplication {

	public static void main(String[] args) {
		System.out.println("HelloWorld!");
		SpringApplication.run(SptingBootTestApplication.class, args);
	}

	@RequestMapping(value = "/")
	public String hello() {
		return "Hello World";
	}

}
```

#### 1.2.3 创建一个可执行的JAR

在项目根目录使用maven命令 mvn clean install打包构建，这个命令的背后处理了（按顺序）：

1. 使用清理插件：maven-clean-plugin:2.5 执行清理删除已有target目录
2. 使用资源插件：maven-resources-plugin:2.6 执行资源文件的处理
3. 使用编译插件：maven-compiler-plugin:3.1编译所有源文件生成class文件至target\classes目录下
4. 使用资源插件：maven-resources-plugin:2.6执行测试资源文件的处理
5. 使用编译插件：maven-compiler-plugin:3.1编译测试目录下的所有源代码
6. 使用插件：maven-surefire-plugin:2.12运行测试用例
7. 使用插件：maven-jar-plugin:2.4对编译后生成的文件进行打包，包名称默认为：artifactId-version，比如本例生成的jar文件：demo-0.0.1-SNAPSHOT，包文件保存在target目录下
8. 使用maven-install-plugin:2.4把上述打包生成的jar包和pom文件安装到本地的仓库中（一般默认的路径为：%HOMEPATH%\.m2\repository\pom中groupId按.分隔的目录层次\pom中的artifactId\pom中的version\jar包的名称）

Refer: [Maven命令行使用：mvn clean install（安装）](https://www.cnblogs.com/frankyou/p/6062414.html)

- troubleshooting：

  1. build的时候找不到插件maven-surefire-plugin，在pom.xml中加上了

  ```xml
  <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-surefire-plugin</artifactId>
          <configuration>
          	<skipTests>true</skipTests>
          </configuration>
  </plugin>
  ```

  2. 找不到@RequestMapping，进而发现IDEA不提示错误，检查两个地方：

     a. File -> Power Save Mode，这个模式开启的话IDEA不会自动编译

     b. File -> Setting -> Build, Execution, Deployment -> Build project automatically 打开，然后重启IDEA

#### 1.2.4 用java运行hello world

使用命令java -jar <JARFILE> 运行target目录中的文件，springboot自己集成了tomcat server，在命令行可以看到在8080端口运行，打开localhost:8080可以看到Hello World

Refer: [Spring Boot教程](https://www.yiibai.com/spring-boot/spring_boot_bootstrapping.html)

## 五、前端

1. ### CSS

- CSS Modules

  > CSS Modules let you use the same CSS class name in different files without worrying about naming clashes. Learn more about CSS Modules [here](https://css-tricks.com/css-modules-part-1-need/).

  这是一个用来scope css的，这个功能在react-script低于2.0.0的版本只能eject然后在webpack中去配置。

  Refer: [Adding a CSS Modules Stylesheet](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/)

  
