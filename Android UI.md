# View的绘制原理

Android View 的绘制原理主要基于视图的测量和布局过程，以及实际的绘制操作。这一过程通过多个步骤协同工作，最终在屏幕上呈现视图。

1. **测量阶段（Measure）**：
   - 当一个 View 被添加到布局中时，它首先会经历测量阶段。测量的主要目的是确定该 View 的宽度和高度。测量过程中，父视图会将自己的测量规格（如最大宽度和高度）传递给子视图，子视图基于这些规格进行测量。
   - 测量过程涉及 `onMeasure()` 方法，开发者可以在该方法中根据父视图的限制来计算视图的实际尺寸。

2. **布局阶段（Layout）**：
   - 在测量阶段结束后，View 会进入布局阶段，父视图会根据子视图的测量结果安排每个子视图的位置，确保子视图按照指定位置显示。
   - 布局过程通过调用 `onLayout()` 方法来完成，在这个方法中，我们可以定义每个子视图的边界（左、上、右、下）。

3. **绘制阶段（Draw）**：
   - 一旦视图的尺寸和位置被确定，接下来就是实际的绘制过程。绘制操作由 `onDraw()` 方法完成，该方法会绘制视图的内容（如文本、图像、形状等）。
   - 在 `onDraw()` 方法中，我们使用 `Canvas` 对象来绘制图形。`Canvas` 提供了丰富的绘制 API，用来绘制线条、矩形、圆形、文本等元素。
   
4. **刷新和重绘（Invalidate）**：
   - 如果视图的内容发生变化（如状态改变、位置改变等），可以通过调用 `invalidate()` 方法来请求重绘。`invalidate()` 会标记视图为需要重绘，随后在下一个绘制周期中调用 `onDraw()` 进行更新。

整个过程是通过视图树来完成的，即一个视图包含其他子视图，父视图负责对子视图的测量、布局和绘制。Android 的绘制系统通过这种方式实现了高度的定制性和灵活性。




# 如何自定义View
自定义 `View` 是 Android 开发中常见的需求，通常用于实现一些复杂的 UI 组件，或者对现有控件进行扩展。自定义 `View` 主要涉及到三个步骤：测量、布局和绘制。以下是如何自定义 `View` 的基本步骤和注意事项。

### 1. 继承 `View` 或相关控件
首先，创建一个新的自定义 `View` 类，继承自 `View` 或其子类（如 `TextView`、`ImageView` 等）。一般来说，如果你只需要实现一个简单的自定义组件，继承 `View` 即可；如果是需要扩展已有的控件功能，继承相关控件会更加方便。

```kotlin
class MyCustomView(context: Context, attrs: AttributeSet) : View(context, attrs) {
    // 在这里定义视图的属性、绘制、交互等
}
```

### 2. 重写 `onMeasure()` 方法
`onMeasure()` 方法是 `View` 测量过程中的关键，它决定了该视图的大小。通常在该方法中，你会根据父视图传入的 `MeasureSpec` 参数计算视图的宽度和高度。

```kotlin
override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
    val width = MeasureSpec.getSize(widthMeasureSpec)
    val height = MeasureSpec.getSize(heightMeasureSpec)

    // 根据需求设置视图的宽高
    setMeasuredDimension(width, height)
}
```

在该方法中，你需要使用 `MeasureSpec.getMode()` 和 `MeasureSpec.getSize()` 来获取父视图传递过来的测量模式和大小，然后根据这些信息来决定当前视图的尺寸。

### 3. 重写 `onLayout()` 方法
如果你的自定义视图包含子视图，需要使用 `onLayout()` 方法来确定子视图的位置和大小。对于简单的自定义视图，通常不需要重写这个方法。

```kotlin
override fun onLayout(changed: Boolean, left: Int, top: Int, right: Int, bottom: Int) {
    super.onLayout(changed, left, top, right, bottom)
    // 布局子视图
}
```

### 4. 重写 `onDraw()` 方法
`onDraw()` 方法负责绘制视图的内容，通常是在这里进行所有的图形绘制操作，如绘制矩形、圆形、文本等。

```kotlin
override fun onDraw(canvas: Canvas) {
    super.onDraw(canvas)
    
    val paint = Paint()
    paint.color = Color.RED
    canvas.drawCircle(width / 2f, height / 2f, 100f, paint)  // 绘制圆形
}
```

在 `onDraw()` 中，`Canvas` 是一个绘图工具，它提供了多种方法来绘制图形、文本等内容。你可以根据需要自定义视图的外观。

