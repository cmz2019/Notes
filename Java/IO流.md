# 文件

## 概念

文件，对我们并不陌生，文件是==保存数据的地方==，比如大家经常使用的word文档，txt文件，excel文件等都是文件。它既可以保存一张图片，也可以保持视频、声音...

### 文件流

文件在程序中是以流的形式来操作的

![image-20210731143109971](https://gitee.com/cmz2000/album/raw/master/image/image-20210731143109971.png)

流：数据在数据源（文件）和程序（内存）之间经历的路径

输入流：数据从数据源（文件）到程序（内存）的路径

输出流：数据从程序（内存）到数据源（文件）的路径

## 常用操作

### 创建文件

创建文件对象的相关构造器和方法

![image-20210731144157132](https://gitee.com/cmz2000/album/raw/master/image/image-20210731144157132.png)

相关方法

```java
new File(String pathname)				//根据路径构建一个File对象
new File(File parentFile, String child)		//根据父目录文件+子路径构建
new File(String parentPath, String child)	//根据父目录+子路径构建
```

这时文件还没有创建成功，还需要调用 `File.createNewFile()` 来创建文件。

举例：

```java
// 方式一
public void create01() {
    File newFile2 = new File("d:/new2.txt");
    if (newFile2.createNewFile()) {
        System.out.println("创建成功");
    }
    else {
        System.out.println("创建失败");
    }
}
// 方式二
public void create02() {
    File file = new File("d:/");
    File newFile3 = new File(file, "new3.txt");
    if (newFile3.createNewFile()) {
        System.out.println("创建成功");
    }
    else {
        System.out.println("创建失败");
    }
}
// 方式三
// 注意细节 d:/ 不能写成 d，否则表示在当前目录下
public void create03() {
    File newFile1 = new File("d:/", "new1.txt");
    if (newFile1.createNewFile()) {
        System.out.println("创建成功");
    }
    else {
        System.out.println("创建失败");
    }
}
```

### 获取文件信息

```java
public void getInfo() {
    File file = new File("d:/new1.txt");
    System.out.println("获取文件名字" + file.getName());
    System.out.println("文件绝对路径" + file.getAbsolutePath());
    System.out.println("文件父级目录" + file.getParent());
    System.out.println("文件大小(字节)" + file.length());
    System.out.println("文件是否存在" + file.exists());
    System.out.println("是否为文件" + file.isFile());
    System.out.println("是否为目录" + file.isDirectory());
}
```

### 目录操作和文件删除

`mkdir` 创建一级目录、`mkdirs` 创建多级目录、`delete` 删除空目录或文件

# IO流原理及流的分类

## Java IO流原理

1. I/O是Input/Output的缩写，I/O技术是非常实用的技术，用于处理数据传输。如读/写文件，网络通讯等。
2. Java程序中，对于数据的输入/输出操作以”流(stream)”的方式进行。
3. java.io包下提供了各种“流”类和接口，用以获取不同种类的数据，并通过方法输入或输出数据。
4. 输入input：读取外部数据（磁盘、光盘等存储设备的数据）到程序（内存）中。
5. 输出output：将程序（内存）数据输出到磁盘、光盘等存储设备中。

## 流的分类

+ 按操作数据单位不同分为：字节流（8 bit），字符流（按字符，一个字符对应多少字节要根据不同编码格式来看）
  + 字节流：操作二进制文件有优势
  + 字符流：操作文本文件有优势
+ 按数据流的流向不同分为：输入流，输出流
+ 按流的角色的不同分为：节点流，处理流/包装流

| （抽象基类） | 字节流       | 字符流 |
| ------------ | ------------ | ------ |
| **输入流**   | InputStream  | Reader |
| **输出流**   | OutputStream | Writer |

Java的IO流共涉及40多个类，实际上非常规则，都是从如上4个抽象类派生的。

由这四个类派生出来的子类名称都是以其父类名作为子类名后缀。

### InputStream

![image-20210801103708953](https://gitee.com/cmz2000/album/raw/master/image/image-20210801103708953.png)

#### FileInoutStream

![image-20210801104524027](https://gitee.com/cmz2000/album/raw/master/image/image-20210801104524027.png)

代码演示如下：

```java
/**
* 使用read()方法
*/
public void fileRead01() {
    File file = new File("d:/hello.txt");
    FileInputStream fileInputStream = null;
    int readData;
    try {
        // 从该输入流读取一个字节的数据。如果没有输入可用，此方法将被阻止
        // 如果返回-1，表示读取到文件尾部
        fileInputStream = new FileInputStream(file);
        while ((readData = fileInputStream.read()) != -1) {
            System.out.print((char) readData); // 转成char显示
        }
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        // 关闭文件流，释放资源
        try {
            fileInputStream.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}

/**
* 使用read(byte[] b)方法
*/
public void fileRead02() {
    File file = new File("d:/hello.txt");
    FileInputStream fileInputStream = null;
    byte[] buf = new byte[8]; // 字节数组，一次读取8个字节
    int readLen;
    try {
        // 从该输入流读取最多b.length字节的数据到字节数组。此方法将阻塞，直到某些输入可用
        // 如果返回-1，表示读取到文件尾部
        // 如果读取正常，返回实际读取的字节数
        fileInputStream = new FileInputStream(file);
        while ((readLen = fileInputStream.read(buf)) != -1) {
            System.out.print(new String(buf, 0, readLen)); // 转成String显示
        }
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        // 关闭文件流，释放资源
        try {
            fileInputStream.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### OutputStream

![image-20210801111317784](https://gitee.com/cmz2000/album/raw/master/image/image-20210801111317784.png)

#### FileOutputStream

![image-20210801111231437](https://gitee.com/cmz2000/album/raw/master/image/image-20210801111231437.png)

在a.txt文件中写入“hello, world"，如果文件不存在，会创建文件（注意：前提是目录已经存在）

代码演示：

```java
public void writeFile() {
    File file = new File("d:/a.txt");
    FileOutputStream fileOutputStream = null;
    try {
        // new FileOutputStream(File file, boolean append)
        // 如果append参数是true，则表示在文件末尾追加
        // 如果是false，则覆盖原文件内容（默认是false）
        fileOutputStream = new FileOutputStream(file);
        String str = "hello, world";
        // str.getBytes()可以把 字符串 -> 字节数组
        fileOutputStream.write(str.getBytes()); // 两种方式都可以
        fileOutputStream.write(str.getBytes(), 0, str.length());
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            fileOutputStream.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 文件拷贝操作

利用InputStream和OutputStream实现文件拷贝

![image-20210801122935058](https://gitee.com/cmz2000/album/raw/master/image/image-20210801122935058.png)

```java
public void fileCopy() {
    File source = new File("d:/cow.JPG");	//源文件
    File dest = new File("d:/cow2.JPG");	//目标文件
    FileInputStream fileInputStream = null;
    FileOutputStream fileOutputStream = null;
    try {
        fileInputStream = new FileInputStream(source);
        fileOutputStream = new FileOutputStream(dest);
        byte[] buf = new byte[1024];	//缓冲byte[]数组，一次读取1024字节
        int readLen = 0;
        while ((readLen = fileInputStream.read(buf)) != -1) {	//循环读取并写入
            fileOutputStream.write(buf, 0, readLen);
        }
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            if (fileInputStream != null) {
                fileInputStream.close();
            }
            if (fileOutputStream != null) {
                fileOutputStream.close();
            }
        }
        catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### Reader

![image-20210801123329766](https://gitee.com/cmz2000/album/raw/master/image/image-20210801123329766.png)

#### FileReader

相关方法

```java
new FileReader(File/String);
int read();			//每次读取单个字符，返回该字符，如果到文件末尾返回-1
int read(char[]);	//批量读取多个字符到数组，返回读取到的字符数，如果到文件末尾返回-1
```

代码演示：

```java
/**
* 采用read(char[])的方式
*/
public void fileReader() {
    File file = new File("d:/story.txt");
    FileReader fileReader = null;
    char[] buf = new char[8];
    int dataLen;	//记录读取到的字符数
    try {
        fileReader = new FileReader(file);
        while ((dataLen = fileReader.read(buf)) != -1) {
            System.out.print(new String(buf, 0, dataLen));
        }
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            fileReader.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### Writer

![image-20210801123513044](https://gitee.com/cmz2000/album/raw/master/image/image-20210801123513044.png)

#### FileWriter

常用方法

```java
new FileWriter(File/String, boolean append);//append为false表示覆盖模式，为true表示追加模式，默认是false
void write(int);				//写入单个字符
void write(char[]);				//写入指定数组
void write(char[], off, len);	//写入指定数组的指定部分
void write(String);				//写入整个字符串
void write(String, off, len);	//写入字符串的指定部分
```

**注意**：FileWriter使用后，必须要关闭（close）或刷新（flush）才会真正把数据写入到文件中，否则写入不到指定的文件（数据还在内存中）！

代码演示其中一种方式：

```java
/**
* 采用write(String, off, len)的方式
*/
public void fileWriter() {
    File file = new File("d:/note.txt");
    FileWriter fileWriter = null;
    String str = "随便写点anything";
    try {
        fileWriter = new FileWriter(file);
        fileWriter.write(str, 0, str.length());
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            fileWriter.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

# 节点流和处理流

## 基本介绍

**节点流**可以从一个特定的数据源读写数据，如FiteReader、FileWriter

![image-20210801144235078](https://gitee.com/cmz2000/album/raw/master/image/image-20210801144235078.png)

**处理流**（也叫包装流）是“连接”在已存在的流（节点流或处理流）之上，对已存在的流（节点流或处理流）进行封装，为程序提供更为强大的读写功能，如BufferedReader、BufferedWriter

![image-20210801144247616](https://gitee.com/cmz2000/album/raw/master/image/image-20210801144247616.png)

![image-20210801144601846](https://gitee.com/cmz2000/album/raw/master/image/image-20210801144601846.png)

节点流和处理流的区别和联系：

1. 节点流是底层流 / 低级流，直接跟数据源相接。
2. 处理流包装节点流，既可以消除不同节点流的实现差异，也可以提供更方便的方法来完成输入输出。
3. 处理流（也叫包装流）对节点流进行包装，使用了修饰器设计模式，不会直接与数据源相连。

处理流的功能主要体现在以下两个方面：

1. 性能的提高：主要以增加缓冲的方式来提高输入输出的效率。
2. 操作的便捷：处理流可能提供了一系列便捷的方法来一次输入输出大批量的数据，使用更加灵活方便。

## 处理流

### Buffered流

#### BufferedReader

BufferedReader 和 BufferedWriter属于字符流，是按照字符来读取数据的。关闭时，只需要关闭外层流即可。

实现读取文本文件（按行读取），代码如下：

```java
public void bufferedReader() {
    File file = new File("d:/note.txt");
    BufferedReader bufferedReader = null;
    try {
        bufferedReader = new BufferedReader(new FileReader(file));
        String line;
        while ((line = bufferedReader.readLine()) != null) {
            System.out.println(line);
        }
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            bufferedReader.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### BufferedWriter

实现往note.txt文件中以追加方式写入数据，代码如下：

```java
public void bufferedWriter() {
    String filePath = "d:/ok.txt";
    BufferedWriter bufferedWriter = null;
    try {
        bufferedWriter = new BufferedWriter(new FileWriter(filePath, true)); //追加模式在FileWriter里确定
        bufferedWriter.write("hello, 写入一个句子");
        bufferedWriter.newLine(); // 插入一个和系统相对应的换行
        bufferedWriter.write("换行啦");
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            bufferedWriter.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

#### Buffered字符文件拷贝

```java
public void bufferedCopy() {
    String srcPath = "d:/note.txt";
    String destPath = "d:/note1.txt";
    BufferedReader br = null;
    BufferedWriter bw = null;
    String line;
    try {
        br = new BufferedReader(new FileReader(srcPath));
        bw = new BufferedWriter(new FileWriter(destPath));
        while ((line = br.readLine()) != null) {
            bw.write(line);
            bw.newLine();
        }
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            br.close();
            bw.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

注意：BufferedReader 和 BufferedWriter是按照字符操作的，不要去操作二进制文件，否则可能造成文件损坏。

#### BufferedInputStream

BufferedInputStream是字节流，在创建BufferedInputStream时，会创建一个内部缓冲区数组。

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210801163416101.png" alt="image-20210801163416101" style="zoom:67%;" />

![image-20210801163540937](https://gitee.com/cmz2000/album/raw/master/image/image-20210801163540937.png)

#### BufferedOutputStream

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210801163705739.png" alt="image-20210801163705739" style="zoom:50%;" />

![image-20210801163738654](https://gitee.com/cmz2000/album/raw/master/image/image-20210801163738654.png)

#### Buffered字节文件拷贝

使用 BufferedInputStream 和 BufferedOutputStream 可以实现字节文件拷贝

```java
public void bufferedCopy2() {
    String srcPath = "d:/note.avi";
    String destPath = "d:/note1.avi";
    BufferedInputStream bis = null;
    BufferedOutputStream bos = null;
    try {
        int readLen;
        byte[] buf = new byte[1024];
        bis = new BufferedInputStream(new FileInputStream(srcPath));
        bos = new BufferedOutputStream(new FileOutputStream(destPath));
        while ((readLen = bis.read(buf)) != -1) {
            bos.write(buf, 0, readLen);
        }
    }
    catch (IOException e) {
        e.printStackTrace();
    }
    finally {
        try {
            bis.close();
            bos.close();
        }
        catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### 对象流

ObjectInputStream和ObjectOutputStream

看以下的需求：

1. 将int num = 100这个int数据保存到文件中，注意不是 100 数字，而是int 100，并且，能够从文件中直接恢复 int 100

2. 将Dog dog = new Dog("小黄", 3)这个dog对象保存到文件中，并且能够从文件恢复
3. 上面的要求，就是能够将基本数据类型或者对象进行序列化和反序列化操作

序列化和反序列化：

1. 序列化就是在保存数据时，保存**数据的值**和**数据类型**
2. 反序列化就是在恢复数据时，恢复数据的值和数据类型
3. 需要让某个对象支持序列化机制，则必须让其类是可序列化的，为了让某个类是可序列化的，该类必须实现如下两个接口之一：
   + Serializable	//这是一个标记接口
   + Externalizable  //该接口有方法需要实现，因此我们一般实现上面的

#### 基本介绍

1. 功能：提供了对基本类型或对象类型的序列化和反序列化的方法
2. ObjectOutputStream 提供序列化功能
3. ObjectlnputStream 提供反序列化功能

#### ObjectlnputStream

![image-20210801211508469](https://gitee.com/cmz2000/album/raw/master/image/image-20210801211508469.png)

代码演示序列化：

```java
public void objectOutput() throws Exception {
    // 序列化后，保存的文件格式不是纯文本，而是按照它自己的格式来保存
    File file = new File("d:/data.dat");
    ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream(file));
    // 序列化数据到 d:/data.dat
    oos.writeInt(100);     	// 自动装箱 int -> Integer(实现了 Serializable)
    oos.writeBoolean(true); // boolean -> Boolean
    oos.writeUTF("你好，世界");  // String
    // 序列化保存 Dog 对象
    oos.writeObject(new Dog("旺财", 3));
}
// 需要实现 Serializable 接口
class Dog implements Serializable {
    String name;
    int age;
    public Dog(String name, int age) {
        this.name = name;
        this.age = age;
    }
    @Override
    public String toString() {
        return "Dog{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

#### ObjectOutputStream

![image-20210801211552429](https://gitee.com/cmz2000/album/raw/master/image/image-20210801211552429.png)

接下来使用ObjectOutputStream把上面的d:/data.dat反序列化并输出打印，代码如下：

```java
public void objectInput() throws Exception {
    // 指定反序列化的文件
    File file = new File("d:/data.dat");
    ObjectInputStream ois = new ObjectInputStream(new FileInputStream(file));
    // 读取(反序列化)的顺序需要和保存数据(序列化)的顺序一致，否则会出现异常
    System.out.println(ois.readInt());
    System.out.println(ois.readBoolean());
    System.out.println(ois.readUTF());
    System.out.println(ois.readObject());
    ois.close();
}
```

运行结果如下：

```
100
true
你好，世界
Dog{name='旺财', age=3}
```

注意事项和细节说明：

+ 读写顺序要一致，即反序列化时的顺序要与序列化的顺序保持一致

+ 要求实现序列化或反序列化对象，需要实现 Serializable

+ 序列化的类中建议添加SerialVersionUID，为了提高版本的兼容性

+ 序列化对象时，默认将里面所有属性都进行序列化，但除了 static 或 transient 修饰的成员

  > transient关键字：在实现了Serializable接口的类中，若属性使用了transient关键字修饰，则在序列化对象的时候，这个属性就不会序列化到指定的目的地中。

+ 序列化对象时，要求里面属性的类型也需要实现序列化接口

+ 序列化具备可继承性，也就是如果某类已经实现了序列化，则它的所有子类也已经默认实现了序列化

### 标准输入输出流

|                         | 编译类型    | 运行类型            | 默认设备 |
| ----------------------- | ----------- | ------------------- | -------- |
| **System.in 标准输入**  | InputStream | BufferedInputStream | 键盘     |
| **System.out 标准输出** | PrintStream | PrintStream         | 显示器   |

代码演示：

```java
public static void main(String[] args) throws Exception {
    Scanner scanner = new Scanner(System.in);
    System.out.println("请输入");
    String input = scanner.next();
    System.out.println(input);
}
```

### 转换流

InputStreamReader 和 OutputStreamWriter

先看一个问题：BufferedReader 默认是使用 utf-8 编码的，如果读取的数据不是采用 utf-8 编码，则会出现乱码

#### 介绍

1. InputStreamReader：Reader的子类，可以将InputStream（字节流）包装成Reader（字符流）
2. OutputStreamWriter：Writer的子类，实现将OutputStream（字节流）包装成Writer（字符流）
3. 当处理纯文本数据时，使用字符流效率更高，并且可以有效解决中文问题，所以建议将字节流转换成字符流
4. 可以在使用时指定编码格式（比如utf-8，gbk，gb2312，ISO8859-1等）

#### InputStreamReader

![image-20210802111304944](https://gitee.com/cmz2000/album/raw/master/image/image-20210802111304944.png)

可以看到，InputStreamReader的构造器可以传入一个InputStream并指定处理的编码格式

代码演示，以gbk格式读取文本文件：

```java
public void inputStreamReader() throws Exception {
    File file = new File("d:/note.txt");
    // 把 FileInputStream 转成 InputStreamReader，并指定编码 gbk
    InputStreamReader isr = new InputStreamReader(new FileInputStream(file), "gbk");
    // 把 InputStreamReader 传入 BufferedReader
    BufferedReader br = new BufferedReader(isr);
    String s = br.readLine();
    System.out.println(s);
    // 关闭外层流
    br.close();
}
```

#### OutputStreamWriter

![image-20210802111536932](https://gitee.com/cmz2000/album/raw/master/image/image-20210802111536932.png)

同样的，OutputStreamWriter也可以指定处理的编码格式

代码演示：

```java
public void outputStreamWriter() throws Exception {
    File file = new File("d:/note.txt");
    OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(file), "gbk");
    osw.write("hello, 世界");
    osw.close();
}
```

### 打印流

PrintStream 和 PrintWriter

打印流只有输出流，没有输入流

#### PrintStream

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210802113536833.png" alt="image-20210802113536833" style="zoom:67%;" />

代码演示：

```java
public void printStream() throws IOException {
    PrintStream ps = System.out; // System.out就是一个PrintStream
    // 在默认情况下，PrintStream 输出数据的位置是标准输出，即显示器
    /*  print源码如下:
        public void print(String s) {
            if (s == null) {
                s = "null";
            }
            write(s);
        }
    */
    ps.print("hello, 世界");
    // 因为print底层使用的是write，所以可以直接调用write进行打印/输出
    ps.write("hello, 世界".getBytes()); // PrintStream是字节流，要以字节方式输出
    ps.close();
    // 可以修改 System.out 的输出位置
    System.setOut(new PrintStream("d:/out.txt"));
    System.out.println("hello, 世界"); //会输出到d:/out.txt中
}
```

#### PrintWriter

<img src="https://gitee.com/cmz2000/album/raw/master/image/image-20210802113638891.png" alt="image-20210802113638891" style="zoom:67%;" />

代码演示：

```java
public void printWriter() throws IOException {
    // PrintWriter pw = new PrintWriter(System.out);
    PrintWriter pw = new PrintWriter(new FileWriter("d:/out.txt"));
    pw.print("hello世界~");
    pw.close();	// 注意，需要调用close或者flush方法才会真正写入数据到文件中
}
```

### Properties类

#### 需求

如下一个配置文件 mysqI.properties

```properties
ip=192.168.0.13
user=root
pwd=12345
```

请问编程读取 ip、user 和 pwd 的值是多少

#### 传统的方法

使用 BufferedReader 来按行读取，再用 split 方法分离

代码如下：

```java
public void properties01() throws IOException {
    BufferedReader br = new BufferedReader(new FileReader("src/mysql.properties"));
    String line;
    while ((line = br.readLine()) != null) {
        String[] data = line.split("=");
        System.out.println(data[0] + "的值是: " + data[1]);
    }
    br.close();
}
```

如果要求只获取ip的值，传统方法就显得有点麻烦了，这时候可以使用Properties类

#### 使用Properties类

专门用于读写配置文件的集合类，配置文件的格式：

```properties
键=值
键=值
```

注意：键值对不需要有空格，值不需要用引号围起来。默认类型是String

常见方法有：

```java
void load(Reader/InputStream); 		// 加载配置文件的键值对到Properties对象
void list(PrintStream/PrintWriter);	// 将数据显示到指定设备
String getProperty(String key);		// 根据键获取值
Object setProperty(String key, String value);	// 设置键值对到Properties对象
void store(Writer/OutputStream, String);	// 将Properties中的键值对存储到配置文件，在idea中，保存信息到配置文件，如果含有中文，会存储为unicode码
```

代码演示：

```java
/**
* 读取配置文件
*/
public void properties02() throws IOException {
    Properties properties = new Properties();
    // 加载指定配置文件
    properties.load(new FileReader("src/mysql.properties"));
    // 把k-v显示到控制台
    properties.list(System.out);
    // 根据key获取对应的值
    String user = properties.getProperty("user");
    String pwd = properties.getProperty("pwd");
    System.out.println("user:" + user + "\npwd:" + pwd);
}
/**
* 写入配置文件（创建or修改）
*/
public void properties03() throws IOException {
    Properties properties = new Properties();
    properties.setProperty("charset", "utf8");
    properties.setProperty("user", "汤姆");
    properties.setProperty("pwd", "abc11");
    properties.store(new FileWriter("src/mysql2.properties"), null);
}
```

