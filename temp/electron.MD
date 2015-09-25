* 能用来调试普通的v8程序么

* 怎么编译。  
使用gyp。
python script/bootstrap.py -v
需要在gitbash下工作，因为需要执行git命令
注意必须要python2.7
    d:\python27\python.exe e:\electron1\script\update.py
    Download directxsdk-ia32      155008  [10.2%]    

Traceback (most recent call last):
  File "vendor\brightray\vendor\gyp\gyp_main.py", line 18, in <module>
    sys.exit(gyp.script_main())
  File "vendor\brightray\vendor\gyp\pylib\gyp\__init__.py", line 532, in script_main
    return main(sys.argv[1:])
  File "vendor\brightray\vendor\gyp\pylib\gyp\__init__.py", line 525, in main
    return gyp_main(args)
  File "vendor\brightray\vendor\gyp\pylib\gyp\__init__.py", line 510, in gyp_main
    generator.GenerateOutput(flat_list, targets, data, params)
  File "vendor\brightray\vendor\gyp\pylib\gyp\generator\ninja.py", line 2382, in GenerateOutput
    pool.map(CallGenerateOutputForConfig, arglists)
  File "d:\python27\lib\multiprocessing\pool.py", line 251, in map
    return self.map_async(func, iterable, chunksize).get()
  File "d:\python27\lib\multiprocessing\pool.py", line 567, in get
    raise self._value
UnicodeDecodeError: 'ascii' codec can't decode byte 0xb7 in position 2521: ordinal not in range(128)
    
    
* 调试主进程
系统自带的只能调试页面中的脚本。

* 关于调试
    用node-inspector中转。
    C:\Users\guozhaokui>node-inspector
    Node Inspector v0.5.0
       info  - socket.io started
    Visit http://127.0.0.1:8080/debug?port=5858 to start debugging.
    Received request for a method not implemented: Debugger.setSkipAllPauses
    
    

* main process ,render process

* v8引擎
    node和浏览器是一个还是各用各的。
    不同进程肯定是各用各的。同一个进程是 一起。
    
* webview 标签
    用一个独立的进程来渲染某个页面。
    怎么调试这个标签的内容。可以打开调试，但是是一个新的窗口
    1. 怎么能并列一个窗口，显示的是另外一个webview的调试信息
        webview并不是一个独立的窗口。
        只要新开的webview的url是调试url就行
    能仿照这个做一个自己的渲染窗口么。
    
* vscode
    也在resource/app目录下
    resources\app\out\vs\workbench\electron-main