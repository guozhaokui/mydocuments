
##调试node
有一个.settings的目录，里面保存运行和调试相关配置。调试的时候会附加到5858端口。实际就是启动了一个新的node进程，并且附加上去。
* 怎么找到node的  
如果都删掉的话，就会提示:  
    can't find runtime 'node' on PATH  
* 如何调试其他进程呢?  
怎么知道node的位置，可以添加新的类型么，例如laya
    initialize: can't create debug session for adapter ID 'laya'
##结构
> Code\app-0.5.0\content_shell.pak
这里面应该有runtime,debug等关键词。pak并不是一个标准的zip文件。
```javascript
{Runtime._remoteBase="http://localhost:8000/inspector-sources/";}
...
{this._debugFrontend=!!Runtime.queryParam("debugFrontend")||(window["InspectorTest"]&&window["InspectorTest"]["debugTest"]);var descriptors=InspectorFrontendHostAPI.EventDescriptors;for(var i=0;i<descriptors.length;++i)
```