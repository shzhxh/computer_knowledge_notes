#### 安装

```
0. sudo apt install android-sdk
1. 从https://developer.android.google.cn下载android_studio.zip
2. 解压到安装目录$Program
3. cd $Program/android-studio/bin
4. ./studio.sh
```

#### 错误处理

- Could not HEAD 'https://dl.google.com/dl/android/maven2/com/android/support/appcompat-v7/maven-metadata.xml'

  这是因为如上网址不能访问，解决办法是使用阿里云的maven。在顶级目录的build.gradle里插入maven的地址使成如下形式：

  ```
  buildscript {
      repositories {
          maven {
              url 'http://maven.aliyun.com/nexus/content/groups/public'
          }
          google()
          jcenter()
      }
  }
  allprojects {
      repositories {
          maven {
              url 'http://maven.aliyun.com/nexus/content/groups/public'
          }
          google()
          jcenter()
      }
  }
  ```
