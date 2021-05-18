[TOC]

# Java 注释
> 单行、块（多行）、文档注释

我们在写代码的过程，通常会增加一些注释内容，用来描述代码的作用或功能，从而提高代码的阅读性。

你可能会质疑，我的代码通过清晰命名就能够解释，留下注释代码会影响整体代码的整洁与美观。再说注释写多了都是废话和一些重复的描述。

那为什么要写注释呢？在代码不足以表达意思的时候，让自己和别人更容易理解自己写的代码和复用代码，就需要注释来表达。做到之前没有见过这段代码，但能根据代码和一些注释快速地知道这段代码是干什么的。
    
    
 ## 1.行注释
  ### 1.1 单行注释
 ```java
 public void test(){
    // 调用外部接口获取信息
    this.queryxx();
}
 ```
 用于简单的描述一行代码的功能
 
  ### 1.2 多行注释
 ```java
 public void test2(){
    /**
     * 这段代码涉及逻辑比较复杂
     * 先查询，在生成数据
     */
    this.queryxx();
}
 ```  
 用于详细的描述一行或一段代码的功能
 
 ## 2.文档注释
 >JDK 中有个工具是 javadoc, 它会根据写的文档注释生成一个 HTML 文档。比如我们会使用到的 jdk 的 API 文档就是通过 javadoc 生成的。所以写的好的文档注释，形成 API 接口文档对后续的开发也有好的引导作用。

文档注释的组成部分如下：
```java
/**
 * 1、描述部分
 * 对类或方法的<b>解释说明</b>
 * 
 *
 * 2、标记部分
 * javadoc 注释标签语法
 * @param  参数
 * @author  作者
 * @return  返回值
 *
 */
```
样例：
```java
 /**
   * Converts a <code>byte[]</code> to a String using the specified character encoding.
   * 
   * @param bytes the byte array to read from
   * @param charset the encoding to use, if null then use the platform default
   * @return a new String
   * @throws NullPointerException
   *             if {@code bytes} is null
   * @since 3.2
   * @since 3.3 No longer throws {@link UnsupportedEncodingException}.
   */
 public static String toEncodedString(byte[] bytes, Charset charset) {
     return new String(bytes, charset != null ? charset : Charset.defaultCharset());
 }

```

### 2.1 类注释
类注类注释必须放在import 语句之后，类定义之前。可以使用以下标记：

- @author  作者
- @version 版本

下面的样例中，在文档注释开始的地方就进行了描述。也有在 @description 标签后进描述的写法，但查阅了网上的资料没有看到 javadoc 工具支持 @description 的，此处结论仅供参考。
    
```java
/**
 * 这是一个{@code List}操作通用方法类
 *
 * @author <a href="mailto:xxxxx@github.com">xxx</a>
 * @version 1.0  2021/01/01.
 */
public class ListUtils {}
```

### 2.2 方法注释
方法注释必须放在每个所描述的方法之前，方法注释可以使用一下标记：

- @param 参数变量描述
- @return 方法返回值说明
- @throws 方法可能抛出的异常说明

```java
/**
 * 将输入的字节信息进行base64加密
 * @param bytes 字节信息
 * @return String base64字符串
 * @author <a href="mailto:xxxxx@github.com">xxx</a>
 */
public static String encodeByteToBase64Str(byte[] bytes) {}
```

### 2.3 域注释
对于静态变量的一个描述
```java
/**
 * 年龄 10 岁
 */
public static final int AGE = 10;
```
## 附录 
 ### 1.文档标签
    - @author       作者
    - @deprecated   过期的类或成员
    - @exception    抛出的异常
    - @param        方法参数
    - @return       方法返回值
    - @see          到另外一个主题的链接
    - @serial       序列化属性
    - @serialData   通过writeObject( ) 和 writeExternal( )方法写的数据
    - @serialField  说明一个ObjectStreamField组件
    - @since        标记当引入一个特定的变化时
    - @throws       抛出的异常
    - @version      类的版本
    - {@inheritDoc}    从直接父类继承的注释
    - {@link}          插入一个到另一个主题的链接
    - {@linkplain}     插入一个到另一个主题的链接，但是该链接显示纯文本字体
    - {@docRoot}       指明当前文档根目录的路径
    - {@value}         显示常量的值，该常量必须是static属性。
    - {@code}          将文本标记为code
    
 ### 2.HTML 标签
 文档注释的描述中支持添加 HTML 标签的形式
 ```te'
 - <p></p>       段落
 - <pre></pre>   文本域
 - <a></a>       超链接
 - <ul></ul>     目录
 - <li></li>     子目录
 - <b></b>       加粗
 - <u></u>       下划线
 - <em></em>     强调
 - <br/>         换行
 
```

   




