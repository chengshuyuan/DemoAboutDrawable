# Android的Drawable
## 1 Drawable简介
- 1 Drawable表示的是一种可以在Canvas上进行绘制的抽象的概念，都表示一种图像的概念。最常见的颜色和图片都可以是一个Drawable
- 2 非图片类型Drawable占用空间较小，这对减小apk的大小也很有帮助
- 3 Drawable的大小
    - Drawable的内部宽高这个参数比较重要，通过getIntrinsicWidth\getIntrinsicHeight这两个方法可以获取他们
    - Drawable的内部宽高不等同于它的大小，一般来说，drawable是没有大小概念的，用作View的背景时，Drawable会被拉伸至View的同等大小
## 2 Drawable的分类
### 2.1 BitmapDrawable
- 1 几乎是最简单的Drawable了，表示一张图片
        
        <xml version="1.0" encoding="utf-8"?>
        <bitmap
            xmls:android="http://schemas.android.com/apk/res/android"
            android:src="xxx" //资源的id
            android:antialias="Boolean" //是否开启抗锯齿功能
            android:dither="Boolean"  //是否开启防抖动效果 
            android:filter="Boolean"  //是否开启过滤效果
            android:gravity=["top" | "bottom" | "left" | "right"]
            android:mapMap="Boolean" //是否纹理映射
            android:tileMode=["disabled" | "clamp" | "repeat" | "mirror"]  //平铺模式          
        />
    - 1 android:antialias 是否开启抗锯齿功能。 
        - 开启后会让图片变得平滑，同时也会在一定程度上降低图片的清晰度，但是这个降低的幅度较低以至于可以忽略不计，因此抗锯齿选项应该开启
    - 2 android:dither 是否开启抖动效果。
        - 当图片的像素质量和手机屏幕的像素匹配不一致时，开这个选项可以让高质量的图片在低质量的屏幕上还能保持较好的显示效果。比如图片的色彩模式为ARGB8888,但是设备屏幕所支持的色彩模式是RGB555,这个时候开启抖动选项可以让图片显示不过于失真。
        - 在Android中创建Bitmap一般会选用ARGB8888这个模式，即ARGB四个同道各8位，在这种色彩模式下，一个像素所占的大小为4个字节
    - 3 android:filter 是否开启过滤效果
        - 当图片尺寸被拉伸或者被压缩时，开启过滤效果尅保持较好的显示效果
    - 4 android:gravity 当图片小于容器的尺寸时，设置此选项可以对图片进行定位，可以采用 | 来组合使用
    - 5 android:tileMode 平铺模式
        - 有以下几个值 ["disable" | "clamp" | "repeat" | "mirror"]
        - disable表示关闭平铺模式，也是默认值，当开启平铺模式后，gravity属性会被忽略
        - repeat 表示简单的水平和垂直方向上的平铺效果
        - mirror 表示一种在水平和垂直方向上的镜面投影效果
        - clamp 图片的四角的像素会扩展到周围区域
### 2.2 ShapeDrawable
- 1 ShareDrawable可以理解为通过颜色来构造的图像，它既可以时纯色的图形，也可以时具有渐变效果的图像。
    
        <?xml version="1.0" encoding="utf-8"?>
        <Shape    
            xmlns:android="http://schemas.android.com/apk/res/android"
            android:shape=["rectangle" | "oval" | "line" | "ring"]>
            <corners
                anroid:radius="Integer"
                android:topLeftRadius="Integer"
                android:topRightRadius="Integer"
                android:bottomLeftRadius="Integer"
                andorid:bottomRightRadius="Integer"
                />
            <gradient
                android:angle="Integer" //渐变的角度，其值必须是45的倍数
                android:centerX="Integer" //渐变中心点的横坐标
                android:centerY="Integer" //渐变中心点的纵坐标
                android:startColor="color" //渐变的起始色
                android:centerColor="color" //渐变的中间色
                android:endColor="color"//渐变的结束色
                android:gradientRadius="Integer"//渐变半径，但是仅当type为radial时有效
                android:type=["linear" | "radial" | "sweep"]
                android:useLevel="Boolean" //一般为false，当Drawable为StateListDrawable时使用true
                />
            <padding
                android:left="Integer"
                android:top="Integer"
                android:right="Integer"
                android:bottom="Integer"
                />    
            <size
                android:witdh="Integer"
                android:height="Integer"
                />
            <solid
                android:color="color"
                />    
            <stroke
                android:width="Integer"
                android:color="color"
                android:dashWidth="Integer"
                android:dashGap="Integer"
                />    
            
        <Shape/>
        
     - 1 android:shape 图像的形状
         - 1 有四个选项["rectangle" | "oval" | "line" | "ring"],默认值是rectangle
         - 2 line和ring两个选项必须要通过<stroke>标签来指定线的宽度、颜色等信息，否则无法达到预期的效果
     - 2 <corners> 四个角度
         - 1 只适用于巨型shape
         - 2 五个属性中，android:radius的优先级最低，会被其他四个覆盖
     - 3 <gradient> 渐变效果
         - 1 与<solid>标签是互斥的，solid表示纯色填充，而gradient则表示渐变效果
         - 2 android:type=["linear" | "radial" | "sweep"], linear(线性渐变) radial(径向渐变)sweep(扫描渐变)
     - 4 <solid> 纯色填充
     - 5 <stroke> Shape的描边
         - 1 android:dashWidth:组成虚线的线段宽度
         - 2 android:dashGap: 组成虚线的线段之间的间隔，间隔越大虚线看起来空隙越大
         - 3 dashWidth和dashGap如果有一个为0，虚线的效果不生效
     - 6 <padding>表示空白
         - 1 它表示的不是shape的空白，而是包办它的View的空白
     - 7 <size> shape的大小
         - 1 它表示的Shape的固有大小，但是它并不是shape最终显示的大小
         - 2 shape是没有宽高的概念的，作为View的背景它会自适应View的宽高
         - 3 通过<shape>标签设置了shape的大小，getIntrinsicWith() getIntrinsicHeight()函数就可以得到，否则这两个函数返回-1
 - 2 需要注意的是<shape>标签创建的Drawable,其实体类是GradientDrawable 
