#### 3.0（可以显示或隐藏状态栏）

- View中加入了一个void setSystemUiVisibility (int visibility) 方法。随着该方法一同出来的有两个属性：STATUS_BAR_HIDDEN、STATUS_BAR_VISIBLE。并且还加入了View.OnSystemUiVisibilityChangeListener来监听系统UI的变化。

#### 4.0（优化隐藏状态栏效果，增加了隐藏导航栏）

- 把3.0种加入的STATUS_BAR_HIDDEN 换成了SYSTEM_UI_FLAG_LOW_PROFILE ， STATUS_BAR_VISIBLE 也换成了 SYSTEM_UI_FLAG_VISIBLE。另外还加入了SYSTEM_UI_FLAG_HIDE_NAVIGATION这个新标签。
- SYSTEM_UI_FLAG_LOW_PROFILE ：让状态栏或导航栏上的图标变暗和让一些不重要的图标消失。只要页面有任何交互，该标签会被清除，样式还原。
- SYSTEM_UI_FLAG_HIDE_NAVIGATION ：隐藏底部导航栏。同样如果用户与页面有任何交互，该标签会被清除，样式会还原。

#### 4.1（优化了隐藏状态栏与导航栏效果，内容可以延伸到其中）

- setSystemUiVisibility增加了SYSTEM_UI_FLAG_FULLSCREEN ，SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN ，SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION
- SYSTEM_UI_FLAG_LAYOUT_FULLSCREEN ，SYSTEM_UI_FLAG_LAYOUT_HIDE_NAVIGATION 这两个属性和上面提到的FULLSCREEN和HIDE_NAVIGATION属性很像，只是多了一个LAYOUT。区别是，这两个属性并不会真正隐藏状态栏或者导航栏，只是把整个content的可布局区域延伸到了其中。
- SYSTEM_UI_FLAG_LAYOUT_STABLE 此View一般和上面几个提到的属性一起使用，它可以保证手动隐藏状态栏或导航栏时，所有的view也都待在本来的位置上不会动。

#### 4.4（优化了隐藏状态栏与导航栏效果，可以设置状态栏与导航栏透明）

- 新增 android:windowTranslucentStatus，android:windowTranslucentNavigation 可以设置状态栏或者导航栏的背景为透明。
- 加入了两个标签 SYSTEM_UI_FLAG_IMMERSIVE ，SYSTEM_UI_FLAG_IMMERSIVE_STICKY 
- SYSTEM_UI_FLAG_IMMERSIVE ：相当于SYSTEM_UI_FLAG_HIDE_NAVIGATION+SYSTEM_UI_FLAG_FULLSCREEN，只有从屏幕上方下滑，或者从屏幕下方上滑时才会执行清除，其他普通交互不会变化。
- SYSTEM_UI_FLAG_IMMERSIVE_STICKY ：该标签与SYSTEM_UI_FLAG_IMMERSIVE作用差不多，只是该标签会让SYSTEM_UI_FLAG_HIDE_NAVIGATION和SYSTEM_UI_FLAG_FULLSCREEN 标签被短暂清除，而不是永久，一会儿后又会自动恢复。
- 实现沉浸式状态栏只能自己创建个view然后占位状态栏，并且顶部会有黑色阴影，多种方式实现：
	1. 设置Window透明之后，创建个view与状态栏大小一致，设置背景色。设置给decorView。
	2. 设置Window透明之后，设置toolbar与colorPrimaryDark颜色，然后跟布局设置android:fitsSystemWindows="true"属性，给系统预留状态栏高度，使内容不延伸到状态栏。
	3. 第二种方式，也可以给toolbar设置android:paddingTop="@dimen/toolbar_padding_top"，需要做状态栏高度兼容判断。然后就不用使用fitsSystemWindows属性。

#### 5.0（可以设置状态栏的颜色）

- 新增两个属性 android:statusBarColor，android:navigationBarColor 

#### 6.0（可以改变状态栏中的内容颜色）

- 加入了SYSTEM_UI_FLAG_LIGHT_STATUS_BAR标签和android:windowLightStatusBar