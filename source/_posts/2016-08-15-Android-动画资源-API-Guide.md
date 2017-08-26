---
title: Android API Guide - 动画资源
date: 2016-08-15 11:21:52
category:
- Android
- API_Guide
tags:
- 动画
---


[原文链接](https://developer.android.com/guide/topics/resources/animation-resource.html?hl=zh-cn)

## 动画资源

[** 属性动画 (Property Animation)** ](https://developer.android.com/guide/topics/resources/animation-resource.html?hl=zh-cn#Property)
	使用[Animator](https://developer.android.com/reference/android/animation/Animator.html?hl=zh-cn) 在一段时间内不断的改变对象的属性值
	
[** 视图动画（View Animation） **](https://developer.android.com/guide/topics/resources/animation-resource.html?hl=zh-cn#View) 
有两种类型：
* [补间动画(Tween)][tween_anim]: 在一个单张图片上进行一系列变换，使用[Animation][Animation] 创建。
* [帧动画(Frame)][frame_anim]: 显示一系列图片作为动画，使用[AnimationDrawable][AnimationDrawable]

### 1. 属性动画
使用XML定义如何改变目标对象的属性，如 背景色、透明度，并指定变换时间
- 文件路径
	`res/animator/filename.xml` 文件名会被用作资源ID

- 编辑后对于资源数据类型：
	[ValueAnimator][ValueAnimator],[ObjectAnimator][ObjectAnimator],[AnimatorSet][AnimatorSet]
- 资源引用方式：
	* Java : `R.animator.filename`
	* XML: `@[package:]animator/filename`
- 格式：
    
```
<set
	android:ordering=["together" | "sequentially"]>
	<objectAnimator
		android:propertyName="string"
		android:duration="int"
		android:valueFrom="float | int | color"
		android:valueTo="float | int | color"
		android:startOffset="int"
		android:repeatCount="int"
		android:repeatMode=["repeat" | "reverse"]
		android:valueType=["intType" | "floatType"]/>
	<animator
		android:duration="int"
		android:valueFrom="float | int | color"
		android:valueTo="float | int | color"
		android:startOffset="int"
		android:repeatCount="int"
		android:repeatMode=["repeat" | "reverse"]
		android:valueType=["intType" | "floatType"]/>
	<set>
	...
	</set>
</set>
```

[tween_anim]: https://developer.android.com/guide/topics/resources/animation-resource.html?hl=zh-cn#Tween "Tween animation"
[frame_anim]: https://developer.android.com/guide/topics/resources/animation-resource.html?hl=zh-cn#Frame "Frame animation"
[Animation]:https://developer.android.com/reference/android/view/animation/Animation.html?hl=zh-cn 
[AnimationDrawable]: https://developer.android.com/reference/android/graphics/drawable/AnimationDrawable.html?hl=zh-cn 
[ValueAnimator]:https://developer.android.com/reference/android/animation/ValueAnimator.html?hl=zh-cn
[ObjectAnimator]:https://developer.android.com/reference/android/animation/ObjectAnimator.html?hl=zh-cn
[AnimatorSet]:https://developer.android.com/reference/android/animation/AnimatorSet.html?hl=zh-cn






### 2. 视图动画 
####     2.1  补间动画
* 文件路径：
`res/anim/filename.xml`

* 编译后资源对于数据类型：
[`Animation`](https://developer.android.com/reference/android/view/animation/Animation.html?hl=zh-cn)

* 引用资源：
Java : `R.anim.filename`
XML: `@[package:]anim/filename`

* 格式：
    
```
	<?xml version="1.0" encoding="utf-8"?>
	<set xmlns:android="http://schemas.android.com/apk/res/android"
		android:interpolator="@[package:]anim/interpolator_resource"
		android:shareInterpolator=["true" | "false"] >
		<alpha
			android:fromAlpha="float"
			android:toAlpha="float" />
		<scale
			android:fromXScale="float"
			android:toXScale="float"
			android:fromYScale="float"
			android:toYScale="float"
			android:pivotX="float"
			android:pivotY="float" />
		<translate
			android:fromXDelta="float"
			android:toXDelta="float"
			android:fromYDelta="float"
			android:toYDelta="float" />
		<rotate
			android:fromDegrees="float"
			android:toDegrees="float"
			android:pivotX="float"
			android:pivotY="float" />
		<set>
			...
		</set>
	</set>
```
##### 2.1.1 插值器（Interpolators）

#### 2.1.2 自定义插值器
- 略



####     2.2 帧动画
按顺序显示一系列图片

* 文件路径
	`res/drawable/filename.xml`
* 编译后对应资源类型：
	[AnimationDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimationDrawable.html?hl=zh-cn)
* 引用方式：
Java: `R.drawable.filename`
XML: `@[package:]drawable.filename`
* 格式:
```
	<?xml version="1.0" encoding="utf-8"?>
	<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
		android:oneshot=["true" | "false"] >
		<item
			android:drawable="@[package:]drawable/drawable_resource_name"
			android:duration="integer" />
	</animation-list>
```
* 示例：
```
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android"
    android:oneshot="false">
    <item android:drawable="@drawable/rocket_thrust1" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust2" android:duration="200" />
    <item android:drawable="@drawable/rocket_thrust3" android:duration="200" />
</animation-list>
//------- Java 代码
ImageView rocketImage = (ImageView) findViewById(R.id.rocket_image);
rocketImage.setBackgroundResource(R.drawable.rocket_thrust);
rocketAnimation = (AnimationDrawable) rocketImage.getBackground();
rocketAnimation.start();
```