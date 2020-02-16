<h1 align="center">Smart-Doc Gradle Plugin</a></h1>

[中文文档](https://github.com/shalousun/smart-doc-maven-plugin/blob/master/README_CN.md)

## Introduce
smart-doc-gradle-plugin is a `gradle` plugin developed by the smart-doc official team. 
This plugin is available from smart-doc 1.8.1.
Using smart-doc-gradle-plugin makes it easier to integrate smart-doc into your project, and integration is more lightweight. 
You no longer need to write unit tests in your project to
Start smart-doc to scan source code analysis and generate API documents. 
You can run the `gradle` command directly or click on the preset` goal` of the smart-doc-maven-plugin in the IDE to generate API documentation. 
smart-doc-gradle-plugin will also make smart-doc's ability to generate API documentation more powerful.
[About smart-doc](https://github.com/shalousun/smart-doc)

## Getting started
### Add plugin
```
<plugin>
    <groupId>com.github.shalousun</groupId>
    <artifactId>smart-doc-maven-plugin</artifactId>
    <version>1.0.2</version>
    <configuration>
        <!--Specify the configuration file used to generate the document-->
        <configFile>./src/main/resources/smart-doc.json</configFile>
        <!--smart-doc implements automatic analysis of the dependency tree to load the source code of third-party dependencies. If some framework dependency libraries are not loaded, an error is reported, then use excludes to exclude-->
        <excludes>
            <!-The format is: groupId: artifactId; refer to the following->
            <exclude>com.google.guava:guava</exclude>
        </excludes>
    </configuration>
    <executions>
        <execution>
            <!--Comment out phase if you don't need to start smart-doc when compiling-->
            <phase>compile</phase>
            <goals>
                <goal>html</goal>
            </goals>
        </execution>
    </executions>
</plugin>
```
### Create a json config 
Create a json configuration file in your project. The smart-doc-maven-plugin plugin will use this configuration information.
For example, create `/src/main/resources/smart-doc.json` in the project. 
The configuration contents are as follows.

**Minimize configuration:**
```
{
   "allInOne": true, // whether to merge documents into one file, generally recommended as true
   "isStrict": false,//If the strict mode is set to true, Smart-doc forces that the public method in each interface in the code has a comment.
   "outPath": "/src/main/resources" //Set the api document output path.
}
```
Only three configuration items are required to use the smart-doc-maven-plugin to generate API documentation. In fact, only outPath must be configured.

**Detailed configuration content:**

When you need to use smart-doc to generate more API document information, you can add detailed configuration content.
```
{
  "serverUrl": "http://127.0.0.1", // Set the server address, not required
  "isStrict": false, // whether to enable strict mode
  "allInOne": true, // whether to merge documents into one file, generally recommended as true
  "outPath": "D: // md2", // Specify the output path of the document
  "coverOld": true, // Whether to overwrite old files, mainly used for mardown file overwrite
  "packageFilters": "", // controller package filtering, multiple package names separated by commas
  "md5EncryptedHtmlName": false, // only used if each controller generates an html file
  "projectName": "smart-doc", // Configure your own project name
  "skipTransientField": true, // Not currently implemented
  "dataDictionaries": [// Configure the data dictionary, no need to set
    {
      "title": "Order Status", // The name of the data dictionary
      "enumClassName": "com.power.doc.enums.OrderEnum", // Data dictionary enumeration class name
      "codeField": "code", // The field name corresponding to the data dictionary dictionary code
      "descField": "desc" // Data dictionary object description information dictionary
    }
  ],

  "errorCodeDictionaries": [{// error code list, no need to set
    "title": "title",
    "enumClassName": "com.power.doc.enums.ErrorCodeEnum", // Error code enumeration class
    "codeField": "code", // Code field name of the error code
    "descField": "desc" // Field name corresponding to the error code description
  }],

  "revisionLogs": [// Set document change records, no need to set
    {
      "version": "1.0", // Document version number
      "status": "update", // Change operation status, generally: create, update, etc.
      "author": "author", // Document change author
      "remarks": "desc" // Change description
    }
  ],
  "customResponseFields": [// Customly add fields and comments. If api-doc encounters a field with the same name later, directly add a comment to the corresponding field. It is not necessary.
    {
      "name": "code", // Override the response code field
      "desc": "Response code", // Override field comment of response code
      "value": "00000" // Set the value of the response code
    }
  ],
  "requestHeaders": [// Set global request headers, no need to set
    {
      "name": "token",
      "type": "string",
      "desc": "desc",
      "required": false,
      "since": "-"
    }
  ]
}
```
**Note:** The above json configuration is completely converted into json using the smart-doc's ApiConfig. 
So the project configuration can also refer to the introduction of smart-doc.
### Generated document
#### Use Maven command
```
// Generate html
mvn -Dfile.encoding = UTF-8 smart-doc: html
// Generate markdown
mvn -Dfile.encoding = UTF-8 smart-doc: markdown
// Generate adoc
mvn -Dfile.encoding = UTF-8 smart-doc: adoc
// Generate postman collection
mvn -Dfile.encoding = UTF-8 smart-doc: postman
```
**Note:** Under the window system, if you use the maven command line to perform document generation, 
non-English characters may be garbled, so you need to specify `-Dfile.encoding = UTF-8` during execution.

View maven's coding
```
# mvn -version
Apache Maven 3.3.3 (7994120775791599e205a5524ec3e0dfe41d4a06; 2015-04-22T19:57:37+08:00)
Maven home: D:\ProgramFiles\maven\bin\..
Java version: 1.8.0_191, vendor: Oracle Corporation
Java home: D:\ProgramFiles\Java\jdk1.8.0_191\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "dos"
```
#### Use IntelliJ IDEA
On Use IntelliJ IDE, if you have added smart-doc-maven-plugin to the project, 
you can directly find the plugin smart-doc plugin and click to generate API documentation.

![smart-doc-maven-plugin](https://raw.githubusercontent.com/shalousun/smart-doc-maven-plugin/master/images/idea.png)

### Generated document example
#### Interface header rendering
![header](https://images.gitee.com/uploads/images/2019/1231/223538_be45f8a9_144669.png "header.png")
#### Request parameter example rendering
![request-params](https://images.gitee.com/uploads/images/2019/1231/223710_88933f55_144669.png "request.png")
#### Response parameter example renderings
![response-fields](https://images.gitee.com/uploads/images/2019/1231/223817_32bea6dc_144669.png "response.png")
## Building
you can build with the following commands. (Java 1.8 is required to build the master branch)
```
mvn clean install -Dmaven.test.skip=true
```
## Other reference
- [Smart-doc manual](https://github.com/shalousun/smart-doc/wiki)

## Who is using
These are only part of the companies using smart-doc, for reference only. If you are using smart-doc, please [add your company here](https://github.com/shalousun/smart-doc/issues/12) to tell us your scenario to make smart-doc better.

![iFLYTEK](https://raw.githubusercontent.com/shalousun/smart-doc/dev/images/known-users/iflytek.png)
&nbsp;&nbsp;<img src="https://raw.githubusercontent.com/shalousun/smart-doc/dev/images/known-users/oneplus.png" title="OnePlus" width="83px" height="83px"/>
&nbsp;&nbsp;<img src="https://raw.githubusercontent.com/shalousun/smart-doc/dev/images/known-users/xiaomi.png" title="Xiaomi" width="170px" height="83px"/>
<img src="https://raw.githubusercontent.com/shalousun/smart-doc/dev/images/known-users/yuanmengjiankang.png" title="yuanmengjiankang" width="260px" height="83px"/>
<img src="https://raw.githubusercontent.com/shalousun/smart-doc/dev/images/known-users/zhongkezhilian.png" title="zhongkezhilian" width="272px" height="83px"/>
<img src="https://raw.githubusercontent.com/shalousun/smart-doc/dev/images/known-users/puqie_gaitubao_100x100.jpg" title="puqie" width="83px" height="83px"/>
## License
smart-doc-maven=plugin is under the Apache 2.0 license.  See the [LICENSE](https://raw.githubusercontent.com/shalousun/smart-doc-maven-plugin/master/LICENSE) file for details.
## Contact
Email： 836575280@qq.com
