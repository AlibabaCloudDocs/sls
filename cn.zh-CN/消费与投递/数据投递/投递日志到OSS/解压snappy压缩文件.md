# 解压snappy压缩文件

日志服务将日志投递到OSS时，支持通过snappy压缩OSS文件。投递成功后，您可以通过C++ Lib、Java Lib、Python Lib和Linux环境解压工具等方式解压OSS文件。

## 使用C++ Lib解压缩

在[snappy](https://github.com/google/snappy)页面下载Lib，执行Snappy.Uncompress方法解压。

## 使用Java Lib解压缩

下载[xerial snappy-java](https://github.com/xerial/snappy-java)，使用Snappy.Uncompress或Snappy.SnappyInputStream解压缩，不支持SnappyFramedInputStream。

**说明：** 1.1.2.1版本存在[bug](https://github.com/xerial/snappy-java/issues/142) ，可能无法解压部分压缩文件，1.1.2.6及以后版本已修复该问题，建议使用最新的Java Lib版本解压缩。

```
<dependency>
<groupId>org.xerial.snappy</groupId>
<artifactId>snappy-java</artifactId>
<version>1.0.4.1</version>
<type>jar</type>
<scope>compile</scope>
</dependency>
```

-   Snappy.Uncompress

    ```
    String fileName = "C:\\我的下载\\36_1474212963188600684_4451886.snappy";
    RandomAccessFile randomFile = new RandomAccessFile(fileName, "r");
    int fileLength = (int) randomFile.length();
    randomFile.seek(0);
    byte[] bytes = new byte[fileLength];
    int byteread = randomFile.read(bytes);
    System.out.println(fileLength);
    System.out.println(byteread);
    byte[] uncompressed = Snappy.uncompress(bytes);
    String result = new String(uncompressed, "UTF-8");
    System.out.println(result);
    ```

-   Snappy.SnappyInputStream

    ```
    String fileName = "C:\\我的下载\\36_1474212963188600684_4451886.snappy";
    SnappyInputStream sis = new SnappyInputStream(new FileInputStream(fileName));
    byte[] buffer = new byte[4096];
    int len = 0;
    while ((len = sis.read(buffer)) != -1) {
        System.out.println(new String(buffer, 0, len));
    }
    ```


## 使用Python Lib解压缩

1.  下载并安装[Python压缩库](https://pypi.org/project/python-snappy/)。

2.  执行解压代码。

    解压缩代码示例如下：

    ```
    import snappy
    compressed = open('/tmp/temp.snappy').read()
    snappy.uncompress(compressed)
                                
    ```

    **说明：** 不支持通过以下命令行工具解压snappy压缩文件，命令行模式下仅支持hadoop模式（hadoop\_stream\_decompress）与流模式（stream\_decompress）。

    ```
    $ python -m snappy -c uncompressed_file compressed_file.snappy
    $ python -m snappy -d compressed_file.snappy uncompressed_file                            
    ```


## 使用Linux环境解压工具解压缩

针对Linux环境，日志服务提供可以解压snappy文件的工具，单击[snappy\_tool](https://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/snappy_tool)下载工具并执行以下代码解压，其中03\_1453457006548078722\_44148.snappy、03\_1453457006548078722\_44148请根据实际情况替换。

```
./snappy_tool 03_1453457006548078722_44148.snappy 03_1453457006548078722_44148
compressed.size: 2217186
snappy::Uncompress return: 1
uncompressed.size: 25223660
```

