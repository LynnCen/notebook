# Agency9文档

### **Agency9.Animation**

  动画表示具有播放控件的动画播放器。动画可用于在3DMaps中设置许多对象的动画。

  要使用动画的引用，请使用animate（）方法获取Maps实例。

##### 方法

##### **bindEvent**(String eventType ,function(Exportable event){})

  将此动画生成的事件绑定到JavaScript函数回调。

  支持的事件类型:"**animationStarted**", "**animationUpdated**", "**animationCompleted**", "**animationCancelled**", "**animationStateChanged**".

- **eventType**---要绑定的事件的类型
- **callback**---事件发生时要调用的函数



##### **cancel()**

  通过停止播放来取消此动画。

  如果动画是非持久的，则调用cancel（）将播放状态设置为“dispossed”，即使动画设置为无限循环。因此它再也不能播放了。

  如果动画是持久的，则调用cancel（）将播放状态设置为“idle”，当前时间设置为0。然后可以通过调用play（）再次播放它。



##### dispose()

  处置此动画，使其播放状态变为“disposed”，这意味着它再也不能运行。动画的当前时间将设置为0。



##### finish()

  根据动画是正在播放还是正在反转，通过查找动画的任一端来完成此动画。如果动画设置为无限循环多次，则无论动画是在播放、反转还是暂停，在调用finish（）时，动画的当前时间将变为无穷大。

  请注意，如果动画“paused”而未设置为无限循环，则当前时间将设置为0。

  调用finish（）后，播放状态将设置为“finished”，除非动画是非持久性的，并且未设置为无限循环。然后播放状态将被设置为“disposed”，这意味着它永远不能再播放。



##### getCurrentTime()

返回此动画的当前时间（秒）。

**return** --动画的当前时间



##### getPlaybackRate()

返回此动画的播放速率。

有关播放速率的详细信息，请参见setPlaybackRate（）方法。

**return** -- the playback rate



##### getPlayState()

返回此动画的播放状态。可能的状态有：

"**idel**" --  当前动画时间为0。

"**pending**" --  动画正在等待某些挂起的任务完成。

"**running**" --  动画正在播放

"**paused**" --  动画已暂停，当前时间属性未更新。

"**finished**"  --  动画已达到其边界之一，当前时间属性未更新。

"**disposed**"  --  动画已释放，无法再次运行。

**return**---    动画的状态



##### pause()

暂停播放此动画，阻止其当前时间更新。播放状态将设置为“暂停”。



##### play()

开始或继续播放此动画，将播放状态设置为“正在运行”。

如果动画播放状态为“完成”，则无论动画的当前时间如何，调用play（）将从开始（或结束-如果设置为反向播放）开始播放动画。

如果播放状态为“disposed”，则调用play（）将无效。



##### setCurrentTime(double t)

设置此动画的当前时间（以秒为单位）.

**t -- **动画的当前时间



##### setPlaybackRate(double rate)

设置此动画的播放速率。该速率可以看作是正常播放速率的倍增（动画时间轴中的1秒将需要1秒才能实时完成）。

默认值为1。

**rate --**  播放速率



##### unbindEvent（String eventType）

解除先前绑定到此动画的事件。

**eventType --**  解除绑定的事件类型



### **Agency9.Animation.GroupEffect**

例如，GroupEffect是由两个或多个keyframeEffect组成的组。

使用GroupEffect时，底层效果将并行设置动画。

GroupEffect可以传递给Maps实例的animate（）方法，也可以传递给SequenceEffect的构造函数（可以在其中添加其他效果），后者又传递给Maps实例的animate（）方法。

##### 构造函数

##### Agency9.Animation.GroupEffect(AnimationEffect[] effects)

构造一个给定效果列表的GroupEffect。



### Agency9.Animation.KeyFrame

关键帧用于动画中，其中每个关键帧描述一组属性值，这些属性值应在动画中的特定偏移处指定给动画对象。例如，在**cbezier**函数中，关键帧之间的插值值是通过计算的。

##### 构造函数

##### **Agency9.ANimation.KeyframeEffect**(**Animation** animation , **Keyframe**[]keyframes,**Timing** timing, JavaScriptObject montionPaths)

在给定可设置动画的对象（如摄影机、关键帧、计时和运动路径）的情况下，构造新的关键帧效果。

有关完整的示例，请参见Keyframe的构造函数。

##### **Agency9.ANimation.KeyframeEffect**(**Animation** animation , **Keyframe**[]keyframes,**Timing** timing)

在给定可设置动画的对象（如PointStyle、关键帧和计时）的情况下构造新的KeyframeEffect。

有关完整的示例，请参见Keyframe的构造函数

 

### **Agency9.Animation.MotionPath3D**

MotionPath3D就像它听起来是3D中的运动路径，通常在动画中使用。CatmullRomCurve3D、EllipseCurve3D和LineCurve3D等曲线可以附加到MotionPath3D。实际运动路径从附加的第一条曲线开始，到最后一条曲线的末端结束。

通过使用关键帧，如果需要，可以将运动路径限制为MotionPath3D的一段。有关详细信息，请参见关键帧对象的构造函数。

