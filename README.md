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

### 1.1 CSS

- CSS Modules

  > CSS Modules let you use the same CSS class name in different files without worrying about naming clashes. Learn more about CSS Modules [here](https://css-tricks.com/css-modules-part-1-need/).

  这是一个用来scope css的，这个功能在react-script低于2.0.0的版本只能eject然后在webpack中去配置。

  Refer: [Adding a CSS Modules Stylesheet](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet/)

- Flex布局

  - 基本概念：flex container/main axis/cross axis/item
  - 容器属性：
    - flex-direction: row|row-reverse|column|column-reverse 主轴方向
    - flex-wrap: nowrap|wrap|wrap-reverse 如果一条轴线排列不下如何换行
    - flex-flow: (row nowrap) flex-direction和flex-wrap的简写形式
    - justify-content: flex-start|flex-end|center|space-between|space-around 项目在主轴上的对齐方式
    - align-items: flex-start|flex-end|center|baseline|stretch 项目在交叉轴上的对齐方式
    - align-content: flex-start|flex-end|center|space-between|space-around|stretch 多根轴线的对齐方式
  - 项目属性：
    - order: integer 定义项目排列顺序，数值越小，排列越靠前，default: 0
    - flex-grow: 项目放大比例，默认为0，即即使存在剩余空间，也不放大
    - flex-shrink: 项目缩小比例，默认为1，即如果空间不足，项目缩小，注: 负值无效
    - flex-basis: 定义在分配多余空间之前，项目占主轴的比例，默认值auto，也可以设置固定值
    - flex: 是flex-grow, flex-shrink,flex-basis的简写，默认0 1 auto，建议优先使用而不是分开写
    - align-self: auto|flex-start|flex-end|center|baseline|stretch 允许单个项目有与其他项目不同对齐方式，可覆盖align-items属性，默认值为auto，表示继承父元素align-items属性，没有父元素，则等同于stretch

  Refer: [flex布局](https://www.jianshu.com/p/4290522e1560) 


### 1.2 JavaScript

- 关于setTimeout

 ```javascript
  const p1 = new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('fail')), 3000)
  })
 ```

  setTimeout里面的第一个参数这样写而不直接写为reject(new Error('fail')是因为setTimeout的第一个参数是一个函数而不是函数的返回值，reject是一个函数，reject(new Error('fail'))是将new Error('fail')传入到reject中得到的返回值，像上面这段例子这样写，第一个参数就是一个函数，三秒之后执行这个函数，返回的是reject(new Error('fail'))的执行结果，即将当前Promise的状态变为reject

- 关于promise(pending)

  1. 调用`resolve`或`reject`并不会终结 Promise 的参数函数的执行

## 六、 NodeJS

1. process.env

   process是Node中的一个全局变量，其中process.env可以用来配置全局属性，可以在程序中配置例如：

   ```javascript
   process.env.TEST = 1;
   ```

   也可以配置在一个.env文件中，这时候要用到一个插件[dotenv](https://link.zhihu.com/?target=https%3A//github.com/motdotla/dotenv%23readme)，然后：

   ```node
   const dotenv = require("dotenv")
   dotenv.config()
   ```

   就可以使用文件中配置的变量了

## 七、JavaScript

### 1.1 Promise

#### 1.1.1 async/await

- **被async标志的函数里面才可以用await**，这个点理解了很久，它需要和**await后面的函数**（可以是返回promise的函数，一般都这样用，也可以不是，但没意义）**会等待promise对象执行完毕并返回**共同理解，从这个async函数外部理解，对于调用这个async函数的调用者来说，应该意识到这个函数可能要花费较长的时间，因为里面的await是“同步的”，调用了这个函数之后可以接着处理其他事情，但是这个函数里面是一种“同步”的状态。

  Refer: [用 async/await 来处理异步](https://www.cnblogs.com/SamWeb/p/8417940.html)

- [async函数的返回值](https://segmentfault.com/a/1190000020782681)总结得非常好，有几个概括：

  1. async函数的返回值是Promise对象，可以用then方法指定下一步的操作。async函数可以看做多个异步操作，包装成一个Promise对象，await命令就是内部then命令的语法糖。
  2. async函数返回一个Promise对象，可以使用then方法添加回调函数。当函数执行的时候，一旦遇到await就会先返回，等到异步操作完成，再接着执行函数体后面的语句。
  3. 返回Promise对象
     async函数返回一个Promise对象。
     async函数内部return语句返回的值，会成为then方法回调函数的参数。

## 八、 编辑器问题

1. Visual Studio Code

   - [ts] 对修饰器的实验支持是一项将在将来版本中更改的功能。设置 "experimentalDecorators" 选项以删除此警告。

     解决方案：在User Settings里面加入

     ```
     "javascript.implicitProjectConfig.experimentalDecorators": true
     ```

## 九、React官网通读

1. [与第三方库协同](https://react.docschina.org/docs/integrating-with-other-libraries.html) 和jQuery库集成的时候出现了

   ```javascript
   class SomePlugin extends React.Component {
     componentDidMount() {
       this.$el = $(this.el);
       this.$el.somePlugin();
     }
   
     componentWillUnmount() {
       this.$el.somePlugin('destroy');
     }
   
     render() {
       return <div ref={el => this.el = el} />;
     }
   }
   ```

   

   理解**this.$el = $(this.el);**

   等号右边是将this.edomd'd'd'dl这个dom ref转换成一个jQuery对象传给左边的this.$el，这个$el写法只是标志它是一个jQuery对象，并不强制要求，是人为的

   总结：

   - dom对象转jQuery对象：

   ```javascript
   var $obj = $(domObj);
   ```

   - jQuery对象转dom对象：

   ```javascript
   var doc2=$("#idDoc2")[0];   //转换jQuery对象为DOM对象  
   doc2.innerHTML="这是jQuery的第一个DOM对象"  
     //使用jQuery对象本身提供的get函数来返回指定集合位置的DOM对象  
   var doc2=$("#idDoc2").get(0);  
   doc2.innerHTML="这是jQuery的第二个DOM对象"  
   ```
