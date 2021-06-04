# Decompress Snappy compressed files

When you ship data from Log Service to Object Storage Service \(OSS\), you can use Snappy to compress OSS objects After you ship the data to OSS, you can use Snappy decompressor for C++ Lib, Snappy decompressor for Java Lib, Snappy decompressor for Python Lib, and Snappy decompressor for Linux to decompress the OSS objects.

## Use Snappy decompressor for C++ Lib to decompress the OSS objects

1.  [Download Snappy Lib](https://github.com/google/snappy).

2.  Use snappy.uncompress to decompress the OSS objects.


## Use Snappy decompressor for Java Lib to decompress the OSS objects

1.  Select one of the following methods to download Java Lib:

    **Note:** Java Lib 1.1.2.1 has bugs and may fail to decompress some OSS objects. We recommend that you use Java Lib 1.1.2.6 and later. The bugs are fixed in these versions. For more information about the bugs, see [bad handling of the MAGIC HEADER](https://github.com/xerial/snappy-java/issues/142).

    -   Click the following link to manually download Java Lib:

        [xerial snappy-java](https://github.com/xerial/snappy-java)

    -   Add the following dependency in a Maven project to download Java Lib:

        ```
        <dependency>
        <groupId>org.xerial.snappy</groupId>
        <artifactId>snappy-java</artifactId>
        <version>1.0.4.1</version>
        <type>jar</type>
        <scope>compile</scope>
        </dependency>
        ```

2.  Select one of the following methods to decompress the OSS objects:

    **Note:** SnappyFramedInputStream is not supported.

    -   Snappy.Uncompress

        The following code is an example:

        ```
        String fileName = "C:\\Downloads\\36_1474212963188600684_4451886.snappy";
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

        The following code is an example:

        ```
        String fileName = "C:\\Downloads\\36_1474212963188600684_4451886.snappy";
        SnappyInputStream sis = new SnappyInputStream(new FileInputStream(fileName));
        byte[] buffer = new byte[4096];
        int len = 0;
        while ((len = sis.read(buffer)) != -1) {
            System.out.println(new String(buffer, 0, len));
        }
        ```


## Use Snappy decompressor for Python Lib to decompress the OSS objects

1.  [Download Python Lib](https://pypi.org/project/python-snappy/).

2.  Use snappy.uncompress to decompress the OSS objects.

    The following code is an example:

    ```
    import snappy
    compressed = open('/tmp/temp.snappy').read()
    snappy.uncompress(compressed)
    ```

    **Note:**

    -   If an error occurs when you use snappy.uncompress to decompress the OSS objects in Windows, for example `UnicodeDecodeError: 'gbk' codec can't decode byte 0xff in position 0: illegal multibyte sequence`, the probable cause is that the files in the Windows file system start with BOM. We recommend that you use a UNIX-like system, or set a correct file number in the encoding of the open function.
    -   The following command cannot be used to decompress the OSS objects. This command is supported only in Hadoop mode \(hadoop\_stream\_decompress\) and streaming mode \(stream\_decompress\).

        ```
        python -m snappy -d compressed_file.snappy uncompressed_file                            
        ```


## Use Snappy decompressor for Linux to decompress the OSS objects

Log Service provides Snappy decompressor for Linux to decompress Snappy files.

1.  [Download snappy\_tool](https://logservice-resource.oss-cn-shanghai.aliyuncs.com/tools/snappy_tool).

2.  Run the following command to decompress the OSS objects:

    Replace 03\_1453457006548078722\_44148.snappy and 03\_1453457006548078722\_44148 with the actual value.

    ```
    ./snappy_tool 03_1453457006548078722_44148.snappy 03_1453457006548078722_44148
    ```

    Sample response:

    ```
    compressed.size: 2217186
    snappy::Uncompress return: 1
    uncompressed.size: 25223660
    ```


