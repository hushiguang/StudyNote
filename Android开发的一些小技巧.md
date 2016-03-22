Android 开发小技巧

1.碰到<code>scrollView</code>嵌套<code>listView</code>或recycleView 优先选择listView添加headView
  嵌套listView的item高度问题，重新计算子View的高度  焦点事件

2.android:clipToPadding的用法；
  android:clipChildren的用法；

3.Android查看运行的前台界面的activity
  adb shell "dumpsys window windows | grep -E 'mCurrentFocus'"

4.滑动冲突事件
  1). Viewpager 和 ListView 冲突 外部和内部的冲突，左右冲突  判段左右位移距离
  2). ScrollView 和 ListView  上下冲突

5.activity 有ScrollView时候windowSoftInputMode 不起作用
	如果你在 manifest 中把一个 activity 设置成 android:windowSoftInputMode="adjustResize"，那么 ScrollView（或者其它可伸缩的 ViewGroups）会缩小，从而为软键盘腾出空间。但是，如果你在 activity 的主题中设置了 android:windowFullscreen="true"，那么 ScrollView 不会缩小。这是因为该属性强制 ScrollView 全屏显示。然而在主题中设置 android:fitsSystemWindows="false" 也会导致 adjustResize 不起作用；

6.Application 数据的存储
	不要在Application中存储重要的一些数据,所以我们小数据应该公告onSaveInstance保存，大数据就通过文件保存。

7.dp，sp，px之间的装换

	//转换dip为px 
	  public static int convertDipOrPx(Context context, int dip) { 
	      float scale = context.getResources().getDisplayMetrics().density; 
	      return (int)(dip*scale + 0.5f*(dip>=0?1:-1)); 
	  } 
	   
	  //转换px为dip 
	  public static int convertPxOrDip(Context context, int px) { 
	      float scale = context.getResources().getDisplayMetrics().density; 
	     return (int)(px/scale + 0.5f*(px>=0?1:-1)); 
	 } 
	 
	public static int sp2px(Context context, float spValue) {
	        float fontScale = context.getResources().getDisplayMetrics().scaledDensity;
	        return (int) (spValue * fontScale + 0.5f);
	    }

	public static int px2sp(Context context, float pxValue) {
	        float fontScale = context.getResources().getDisplayMetrics().scaledDensity;
	        return (int) (pxValue / fontScale + 0.5f);
	    }

	/***
     * 获取手机状态栏的高度
     *
     * @param context
     * @return
     */
    public static int getStatusBarHright(Context context) {
        Class c = null;
        int y;
        try {
            c = Class.forName("com.android.internal.R$dimen");
            Object obj = c.newInstance();
            Field field = c.getField("status_bar_height");
            int x = Integer.parseInt(field.get(obj).toString());
            y = context.getResources().getDimensionPixelSize(x);
            Log.e("TAG", y + "");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (NoSuchFieldException e) {
            e.printStackTrace();
        }
        return y;
    }

8.Json格式的错误
	当有 String "can't convert jsonOnject" 的时候切记看下Json格式前面是否有多余字符？

9.ANR问题，且在activity退出之后，取消掉上个页面的线程操作	

10.设置软键盘位数字number

	edittext.setInputType(EditorInfo.TYPE_CLASS_PHONE);

11.bitmap及时回收

12.所有的图片务必先用 "<https://tinypng.com/>" 压缩一遍

13.使用CardView切记，否则AndroidL以上和L以下的版本显示的padding不一致

	app:cardPreventCornerOverlap="true"
	app:cardUseCompatPadding="true"

14.科学计数法的类 DecimalFormat ，
	文件大小格式化工具 Formatter.formatFileSize() 
	字母索引工具类 AlphabetIndexer

15.显示密码的操作

	edittext.setTransformationMethod(HideReturnsTransformationMethod.getInstance()); //显示密码
	edittext.setTransformationMethod(PasswordTransformationMethod.getInstance());	 //隐藏密码

16.文本加超链接
	Linkify.addLinks();

17.按两次返回键退出的操作
	判断toast.getView.getParent 是否为null即可
	
	 private void exitToast(){
	        if (mToast.getView().getParent() != null){
	            System.exit(0);
	        }else {
	            mToast.show();
	        }
	 }
	
18.没有滚动是因为 要滚动到的位置，已经在屏幕里面了，这时候是不滚动的，
	只有要滚到的位置没有在屏幕上，才会滚动。
	mLayoutManager = mRecyclerView.getLayoutManager();  
	mLayoutManager.scrollToPositionWithOffset(pos, 0);  

19.手机Wifi去叹号
	
	adb shell "settings put global captive_portal_server www.v2ex.com"

20.DrawLayout 记得设置无阴影模式 (去除黑色)

	mDrawLayout.setScrimColor(getResources().getColor(android.R.color.transparent));
	
21.占位，可以用 Space 来取代 View。 最棒的一点是Space可以跳过 Draw 这个过程。

22.android listview中的消息被软键盘遮挡了,在设置listview的时候加上android:transcriptMode="normal"就好了

23.TextView默认上下是有一定的padding的，有时候我们可能不需要上下这部分留白，加上它即可。

	includeFontPadding="false"

24.ViewDragHelper,做过自定义ViewGroup的童鞋都应该知道这个东西吧，用来处理触摸事件的神器，

25.Activity.recreate重新创建Activity。立马刷新当前Activity，而不会有明显的重启Activity的动画。

26.android:animateLayoutChanges 这是一个非常酷炫的属性。在父布局加上 android:animateLayoutChanges="true" 后，
	如果触发了layout方法（比如它的子View设置为GONE），系统就会自动帮你加上布局改变时的动画特效！！

27.RecyclerView 可以直接在布局里面设置管理器

	app:layoutManager="android.support.v7.widget.RecyclerView.LayoutManager"

