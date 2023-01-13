---
title: 在 IIS Express 中使用 IIS CORS Module
date: 2023-01-10 18:06:08 +0800
tags: [IIS Express, IIS CORS Module, CORS]
---

## 安装环境
- Visual Studio 2022
- IIS
- IIS CORS Module
  - 下载地址 https://www.iis.net/downloads/microsoft/iis-cors-module

## 步骤
- 修改解决方案配置文件，路径：{解决方案目录}\\.vs\\{项目名称}\config\applicationhost.config 
    1. 在 sectionGroup 节添加 cors
    ```xml
    <sectionGroup name="system.webServer">
        ...
        <section name="cors" overrideModeDefault="Allow" />
    </sectionGroup>
    ```

    2. 在 globalModules 节添加 CorsModule
    ```xml
    <globalModules>
        ...
        <add name="CorsModule" image="%SystemRoot%\system32\inetsrv\iiscors.dll" />
    </globalModules>
    ```

    3. 在 location path="" 节添加 CorsModule
    ```xml
    <location path="" overrideMode="Allow">
        <system.webServer>
            <modules>
            ...
            <add name="CorsModule" />
            </modules>
        ...
    ```

- 将 CORS 的构架信息添加到 IIS Express 架构中  
  - 拷贝 C:\Windows\System32\inetsrv\config\schema\cors_schema.xml 到 C:\Program Files\IIS Express\config\schema 目录  
- 根据需要修改 web.config  
    请参照[IIS CORS module](https://learn.microsoft.com/en-us/iis/extensions/cors-module/cors-module-configuration-reference) 修改
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration>
        <system.webServer>
            <cors enabled="true" failUnlistedOrigins="true">
                <add origin="*" />
                <add origin="https://*.microsoft.com"
                    allowCredentials="true"
                    maxAge="120"> 
                    <allowHeaders allowAllRequestedHeaders="true">
                        <add header="header1" />
                        <add header="header2" />
                    </allowHeaders>
                    <allowMethods>
                        <add method="DELETE" />
                    </allowMethods>
                    <exposeHeaders>
                        <add header="header1" />
                        <add header="header2" />
                    </exposeHeaders>
                </add>
                <add origin="http://*" allowed="false" />
            </cors>
        </system.webServer>
    </configuration>
    ```
- 参考信息
  - [在IIS Express中使用IIS CORS模块](https://qiita.com/nt-7/items/9f892b67980901f1a378)
  - [IIS CORS 模块配置参考](https://learn.microsoft.com/en-us/iis/extensions/cors-module/cors-module-configuration-reference)
