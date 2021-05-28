# 解压Snappy压缩文件

日志服务将日志投递到OSS时，支持通过Snappy压缩OSS文件。投递成功后，您可以通过C++ Lib、Java Lib、Python Lib、Linux工具等方式解压OSS文件。

## 使用C++ Lib解压

1.  [下载Snappy Lib](https://github.com/google/snappy)。

2.  使用Snappy.Uncompress解压。


## 使用Java Lib解压

1.  选择以下任意一种方式下载Java Lib。

    **说明：** 1.1.2.1版本存在Bug ，可能无法解压部分压缩文件。1.1.2.6及以上版本已修复该问题，建议您使用1.1.2.6及以上版本的Java Lib。关于该Bug的更多信息，请参见[bad handling of the MAGIC HEADER](https://github.com/xerial/snappy-java/issues/142)。

    -   手动方式

        [xerial snappy-java](https://github.com/xerial/snappy-java)

    -   Maven方式

        ```
        <dependency>
        <groupId>org.xerial.snappy</groupId>
        <artifactId>snappy-java</artifactId>
        <version>1.0.4.1</version>
        <type>jar</type>
        <scope>compile</scope>
        </dependency>
        ```

2.  使用以下任意一种方法解压。

    **说明：** 不支持使用SnappyFramedInputStream。

    -   Snappy.Uncompress

        示例代码如下：

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

        示例代码如下：

        ```
        String fileName = "C:\\我的下载\\36_1474212963188600684_4451886.snappy";
        SnappyInputStream sis = new SnappyInputStream(new FileInputStream(fileName));
        byte[] buffer = new byte[4096];
        int len = 0;
        while ((len = sis.read(buffer)) != -1) {
            System.out.println(new String(buffer, 0, len));
        }
        ```


## 使用Python Lib解压

1.  [下载Python Lib](https://pypi.org/project/python-snappy/)。

2.  使用snappy.uncompress解压。

    -   Python 2示例代码如下：

        ```
        import snappy
        compressed = open('/tmp/temp.snappy').read()
        snappy.uncompress(compressed)
        ```

    -   Python 3示例代码如下：

        ```
        import snappy
        compressed = open('/tmp/temp.snappy','rb').read()
        print(snappy.uncompress(compressed).decode(encoding='utf-8',errors="ignore"))
        ```

    **说明：**

    -   如果您在Windows系统下，使用snappy.uncompress解压出现报错，例如`UnicodeDecodeError: 'gbk' codec can't decode byte 0xff in position 0: illegal multibyte sequence`，可能是Windows文件系统以BOM开头导致的。建议您使用类Unix系统或者在open函数的encoding中设置正确的文件编码。
    -   不支持通过以下命令行工具解压snappy压缩文件，命令行模式下仅支持hadoop模式（hadoop\_stream\_decompress）与流模式（stream\_decompress）。

        ```
        python -m snappy -d compressed_file.snappy uncompressed_file                            
        ```


## 使用Linux工具解压

针对Linux环境，日志服务提供可以解压Snappy文件的工具。

1.  [下载snappy\_tool](https://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/snappy_tool)。

2.  执行以下命令解压。

    请根据实际情况替换03\_1453457006548078722\_44148.snappy和03\_1453457006548078722\_44148。

    ```
    ./snappy_tool 03_1453457006548078722_44148.snappy 03_1453457006548078722_44148
    ```

    返回结果示例：

    ```
    compressed.size: 2217186
    snappy::Uncompress return: 1
    uncompressed.size: 25223660
    ```


