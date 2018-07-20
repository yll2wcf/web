## debug模式下build 会遇到 `React/RCTBridgeModule.h' file not found` 的问题

解决办法，先选择targets下的React 进行一次编译，成功后，再选择回project，进行编译

## release模式下，构建或编译时遇到 `React/RCTBridgeModule.h' file not found` 的问题

To solve the issue, you have to do the following:

1. In Xcode, go to the project scheme \(Product -&gt; Scheme -&gt; Manage Scheme -&gt; double click your project\).
2. Click on the 'Build' option at the left pane.
3. Uncheck 'Parallelize Build' under **Build Options**.
4. Then in **Targets **section, click ' **+** ' button then search for 'React'. Select it and click ' **Add **'.
5. 'React' should now appear under **Targets **section. Click and drag it to the top so that it will be the first item in the list \(before your project\).
6. Clean the project and build.

Note: You might still have similar header issue with other libraries \(e.g. react-native-fbsdk\) that are referring to those react native .h files.

