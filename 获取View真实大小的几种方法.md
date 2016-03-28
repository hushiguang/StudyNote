# 在<code>activity</code>获取<code>View</code>的真实大小
  
  在获取View的真实大小的时候，会在activity的<code>onCreate()</code>方法中去获取，但是发现拿出的都是0.
  so,总结下几个方法.
  

  * <code>Activity/View#onWindowFocusChanged</code>
  
