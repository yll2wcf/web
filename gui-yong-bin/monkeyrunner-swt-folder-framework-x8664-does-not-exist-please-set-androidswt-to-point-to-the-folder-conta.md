monkeyrunner.bat运行报错。
SWT与AWT相似，framework\x86_64没有找到，
Please set ANDROID_SWT to point to the folder containing swt.jar for your platform.由这句话说明monkeyrunner.bat里面的文件指向错误。
所以用文件编辑器打开monkeyrunner.bat进行修改。
（1）。set frameworkdir=lib （纠正）==》set frameworkdir=..\lib
（2）。Dcom.android.monkeyrunner.bindir=..\framework -jar %jarpath% %* （纠正）==》Dcom.android.monkeyrunner.bindir=..\..\platform-tools -jar %jarpath% %*

---------------------

本文来自 毁灭天君 的CSDN 博客 ，全文地址请点击：https://blog.csdn.net/qq_21650171/article/details/78986788?utm_source=copy 