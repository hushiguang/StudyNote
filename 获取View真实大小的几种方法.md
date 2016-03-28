# 在<code>activity</code>获取<code>View</code>的真实大小
  
  在获取View的真实大小的时候，会在activity的<code>onCreate()</code>方法中去获取，但是发现拿出的都是0.
  so,总结下几个方法.来自《Android开发艺术探索》的View的工作原理
  

  * <code>Activity/View#onWindowFocusChanged</code>

  	<code>onWindowFocusChanged</code>方法的含义：<code>View</code>已经初始化完毕了，宽和高已经准备好了，这个时候去获取高和宽都是没问题的，需要注意的是，<code>onWindowFocusChanged</code>会被调用多次，当<code>activity</code>继续执行和暂停执行的时候，<code>onWindowFocusChanged</code>均会被调用，如果频繁的进行<code>onResume</code>和<code>onPause</code>,那么<code>onWindowFocusChanged</code>也会被频繁的调用.

  	<pre><code>
  	   @Override
	    public void onWindowFocusChanged(boolean hasFocus) {
	        super.onWindowFocusChanged(hasFocus);
	        Log.d("TAG","onWindowFocusChanged");

	        if (hasFocus) {
	            int width = view.getMeasuredWidth();
	            int height = view.getMeasuredHeight();
	            Log.d("TAG", "width " + width + " height " + height);
	        }

	    }
	</code></pre>


  * <code>view.post(runnable)</code>

  	通过post可以将一个<code>Runnable</code>投递到消息队列的尾部，然后等待<code>Lopper</code>调用此<code>Runnable</code>的时候,><code>View</code>也已经初始化好了.

  	<pre><code>
  		 view.post(new Runnable() {
            @Override
            public void run() {
                int width = view.getMeasuredWidth();
                int height = view.getMeasuredHeight();
                Log.d("TAG", "width " + width + " height " + height);
            }
        });
  	</code></pre>

  * <code>ViewTreeObserver</code>

  	使用<code>ViewTreeObserver</code>的众多回调可以完成这个功能，比如使用<code>OnGlobalLayoutListener</code>这个接口，当<code>View</code>树的状态发生变化或者<code>View</code>树内部的<code>View</code>的可见性发生改变的时候，<code>onGlobaLayout</code>的方法将会被回调，因此这是获取<code>View</code>的高度和宽一个很好的时机，需要注意的是，伴随着<code>View</code>树的状态改变等，<code>onGlobaLayout</code>挥别调用多次.

  	<pre><code>
  		ViewTreeObserver viewTreeObserver = view.getViewTreeObserver();
        viewTreeObserver.addOnGlobalLayoutListener(new ViewTreeObserver.OnGlobalLayoutListener() {
            @Override
            public void onGlobalLayout() {
                view.getViewTreeObserver().removeOnGlobalLayoutListener(this);
                int width = view.getMeasuredWidth();
                int height = view.getMeasuredHeight();
                Log.d("TAG", "width " + width + " height " + height);
            }
        });
  	</code></pre>







  
