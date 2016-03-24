# View树的绘制

View Tree Measure 
* ViewGroup Measure
<pre><code> 
	    /**
	     * @param widthMeasureSpec The width requirements for this view //widthMeasureSpec该视图的宽度要求
	     * @param heightMeasureSpec The height requirements for this view  //heightMeasureSpec该视图的高度要求
	     */
    protected void measureChildren(int widthMeasureSpec, int heightMeasureSpec) {
        final int size = mChildrenCount;  //所有子View的数量
        final View[] children = mChildren;
        for (int i = 0; i < size; ++i) {  //遍历所有子View 跳过所有View状态为GONE的
            final View child = children[i];
            if ((child.mViewFlags & VISIBILITY_MASK) != GONE) {
            	//让所有子View去测量自己本身
                measureChild(child, widthMeasureSpec, heightMeasureSpec);
            }
        }
    }
</code></pre>
<pre><code>
	     /**
	     * @param child The child to measure //需要测量的View
	     * @param parentWidthMeasureSpec The width requirements for this view  //widthMeasureSpec该视图的宽度要求
	     * @param parentHeightMeasureSpec The height requirements for this view //heightMeasureSpec该视图的高度要求
	     */
    protected void measureChild(View child, int parentWidthMeasureSpec,
            int parentHeightMeasureSpec) {
        //拿到该View的布局参数
        final LayoutParams lp = child.getLayoutParams();
        //获取该View的宽度的视图要求
        final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
                mPaddingLeft + mPaddingRight, lp.width);

        //获取该View的高度的视图要求
        final int childHeightMeasureSpec = getChild
        MeasureSpec(parentHeightMeasureSpec,
                mPaddingTop + mPaddingBottom, lp.height);

        // 用于获取 View 最终的大小 View测量
        child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
    }
</code></pre>
<pre><code>
	     /**
	     * @param spec The requirements for this view
	     * @param padding The padding of this view for the current dimension and
	     *        margins, if applicable
	     * @param childDimension How big the child wants to be in the current
	     *        dimension
	     * @return a MeasureSpec integer for the child
	     */
    public static int getChildMeasureSpec(int spec, int padding, int childDimension) {
        int specMode = MeasureSpec.getMode(spec); //所提供的测量规范的模式
        int specSize = MeasureSpec.getSize(spec); //所提供的措施规范大小
        int size = Math.max(0, specSize - padding); // 取一个大的数
        int resultSize = 0;
        int resultMode = 0;

        switch (specMode) {
        // Parent has imposed an exact size on us
        //家长已经强加给我们一个确切的大小
        case MeasureSpec.EXACTLY:
            if (childDimension >= 0) {
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size. So be it.
                //子view想占用全部的大小
                resultSize = size;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size. It can't be
                // bigger than us.
                //孩子是个不确认大小的
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

        // Parent has imposed a maximum size on us
        //
        case MeasureSpec.AT_MOST:
            if (childDimension >= 0) {
                // Child wants a specific size... so be it
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size, but our size is not fixed.
                // Constrain child to not be bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size. It can't be
                // bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

        // Parent asked to see how big we want to be
        case MeasureSpec.UNSPECIFIED:
            if (childDimension >= 0) {
                // Child wants a specific size... let him have it
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size... find out how big it should
                // be
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size.... find out how
                // big it should be
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            }
            break;
        }
        //返回View的大小和模式
        return MeasureSpec.makeMeasureSpec(resultSize, resultMode);
    }
</code></pre>

* 测量模式的计算

| 父ViewGroup模式  |子View模式 | 最终子View的模式  |
| :-------| --------:| :--: |
| EXACTLY  		| EXACTLY  		 |  EXACTLY  	|
| EXACTLY  		| AT_MOST  		 |  EXACTLY  	|
| EXACTLY 		| UNSPECIFIED	 |  AT_MOST  	|
| AT_MOST  		| EXACTLY  		 |  EXACTLY   	|
| AT_MOST  		| AT_MOST  		 |  AT_MOST  	|
| AT_MOST  		| UNSPECIFIED	 |  AT_MOST  	|
| UNSPECIFIED	| EXACTLY  		 |  EXACTLY  	|
| UNSPECIFIED	| AT_MOST  		 |  UNSPECIFIED	|
| UNSPECIFIED	| UNSPECIFIED	 |  UNSPECIFIED	|

* View Measure

<pre><code>
	protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
	        setMeasuredDimension(getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
	         getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
	    }
</code></pre>
  
	



	
