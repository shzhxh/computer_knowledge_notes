#### 简介

从class文件生成C文件(头文件和源文件)

#### 语法

```
javah [options] <classes>
	# classes是完整的类名，如java.lang.Object
```

#### 选项

```
-o file	# 输出到文件file
-d dir	# 输出到目录dir，注：-d和-o是互斥的
-V -verbose
-h --help -?
-version
-jni	# 生成jni风格的头文件(默认)
-force
-classpath path
-cp path
-bootclasspath path
```



