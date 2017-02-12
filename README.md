URLRedirector
=============

使用 WebExtension 方式编写的 Firefox URL 重定向插件。

插件的开发受 [gooreplacer](https://github.com/jiacai2050/gooreplacer) 启发，由于 gooreplacer 在 MAC 下运行时会遇到停止运行的问题，因此采用 WebExtension 方式实现，目前测试结果良好。

插件可以处理 URL 的自动重定向，可用于帮助网页开发和调试，以及解决国内无法访问 google CDN 之类的问题（ stackoverflow 等国外网站再也不用等了^_^）。

插件仅支持正则表达式替换方式，不支持通配符。

供用户使用的在线规则有:

[https://raw.githubusercontent.com/fengyc/URLRedirector/master/tools/gooreplacer.gson](https://raw.githubusercontent.com/fengyc/URLRedirector/master/tools/gooreplacer.gson)

[https://raw.githubusercontent.com/fengyc/URLRedirector/master/tools/rules.json](https://raw.githubusercontent.com/fengyc/URLRedirector/master/tools/rules.json)

[极客族的在线规则](http://cdn.geekzu.org/cached.html) :

[https://raw.githubusercontent.com/fengyc/URLRedirector/master/tools/rules_geekzu.json](https://raw.githubusercontent.com/fengyc/URLRedirector/master/tools/rules_geekzu.json)

AMO 的上架地址（已上线） [https://addons.mozilla.org/zh-CN/firefox/addon/urlredirector/](https://addons.mozilla.org/zh-CN/firefox/addon/urlredirector/)

AMO 的上架审核时间很长，因此做了一个只签名不上架的版本，在 tools 目录下。

安装插件前，请升级 firefox 到最新版本（48+）。

版本和特性列表
-------

版本变更历史请查看 CHANGELOG 。

目前支持的特性：

1. 支持在线规则、在线规则自动更新
2. 支持自定义规则

限于 WebExtension 的能力，目前不支持导入、导出配置。

其它说明
--------

1. 规则示例

    简单规则，将 a.com 替换为 b.com ：
    
        原始地址 a.com  目标地址 b.com
        原始请求 https://www.a.com
        重定向到 https://www.b.com
    
    简单正则表达式：
    
        原始地址 a.*.com 目标地址 b.com
        原始请求 https://www.abc.com
        重定向到 https://www.b.com
        
    使用原始地址的部分信息：
    
        原始地址 (\d+).com  目标地址 com/$1
        原始请求 https://www.a.1.com
        重定向到 https://www.a.com/1

2. 内部实现

    内部规则 Rule 为简单 javascript 对象：
    
        {
            description: <规则描述，可选>
            origin: <原始地址正则表达式，必需>
            exclude: <排除地址正则表达式，可选>,
            methods: <原始请求方法列表，可选>
            types: <URL 资源类型列表，可选>
            target: <目标地址正则表达式，必需>
            enable: <是否启用，可选>
        }
    
    不在 Rule 对象描述中的字段可随意添加，不影响结果；
    请求方法可为 GET / POST / DELETE 等等，如 `["GET", "POST"]`；
    URL 资源类型见 [ResourceType](https://developer.mozilla.org/en-US/Add-ons/WebExtensions/API/webRequest/ResourceType) 。
    
    在线规则使用一个包含了规则列表的 json 格式的文件表示：
    
        {
            version: <版本号，可选>,
            rules: <规则列表，必须>
        }
    
    整合了在线规则和规则对象的示例文件见 `tools` 目录下的 `rules_demo.json`。
    
    目前支持两种格式，通过 version 进行区分。
    （1）不包含 version 或 version < 1.0 时，使用 gooreplacer 定义的格式，见 `tools/gooreplacer.gson` 或 [https://github.com/jiacai2050/gooreplacer4chrome/raw/master/gooreplacer.gson](https://github.com/jiacai2050/gooreplacer4chrome/raw/master/gooreplacer.gson) ；
    （2）包含 version 且 version >= 1.0 时，使用新格式，见 `tools/rules.json` 。
    
    两种格式的主要区别在于 rules 字段，在 gooreplacer 中 rules 为对象，在新格式中，rules 为列表（为了在后期能更加方便地支持规则排序）。

由于在线规则使用 json 格式，因此需要注意 json 转义符问题， [json 在线校验工具](http://json.cn/) 可检查文件内容是否符合 json 格式。

用户定制的本地规则优先于在线规则，可覆盖在线规则。

（ grunt 是个好工具，打包真方便）


帮助 URLRedirector
----------------

我需要你的帮助来提高 URLRedirector 的功能和质量，你可以从以下几个（但不限于）方面提供你力所能及的帮助：

1. 在 issue 中提出你遇到的问题或需要的功能
2. fork 本仓库，贡献你的代码或文档，并提交 pull request
3. 参与功能和 bug 的讨论

……


License
-------

Copyright © 2016 Yingcai FENG

The MIT License (MIT)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