### 5. 处理触摸事件（如果需要）
如果你的自定义视图需要响应用户的交互，如点击、滑动等，需要重写 `onTouchEvent()` 方法来处理触摸事件。

```kotlin
override fun onTouchEvent(event: MotionEvent): Boolean {
    when (event.action) {
        MotionEvent.ACTION_DOWN -> {
            // 处理按下事件
        }
        MotionEvent.ACTION_MOVE -> {
            // 处理移动事件
        }
        MotionEvent.ACTION_UP -> {
            // 处理抬起事件
        }
    }
    return true
}
```

`onTouchEvent()` 方法允许你获取触摸事件的信息，进行交互逻辑处理。

### 6. 支持属性和样式（可选）
你可以通过 `attrs` 参数在 XML 中定义自定义属性，从而让自定义视图支持可定制的外观和行为。

```kotlin
val typedArray = context.obtainStyledAttributes(attrs, R.styleable.MyCustomView)
val customColor = typedArray.getColor(R.styleable.MyCustomView_customColor, Color.BLACK)
typedArray.recycle()
```

在 XML 布局中，你可以定义视图的属性，样式可以通过 `attrs` 进行访问。

### 7. 实现构造函数
自定义 `View` 需要实现多个构造函数，通常包括一个带 `Context` 和 `AttributeSet` 的构造函数（用于从 XML 加载），以及一个无参构造函数（用于动态创建视图）。

```kotlin
constructor(context: Context) : super(context)
constructor(context: Context, attrs: AttributeSet) : super(context, attrs)
constructor(context: Context, attrs: AttributeSet, defStyleAttr: Int) : super(context, attrs, defStyleAttr)
```

### 8. 在布局中使用自定义视图
在 XML 布局文件中，你可以像使用其他标准控件一样使用自定义视图：

```xml
<com.example.myapp.MyCustomView
    android:id="@+id/myCustomView"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:customColor="#FF0000" />
```

### 总结
自定义 `View` 需要关注测量（`onMeasure()`）、布局（`onLayout()`）和绘制（`onDraw()`）三个主要步骤。如果你的视图需要响应用户交互，还需要处理触摸事件。在开发过程中，确保保持高效的绘制和合适的交互逻辑，避免在 UI 线程中进行复杂的计算，从而保持流畅的用户体验。



# 自定义view与viewgroup的区别
`View` 和 `ViewGroup` 是 Android UI 组件的基础类，它们在自定义时有一些不同之处。

### 1. `View` 与 `ViewGroup` 的基本概念
- **`View`** 是 Android UI 系统中的基本组件，表示屏幕上的一个元素。它通常是一个单一的 UI 组件，例如按钮、文本框、图片等。`View` 只负责绘制和响应用户交互。
- **`ViewGroup`** 是 `View` 的子类，代表一个视图容器，它可以包含一个或多个 `View` 或 `ViewGroup`。`ViewGroup` 负责管理其包含的视图（如布局的作用），并处理布局和绘制等工作。

### 2. 主要区别

- **功能不同**：
  - `View` 只负责单一的 UI 元素的显示和交互，通常包括文本、图形、按钮等。
  - `ViewGroup` 作为容器，主要负责管理多个 `View` 或其他 `ViewGroup`，它通常用于布局的实现，像 `LinearLayout`、`FrameLayout`、`RelativeLayout` 等都继承自 `ViewGroup`。

- **布局管理**：
  - `View` 没有布局管理功能，不需要考虑子视图的位置和尺寸。
  - `ViewGroup` 需要考虑布局管理，它会根据传入的布局参数对其包含的子视图进行布局和调整。

- **测量和布局**：
  - `View` 在绘制过程中只需要关注自己的大小和位置。
  - `ViewGroup` 在 `onLayout()` 中需要遍历其所有子视图，对它们进行测量（通过 `onMeasure()`）并指定位置（通过 `onLayout()`）。

- **自定义时的区别**：
  - **自定义 `View`**：只需继承 `View` 类，重写 `onDraw()` 和 `onMeasure()` 等方法来实现自定义的 UI 元素。
  - **自定义 `ViewGroup`**：继承 `ViewGroup`，除了重写 `onDraw()` 和 `onMeasure()` 之外，还需要重写 `onLayout()` 方法来处理子视图的布局。

