# SevenZipExtractor

可配合 [Costura.Fody](https://github.com/Fody/Costura) 使用的 [SevenZipExtractor](https://github.com/adoconnection/SevenZipExtractor)

C# `SevenZipExtractor` with `Costura.Fody`

### How To Do ?

> A = 当前的 SevenZipExtractor 项目

> B = 目标项目 将调用 SevenZipExtractor（已引用 `Costura.Fody`）

在此之前 B 先 NeGet 搜索引用 `Costura.Fody`，B 目录中将出现 `FodyWeavers.xml`

1. `git clone` 打开解决方案，解决方案资源管理器 选中 `SevenZipExtractor` 右键 生成，SevenZipExtractor.dll 将会出现在 `A/bin/Release/` 中

    - 跳过此步骤，可直接下载 [SevenZipExtractor.dll](https://github.com/Zneiat/SevenZipExtractor/releases)
    
2. 复制 `A 目录/bin/Release/SevenZipExtractor.dll` 到 `B 目录` 中

3. `B` 中引用 SevenZipExtractor.dll 并配置 `复制本地=False` 如图
    
    ![20180206134927](https://user-images.githubusercontent.com/22412567/35843845-5cbe0244-0b45-11e8-8e18-f5c05b8fbce7.png)
    
    ![20180206135034](https://user-images.githubusercontent.com/22412567/35843868-917fa60e-0b45-11e8-8e89-6621c7eede6a.png)
    
4. `B` 中创建目录 `Costura32` 和 `Costura64` 并将 `A 目录/bin/Release/(x86|x64)/7z.dll` 对应放入其中，对 .dll 文件进行配置 `生成操作=嵌入的资源, 复制到输出目录=不复制` 如图所示
    
    ![20180206135058](https://user-images.githubusercontent.com/22412567/35843916-c5fbf98c-0b45-11e8-85f9-ad923df7417f.png)

5. 配置 `FodyWeavers.xml`

    ```xml
    <Weavers>
      <Costura CreateTemporaryAssemblies='true'
               Unmanaged32Assemblies='7z|SevenZipExtractor'
               Unmanaged64Assemblies='7z|SevenZipExtractor'
               IncludeAssemblies='SevenZipExtractor'/>
    </Weavers>
    ```
    
    ![20180206135305](https://user-images.githubusercontent.com/22412567/35844059-72d64040-0b46-11e8-8529-45fd526a6ec3.png)
    
6. 好啦好啦 清理 再 生成 B 即可。现在就只有一个文件了，`7z.dll` 和 `SevenZipExtractor.dll` 都已嵌入单个 `.exe` 中

    ![20180206135433](https://user-images.githubusercontent.com/22412567/35844073-85a5568e-0b46-11e8-9aa7-cb24b0c30959.png)

    
# Original Introduction
C# wrapper for 7z.dll (x86 and x64 included) 

Based on code from: http://www.codeproject.com/Articles/27148/C-NET-Interface-for-Zip-Archive-DLLs

Supported formats:
* 7Zip
* APM
* Arj
* BZip2
* Cab
* Chm
* Compound
* Cpio
* CramFS
* Deb
* Dll
* Dmg
* Exe
* Fat
* Flv
* GZip
* Hfs
* Iso
* Lzh
* Lzma
* Lzma86
* Mach-O
* Mbr
* Mub
* Nsis
* Ntfs
* Ppmd
* Rar
* Rar5
* Rpm
* Split
* SquashFS
* Swf
* Swfc
* Tar
* TE
* Udf
* UEFIc
* UEFIs
* Vhd
* Wim
* Xar
* XZ
* Z
* Zip


#### NuGet
```
Install-Package SevenZipExtractor
```

#### Examples

```cs
using (ArchiveFile archiveFile = new ArchiveFile(@"Archive.ARJ"))
{
    archiveFile.Extract("Output"); // extract all
}

```

```cs
using (ArchiveFile archiveFile = new ArchiveFile(@"Archive.ARJ"))
{
    foreach (Entry entry in archiveFile.Entries)
    {
        Console.WriteLine(entry.FileName);
        
        // extract to file
        entry.Extract(entry.FileName);
        
        // extract to stream
        MemoryStream memoryStream = new MemoryStream();
        entry.Extract(memoryStream);
    }
}

```

#### 7z.dll
7z.dll (x86 and x64) will be added to your BIN folder automatically.


#### License
- Source code in this repo is licensed under The MIT License
- 7z binaries license http://www.7-zip.org/license.txt
