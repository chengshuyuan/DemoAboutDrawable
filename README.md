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
## 3 自定义Drawable