##### 构造函数

##### Agency9.Animation.MotionPath3D()

构造一个新的空MotionPath3D。如果使用此构造函数，请不要忘记使用append（）方法附加至少一条曲线。

##### Agency9.Animation.MotionPath3D(Curve3D[] curves)

在给定包含一条或多条曲线的曲线列表的情况下，构造MotionPath3D。

##### 方法

##### append(Curve3D curve)

将曲线（例如CatmullRomCurve3D、EllipseCurve3D或LineCurve3D）附加到此MotionPath3D。

curve - 要附加的曲线

##### getCurveEnd(int curveIndex)

返回一个介于0和1之间的值，该值是具有给定curveIndex的曲线末端的运动路径的“进度”。

将返回值传递给getPointAt（u）方法以检索3D位置。

**curveIndex** -- MotionPath3D中曲线的索引

 **return** -- 返回一个0 到1之间的值

##### getlength()

返回此MotionPath3D的总长度（单位）。

**return -- ** MotionPath3D的长度

##### getNumCurves()

返回包含在该MotionPath3D的pathNumber。

**return -- ** 曲线的数量

##### getPointAt(double u)

返回运动路径上某个点在u处的三维位置，该值介于0和1之间，其中0是路径的起点，1是终点。

**u -- ** 介于0-1之间的值

**return -- ** 路径上u处的三维位置

##### getSegmentEnd(int curveIndex,int segmentIndex)

给定curveIndex和segmentIndex，返回介于0和1之间的值。返回的值是该曲线段末端的运动路径的“进度”。

将返回值传递给getPointAt（u）方法以检索3D位置。

**curveIndex  -- ** 此MotionPath3D中曲线的索引

 **segmentIndex -- ** 曲线的段索引

**return -- ** 介于0-1之间的值

### **Agency9.Animation.SequenceEffect**

SequenceEffect是两个或多个KeyframeEffects或GroupEffects的序列。

使用SequenceEffect时，基础效果将以串联方式设置动画。

SequenceEffect可以传递给Maps实例的animate（）方法。

##### 构造函数

##### **Agency9.Animation.SequenceEffect**（AnimationEffect[] effects）

构造给定效果列表的SequenceEffect。



### **Agency9.Animation.Timing**

计时对象包含用于调整动画等行为的设置。这些设置包括迭代次数、持续时间、方向、使用哪个缓和函数（插值函数）等等。

##### 静态方法

**Agency9.Animation.Timing.DIRECTION_ALTERNATE** 交叉

**Agency9.Animation.Timing.DIRECTION_ALTERNATE_REVERSE** 交叉反转

**Agency9.Animation.Timing.DIRECTION_NORMAL ** 正常 

**Agency9.Animation.Timing.DIRECTION_REVERSE**  反转

##### 构造函数

##### **Agency9.Animation.Timing（）**

Constructs a new Timing.

##### Agency9.Animation.Timing（JavascriptObject jso)

构造一个新的计时，给定一个JavaScript对象jso，其中包含计时设置。

example json :

```
 var timing = new Agency9.Animation.Timing({
         "delay": 0,               // 启动前延迟（秒）
         "iterations": 0,          // 运行多少次（0=无限次）
         "duration": 10,           // 动画从第一个关键帧到最后一个关键帧的时间，以秒为单位
         "direction": "alternate", //normal, reverse, alternate, alternate-reverse
         "easing": "ease-in-out"   // linear, ease, ease-in, ease-in-out, ease-out
    });
```

##### 方法

##### getDelay()

返回此计时的延迟（秒）。

**return -- ** 延迟

##### getDirection()

返回为此计时设置的方向。

有关要与返回值进行比较的常量名称的详细信息，请参见setDirection（）方法。

**return -- ** 方向

##### getDuration()

返回此计时的持续时间（秒）。

如果实现未设置 则返回0

**return-- ** 计时器的持续时间

##### getEasing()

返回设置为此计时的插值函数。

**return -- ** 插值函数 interpolation function

##### getIterations()

返回此计时的迭代次数。

0表示无线次

**return -- ** 迭代次数

##### setDirection(String direction)

设置此方向的计时。有效方向是:

```
     Agency9.Animation.Timing.DIRECTION_NORMAL
     Agency9.Animation.Timing.DIRECTION_REVERSE
     Agency9.Animation.Timing.DIRECTION_ALTERNATE
     Agency9.Animation.Timing.DIRECTION_ALTERNATE_REVERSE
```

默认的方向是 DIRECTION_NORMAL.

**direction - ** 设置的方向

##### setDuration(double duration)

设置此计时的持续时间（以秒为单位）。

**duration -- ** 方向

##### setEasing(InterpolationFunction interpolationFunction)

将easing 设置为计时器，一个像CubicBezier的插值函数

默认值 Agency9.Interpolation.CubicBezier.LINEAR.

**interpolationFunction** --  设置的插值函数

##### setIterations(int iterations)

将迭代次数设置为此计时。

 值为0表示迭代次数无限。默认值为1。

**iterations -- ** 迭代次数或0表示无限

 





















