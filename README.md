#### .NET Compact Framework新增内容：
[microsoft官方介绍](https://msdn.microsoft.com/en-us/library/bb397835(v=vs.90).aspx)
- ##### Windows Communication Foundation(WCF)
- ##### LINQ
  -  relational databases
  -  XML data
  -  in-memory objects
- ##### Windows Forms
  - Panel
  - Splitter
  - PictureBox
- ##### SoundPlayer 声音播放对象
- ##### Compression 压缩与解压缩
  - ###### NET Compact Framework 3.5添加了对System.IO.Compression命名空间中的以下类的支持：
  1. CompressionMode
  2. DeflateStream
  3. GZipStream
- ##### Delegates


### PS
- ###### PDA中的文件夹路径是从根目录开始的：即,.NET Compact Framework与Windows Embedded CE一样，不支持 **==相对路径==**
eg:
```
 FileStream IconStream = new FileStream(".\\My Documents\\notify.ico",
                FileMode.Open, FileAccess.Read);
```
如果希望到安装文件目录中，则需要：
```
".\\Program Files\PDA本地测试\\notify.ico"
```


- ###### All classes that inherit from the Control class support simple data property binding. Additionally, the classes that inherit from ListControl, such as ListBox and ComboBox, can bind to objects (such as ArrayList) that expose IList or IListSource. 

- 所有从Control类继承的类都支持简单的数据属性绑定。此外，从ListControl继承的类（如ListBox和ComboBox）可以绑定到继承自IList或IListSource的对象（如ArrayList）。
- ###### The .NET Compact Framework supports data binding to a strongly typed DataSet, and you can also bind to a DataSet with schema that is read from an XML Schema definition language (XSD) file.    
- .NET Compact Framework支持将数据绑定到强类型的DataSet，还可以使用从XML模式定义语言（XSD）文件读取的模式，绑定到DataSet。 
- ###### .NET Framework中的LINQ和.NET Compact Framework 中的LINQ之间的区别如下：
- #### PS : 准的查询操作符允许您将查询应用于任何基于IEnumerable <T>的信息源。

- 1. 在.NET Compact Framework上，只支持标准查询运算符。支持LINQ to DataSet，它为DataSet和DataTable提供了LINQ支持。
- 2. 在.NET Compact Framework上，除了System.Xml.XPath  扩展外，还支持LINQ to XML 。

- ##### PS ： How to: [改变方向和分辨率](https://msdn.microsoft.com/en-us/library/ms229649(v=vs.90).aspx)


#### Threading in the .NET Compact Framework
- In the .NET Compact Framework version 1.0, the default maximum number of threads in a thread pool is 256 threads with a stack size of 64 KB.
-  ##### 在.NET Compact Framework 1.0版中，线程池中默认的==最大线程数是256个线程，堆栈大小为64 KB==。
- In the .NET Compact Framework version 2.0 and later versions, this limit is reduced to a maximum of 25 threads with a stack size of 128 KB, which more closely matches the functionality of the full .NET Framework.
- ##### 在.NET Compact Framework 2.0及更高版本中，此限制减少到==最多25个线程，堆栈大小为128 KB==，这与完整的.NET Framework的功能更接近。
- ##### 您可以通过减少并发Web请求的数量或增加线程池中允许的最大线程来避免线程耗尽。.NET Compact Framework 2.0支持SetMaxThreads方法。
  - 1. workerThreads：线程池中的最大工作线程数。这可以是任何值。
  - 2. completionPortThreads：线程池中异步线程的最大数量。.NET Compact Framework当前忽略这个值，但它必须设置在1到1000之间。为了将来的兼容性，建议使用500，因为它是完整.NET Framework的默认值。
  - PS: 对于运行.NET Compact Framework 1.0的设备，可以通过更改注册表设置来减少线程池中允许的最大线程数。将CFROOT \ ThreadPool项中的MaxThreads设置为所需的值。请注意，此注册表项不会被更高版本的.NET Compact Framework使用。

### 一些例子：
#### 如何：[进行红外文件传输](https://msdn.microsoft.com/en-us/library/ms172496(v=vs.90).aspx)

#### 如何：[Network Programming in the .NET CF](https://msdn.microsoft.com/en-us/library/1afx2b0f(v=vs.90).aspx)


### 提高性能的几个办法：
- #### 1. (Windows Forms and graphics)窗体优化：
    - ##### 在提供它们的控件上使用BeginUpdate和EndUpdate方法,例如ComboBox,ListBox,ListView,ToolStripComboBox和TreeView;
    - ##### 在使用Show()方法之前，在后台加载其他表单并使用数据填充控件;
    - ##### 在处理事件订阅对象之前,使用减法赋值操作符(-=))取消订阅其事件;
    - ##### 覆盖控件上的OnKeyDown,OnKeyPress和OnKeyUp方法,而不是添加关键事件处理程序.
    - ##### 同步事件处理代码只执行必要的任务,以便待处理的流程可以继续;
    - ##### 重新定位控件时使用SuspendLayout和ResumeLayout方法.

- #### 2. (data and strings)内存中存放数据和字符串：
    - ##### for循环中操作整形对象,避免进行Object对象的操作 
    - ##### 避免使用枚举类型的ToString(),因为它会通过搜索元数据表的性能影响;
    - ##### 单个方法使用的内存不得超过64KB;
    - ##### 频繁操作字符串的时候，请使用StringBuilder;
    - ##### 限制打开的SqlCeCommand对象的数量,并在完成后析构.

- #### 3. (Collections)集合的内存优化：
    - ##### 如果集合基于数组,则使用索引器.Use indexers if the collection is based on an array.
    - ##### 尽可能指定固定长度的集合,避免使用动态调整大小.
    - ##### 使用泛型集合来避免值类型的装箱和取消装箱开销.自定最适合需求的集合类.
- #### 4.(XML)的内存优化：
    - ##### 使用XmlTextReader和XmlTextWriter而不是使用更多内存的XmlDocument;
    - ##### 指定XmlReaderSettings和XmlWriterSettings的设置以提高性能。合理的配置IgnoreWhitespace和IgnoreComments属性值,可以显著提高性能.
    - ##### 使用比ANSI和Windows codepage编码==更快的UTF-8，ASCII和UTF-16字符编码==.
    - ##### 避免使用模式(schema)进行解析,因为它需要额外的验证工作.
    - ##### 将列映射为属性,并在从XML源填充DataSet时使用类型化的DataSet.
    - ##### 填充DataSet时避免以下情况：
       - 模式推断(schema)
       - 嵌套Table
       - 多个DateTime列.为了获得更好的性能,请改用Ticks(long:Int64)属性值.
    - ##### 使用XML反序列化时,以下准则可提高性能：
       - 保持元素和属性名称==尽可能短==,因为每个字符都必须经过验证. 
       - 基于属性数据的XML比基于元素数据的XML更快.
       - 在适合的情况下使用XmlNodeReader.Skip()方法.
         ```
           XmlNodeReader reader = null;

            try
            {
               //Create and load the XML document.
               XmlDocument doc = new XmlDocument();
               doc.LoadXml("<!-- sample XML -->" +
                           "<book>" +
                           "<title>Pride And Prejudice</title>" +
                           "<price>19.95</price>" +
                           "</book>");
        
               //Load the XmlNodeReader 
               reader = new XmlNodeReader(doc);
        
               reader.MoveToContent(); //Move to the book node.
               reader.Read();  //Read the book start tag.
               reader.Skip();   //Skip the title element.
        
               Console.WriteLine(reader.ReadOuterXml());  //Read the price element.
        
             } 
        
             finally 
             {
                if (reader != null)
                  reader.Close();
             }
         ```
       - 当性能变得至关重要时,考虑二进制序列化.
    - 由于序列化大量的XML可能会占用内存,因此应考虑使用BinaryReader和BinaryWriter来构建自定义的二进制序列化机制.
    - XML序列化时,使用每个类型的一个XmlSerializer实例来减少搜索元数据的时间
- #### 5.(Web service)的内存优化：
    - ##### 读取和写入时,使用[DiffGram](https://msdn.microsoft.com/en-us/library/ms172088(v=vs.90).aspx)格式的DataSet.
    - ##### 将远程调用的DataSet及其Schame保存为XML.
    - ##### 在启动画面中尽量(第一次)调用简单的Web服务方法,因为第一个调用比后续调用要慢.
    - ##### 小心处理网络与数据错误.
    - ##### 在某些情况下,在进行Web服务调用之前手动序列化DataSet为XML字符串可以获得更好的性能.
- #### 6.在高级编程的内存优化：
    - ##### 异步处理大型耗时操作.
    - ##### 避免虚拟呼叫, .NET Compact Framework运行时虚拟调用比静态调用或实例调用慢大约30％.由于资源受限,.NET Compact Framework不使用vtables,因此必须通过遍历类和接口层次来调用方法,这是一项昂贵的操作. .NET Compact Framework维护已解析的虚拟调用的缓存,所以在大多数情况下,调用不需要重新解释.
    - ##### 尽可能使用字段(fields)而不是属性(properties).
    - ##### 定义值类型时,覆盖GetHashCode和Equals方法.如果它们没有被覆盖,那么运行时会在基类ValueType类中为这些方法使用通用版本.
    - ##### 谨慎使用反射.将反射用于未实例化类的调查目的会影响应用程序中实例化对象的性能.
    - ##### 请注意,在某些情况下,直接从文件中读取应用程序数据可能比使用ResourceManager更有效.ResourceManager将会查找文件系统中的多个位置,以找到最佳匹配用于组装它找到的二进制资源.为工作使用适当的工具.