### 2.3 LayerDrawable
- 1 LayerDrawable对应的标签时<layer-list>,它表示一种层次化的Drawable集合，通过将不同的Drawable放置在不同的层上边从而达到一种叠加后的效果。
        
        <?xml version="1.0" encoding="utf-8"?>
        <layer-list
            xmlns:android="http://schemas.android.com/apk/res/android">
            <item
                android:drawable="res"
                android:id=""
                android:top="dimension"
                android:right="dimension"
                android:bottom="dimension"
                android:left="dimension"
            />
        </layer-list>
    - 1 每个layer-list中可以包哈多个item，每个item都表示一个drawable
    - 2 默认情况下，么个drawable会被压缩到View的大小
### 2.4 StateListDrawable
- 1 StateListDrawable对应于<selector>标签，它也表示Drawable集合，每个drawable对应着View的一种状态
      
        <?xml version="1.0" encoding="utf-8"?>
        <selector xmlns:android="http://schemas.android.com/apk/res/android"
            android:constantSize="Boolean"
            android:dither="boolean"
            android:variablePadding="boolean">
            <item
                android:drawable="res"
                android:state_pressed="boolean"
                android:state_focused="boolean"
                android:state_hovered="boolean"
                android:state_selected="boolean"
                android:state_checkable="boolean"
                android:state_enabled="boolean"
                android:state_activated="boolean"
                android:state_window_focused="boolean"
            />
        </selector> 
           
     - 1 android:constantSize 
        - StateListDrawable的固有大小是否跟随其状态的改变而改变   
        - 因为状态的改变会导致StateListDrawable切换到具体的Drawable，而不同的Drawable具有不同的固定大小
        - true表示StateListDrawable的固定大小保持不变，这时它的固有大小时内部所有Drawable的固有大小的最大值
        - false则会随着状态的改变而改变，默认为false
     - 2 android:dither
        - 是否开启抖动效果，开启此选项可以让图片在低质量的屏幕上依然能获得较好的显示效果，默认为true
     - 3 android:variablePadding
        - StateListDrawable的padding表示是否随着其状态的改变而改变
        - false表示StateListDrawable的padding是内部所有Drawable的padding的最大值，默认为诶false
- 2 系统会根据View当前状态从selector中选择对应的item，每个item对应一个具体的drawable，系统会按照从上到下的顺序查找，直到查找到第一条匹配的item
- 3 一般来说，selector的最后一条并不附带任何的状态，当所有的状态都没有匹配到时，系统会选择默认的item
        
### 2.4  LevelListDrawable
- 1 LevelListDrawable对应于<level-list>标签，它同样表示一个Drawable的集合，集合中的每个Drawable表示一个等级概念，根据不同的level，LevelListDrawable会切换到对应的Drawable
        
        <?xml version="1.0" encoding="utf-8"?>
        <level-list
           xmls:android="http://schemas.android.com/apk/res/android">
           <item
               android:drawable="res"
               android:maxLevel="Integer"
               android:minLevel="Integer"
           />
        </level-list>
        
     - 1 每个item表示一个drawable，并且有对应的等级范围(由android:maxLevel和android:minLevel确定，最小值和最大值之间的等级会对应这个item)
     - 2 Drawable的等级是有范围的，即0-10000 
       
### 2.5 TransitionDrawable
- 1 TransitionDrawable对应于<transition>标签，它用于实现两个Drawable之间的淡入和淡出效果
       
        <?xml version="1.0" encoding="utf-8"?>
        <transition
            xmls:android="http://schemas.android.com/apk/res/android">
            <item
                android:drawable="res"
                android:id="@+id/id"
                android:top="dimension"
                android:left="dimension"
                android:right="dimension"
                android:bottom="dimension"
            />
        </transition>           
        
        <TextView 
            android:id="@+id/textview"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:backgroud="@drawable/transition_drawable"/>
        
        //最后通过startTransition和reverseTransition方法来实现淡入和淡出的效果以及逆过程
        TransitionDrawable drawable = (TransitionDrawable) textview.getBackground();
        drawable.startTransition(1000);   
        
###  2.6 InsertDrawable
- 1 InsertDrawable对应于标签<insert>,它可以将其他Drawable内嵌到自己当中，并且可以在四周留下一定的兼具。
- 2 当一个View希望自己的背景比自己的实际区域小的时候，可以采用InsertDrawable来实现    
   
        <?xml version="1.0" encoding="utf-8"?>
            <insert
                xmls:android="http://schemas.android.com/apk/res/android">
                <item
                    android:drawable="res"
                    android:id="@+id/id"
                    android:insertTop="dimension"
                    android:insertLeft="dimension"
                    android:insertRight="dimension"
                    android:insertBottom="dimension"
                />
            </insert>  
            
### 2.7 ScaleDrawable
- 1 ScaleDrawable对应于<scale>标签，它可以根据自己的等级将指定的Drawable缩放到一定的比例            
