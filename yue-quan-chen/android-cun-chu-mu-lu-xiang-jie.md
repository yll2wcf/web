#### 内部存储空间中的应用私有目录

- data/data/app package name：WebView 缓存页面信息，SharedPreferences 和 SQLiteDatabase 持久化应用相关数据等
- 当用户卸载 App 时，系统自动删除 data/data 目录下对应包名的文件夹及其内容。
- getFilesDir()
- getCacheDir()
- 宿主 App 可以直接读写内部存储空间中的应用私有目录

#### 外部存储空间中的应用私有目录

- /storage/emulated/0/Android/data/app package name
- 同属于应用私有目录，当用户卸载 App 时，系统也会自动删除外部存储空间下的对应 App 私有目录文件夹及其内容。
- getExternalFilesDir()
- getExternalCacheDir()
- 4.4 版本开始，宿主 App 可以直接读写本应用外部存储空间中的应用私有目录，不再需要显式声明这读写权限

#### 外部存储空间中的公共目录

- 拍照类应用的图片文件，用户是使用浏览器手动下载的文件等
- Environment.getExternalStoragePublicDirectory(String type);
	- DIRECTORY_MUSIC：/storage/emulated/0/Music
	- DIRECTORY_MOVIES：/storage/emulated/0/Movies
	- DIRECTORY_PICTURES：/storage/emulated/0/Pictures
	- DIRECTORY_DOWNLOADS：/storage/emulated/0/Download
- 访问外部存储空间时记得申请读写权限

#### 外部存储空间中的其他目录

- Environment.getExternalStorageDirectory();
