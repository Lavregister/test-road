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

  ```gradle
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

  