### 3. 使用场景
- **自定义 `View`**：当你需要一个独立的 UI 元素，并且不需要包含其他视图时，使用 `View`。例如，绘制一个自定义的图形、进度条或按钮。
- **自定义 `ViewGroup`**：当你需要一个容器来管理多个 `View` 或其他 `ViewGroup` 时，使用 `ViewGroup`。例如，自定义一个布局容器，来管理多个子视图并确定它们的排列方式。

### 4. 示例
- **自定义 `View`**：
  ```kotlin
  class MyCustomView(context: Context) : View(context) {
      override fun onDraw(canvas: Canvas) {
          // 绘制自定义内容
      }

      override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
          // 测量视图尺寸
          setMeasuredDimension(200, 200)
      }
  }
  ```

- **自定义 `ViewGroup`**：
  ```kotlin
  class MyCustomViewGroup(context: Context) : ViewGroup(context) {
      override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
          // 布局子视图
          val child = getChildAt(0)
          child.layout(0, 0, width, height)
      }

      override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
          // 测量子视图
          measureChild(getChildAt(0), widthMeasureSpec, heightMeasureSpec)
          setMeasuredDimension(MeasureSpec.getSize(widthMeasureSpec), MeasureSpec.getSize(heightMeasureSpec))
      }
  }
  ```

### 总结
- **`View`** 是单一的 UI 元素，负责绘制和交互。
- **`ViewGroup`** 是一个容器，负责管理和布局多个子视图。自定义时，`ViewGroup` 要比 `View` 更复杂，需要处理子视图的测量、布局等功能。





# View的绘制流程是从Activity的哪个生命周期方法开始执行的

在 Android 中，`View` 的绘制流程通常是从 `Activity` 的 `onResume()` 生命周期方法开始执行的。具体流程如下：

1. **`onResume()`**：`Activity` 被恢复到前台并处于交互状态时，`onResume()` 会被调用。这时，`Activity` 会进入到可视状态，系统会开始对视图进行布局和绘制。

2. **`View` 绘制流程**：
   - 在 `onResume()` 后，Android 会调用 `View` 的 `measure()`、`layout()` 和 `draw()` 方法来完成视图的测量、布局和绘制过程。
   - `measure()`：系统计算每个视图的大小，调用 `onMeasure()` 方法。
   - `layout()`：系统确定每个视图的位置，调用 `onLayout()` 方法。
   - `draw()`：系统绘制视图，调用 `onDraw()` 方法。

在 `Activity` 的生命周期中，视图的绘制通常是伴随着 `onResume()` 的调用进行的，确保视图在用户与 `Activity` 交互时能够正确显示。如果在 `Activity` 中有视图变化或者需要重新绘制的场景（比如调整视图的状态），则会触发重新绘制流程。






# Activity,Window,View三者的联系和区别

`Activity`、`Window` 和 `View` 是 Android 中的三个核心组件，它们在 UI 的展示和管理中起着不同的作用，虽然它们密切相关，但各自的功能和职责不同。

### 1. **Activity**
- **作用**：`Activity` 是一个应用程序的组件，负责与用户进行交互并展示界面。每个 `Activity` 通常负责一个单独的界面或屏幕。
- **功能**：`Activity` 承载了 UI 界面的逻辑，它管理 `Window` 和 `View` 的生命周期、事件处理以及与用户交互的行为。
- **生命周期**：`Activity` 的生命周期方法（如 `onCreate()`、`onStart()`、`onResume()` 等）控制着视图的初始化、展示、暂停和销毁等过程。

### 2. **Window**
- **作用**：`Window` 是一个抽象的容器，代表着一个显示区域。在 Android 中，每个 `Activity` 都有一个 `Window` 对象来承载和显示它的内容。
- **功能**：`Window` 作为 UI 层的容器，负责显示内容和管理与视图的交互，它通过 `DecorView` 包裹了 `View` 层级结构（即界面的所有视图）。
  - `Window` 负责显示 `Activity` 的内容。
  - 它处理输入事件的传递，并将这些事件转发给对应的 `View`。
- **类型**：`Window` 有多种类型，例如普通的全屏 `Window` 或对话框等类型，通常是通过 `setContentView()` 设置布局的地方。

### 3. **View**
- **作用**：`View` 是 Android UI 中的基本组件，代表屏幕上的一个元素，通常用于显示文本、按钮、图片等内容。
- **功能**：`View` 负责绘制和响应用户的交互。每个 UI 元素（如按钮、文本框、图像）都是 `View` 的一个实例。
  - `View` 负责自己如何绘制（通过 `onDraw()`）、如何响应触摸事件（通过 `onTouchEvent()`）等。
  - 它也是 `ViewGroup`（一个特殊的 `View`）的基础，用于布局多个子 `View`。

