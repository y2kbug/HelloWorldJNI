# Mac OS 下编译JNI

##创建HelloWorld.java
```java
class HelloWorld {
	private native void print();
	public static void main(String[] args) {
		new HelloWorld().print();
	}
	static {
		System.loadLibrary("HelloWorld");
	}
}
```

## 编译成class
```
$ javac HelloWorld.java
```

##生成C++头文件
```
javah --jni HelloWorld
```

##创建HelloWorld.cpp 添加native c++代码
```cpp
#include <jni.h>
#include <iostream>
#include "HelloWorld.h"
using namespace std;
 
JNIEXPORT void JNICALL 
Java_HelloWorld_print(JNIEnv *, jobject){
	cout << "Oh JNI, how cumbersome you are!\n";
	return;
}
```

###生成动态链接库
```
g++ -dynamiclib -o libhelloworld.jnilib  HelloWorld.cpp  -I /System/Library/Frameworks/JavaVM.framework/Headers
```

####-dynamiclib选项表示生成动态库
####-o指定生成的动态库文件的名称
####-I 指定编译的依赖的头文件所在的路径
  