### 三者之间的关系与区别：
- **关系**：
  - `Activity` 是用户交互的入口，负责 UI 和用户行为的协调，`Activity` 管理 `Window` 并将 `View` 渲染到屏幕上。
  - `Window` 是显示界面的容器，`Activity` 会通过 `Window` 来显示其内容。每个 `Activity` 都有一个 `Window`，而 `Window` 里包含了界面的根视图（通常是 `DecorView`）。
  - `View` 是构成 UI 的基本单元，`Activity` 会在 `Window` 中加载和显示一组 `View`，这些 `View` 实际上是界面上显示的所有元素。

- **区别**：
  - `Activity` 是用于管理整个界面的生命周期和行为，而 `Window` 是一个容器，负责显示 `Activity` 中的内容。
  - `View` 是最小的 UI 组件，代表屏幕上的具体元素；`Window` 和 `Activity` 都是用于承载和管理这些 `View` 的。

### 例子
- 当你调用 `setContentView()` 时，实际上是在 `Activity` 中指定了一个 `View`（通常是一个布局文件）。这个布局会被添加到 `Activity` 的 `Window` 中，最终显示在屏幕上。





# 在onResume中是否可以测量宽高
在 `onResume()` 中直接测量宽高是不合适的。`onResume()` 方法是 `Activity` 生命周期的一部分，它在 `Activity` 进入前台并且用户可以交互时被调用，但此时 `View` 的布局（测量和绘制）通常已经完成。

实际测量宽高的过程通常发生在 `onLayout()` 和 `onMeasure()` 方法中，它们是在视图的布局过程中调用的。在 `onResume()` 中，视图已经基本确定了尺寸，因此如果你在此时尝试进行测量，可能会得到不准确的结果。

如果你需要在 `onResume()` 中获取视图的尺寸，通常可以在视图完成布局后通过 `View.getWidth()` 和 `View.getHeight()` 来访问它们的宽高。这是因为这些值在布局完成后才会准确计算出来。





# 在onResume中是否可以测量宽高

在 `onResume()` 中直接测量宽高是不合适的。`onResume()` 方法是 `Activity` 生命周期的一部分，它在 `Activity` 进入前台并且用户可以交互时被调用，但此时 `View` 的布局（测量和绘制）通常已经完成。

实际测量宽高的过程通常发生在 `onLayout()` 和 `onMeasure()` 方法中，它们是在视图的布局过程中调用的。在 `onResume()` 中，视图已经基本确定了尺寸，因此如果你在此时尝试进行测量，可能会得到不准确的结果。

如果你需要在 `onResume()` 中获取视图的尺寸，通常可以在视图完成布局后通过 `View.getWidth()` 和 `View.getHeight()` 来访问它们的宽高。这是因为这些值在布局完成后才会准确计算出来。




# 如何更新UI，为什么子线程不能更新UI？

在 Android 中，更新 UI 必须在主线程（UI 线程）上进行。原因是 Android 的 UI 系统是单线程的，所有 UI 元素的修改和渲染操作都必须在主线程中完成。子线程直接更新 UI 会导致线程安全问题，因为多个线程可能会同时访问 UI 元素，造成数据不一致或崩溃。

为了在子线程中更新 UI，通常使用以下方法：

1. **`runOnUiThread()`**：这是 `Activity` 提供的方法，允许在主线程上执行一段代码。通过这个方法，可以将更新 UI 的任务提交到主线程。

2. **`Handler`**：通过 `Handler` 和 `Looper`，你可以在子线程中发送消息或任务到主线程，进而更新 UI。

3. **`View.post()`**：对于任何 `View` 对象，可以使用 `post()` 方法将一个 Runnable 任务提交到主线程执行。

通过这些方法，子线程可以间接地更新 UI，从而避免直接修改 UI 导致的错误。






# DecorView, ViewRootImpl,View之间的关系

* DecorView 是 Activity 中的根视图，承载整个界面。

* ViewRootImpl 是 UI 渲染和事件处理的关键，它将 DecorView 和 View 层次结构联系起来。

* View 是界面上的具体元素，负责呈现界面的内容和响应用户操作。




# invalidate() 和 postInvalicate() 区别
invalidate() 只能在主线程中直接调用，立即请求视图重绘。

postInvalidate() 可以在任何线程中调用，将重绘请求延迟到主线程执行。





