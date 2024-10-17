---

title: Android Studio——ActivityResultLauncher

---

# 【Android Studio】ActivityResultLauncher

[TOC]



## 一：什么是ActivityResultLauncher

ActivityResultLauncher是Android Studio用于在应用程序中启动活动并接收其结果的组件，它是startActivityForResult的改进版本。

优点：

- 类型安全：结果处理逻辑和启动器绑定在一起，避免传统方法中可能出现的类型转换错误。

- 解耦：不需要重写onActivityResult（），逻辑更加清晰。

- 生命周期感知：启动器与生命周期相关联，在适当的时间注册和取消注册。

  

## 二：ActivityResultLauncher

ActivityResultLauncher 是一个用于简化 Android 开发中 startActivityForResult 流程的新 API。它通过注册 contract 和 ActivityResultCallback 来启动和接收活动结果，提供更直观的流程管理。ActivityResultLauncher 是 Android 官方推荐的用来替代 startActivityForResult 的新方式，通过它可以非常方便地调用系统 Intent 进行拍照、选取本地文件等操作。

## 三：活动之间互相传递数据

【目的】创建两个Activity文件实现点击按钮互相在页面上显示数据。

### 3.1向下一个Activity传递数据

前置要求：

设置layout的布局：

ActivityA_layout:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/pushtoA"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center|center_horizontal"
        android:text="@string/page4"
        android:textSize="34sp" />

    <Button
        android:id="@+id/ButtonToA"
        android:layout_width="match_parent"
        android:layout_height="124dp"
      B_layout:  android:gravity="center"
        android:text="@string/Button4"
        android:textSize="34sp" />

    <TextView
        android:id="@+id/viewB"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="@string/view2"
        android:textSize="34sp" />
</LinearLayout>
```

ActivityB_layout:

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:id="@+id/pushtoB"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center|center_horizontal"
        android:text="@string/page3"
        android:textSize="34sp" />

    <Button
        android:id="@+id/ButtonToB"
        android:layout_width="match_parent"
        android:layout_height="124dp"
        android:gravity="center"
        android:text="@string/Button3"
        android:textSize="34sp" />

    <TextView
        android:id="@+id/viewA"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:text="@string/view1"
        android:textSize="34sp" />
</LinearLayout>
```

向下一个活动传递数据，通过Intent类，此处我们使用点击Button触发，在下一个页面显示信息。

新建Activity3页面，在Activity文件中**创建一个Intent的对象，传入上下文，和下一个活动的.class**

![image-20241017082204903](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017082204903.png)

在Activity4页面，写入以下内容：

![image-20241017082339651](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017082339651.png)

运行结果：

ActivityA界面：

![image-20241017082745539](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017082745539.png)

点击button<push to B>显示：

![image-20241017082833606](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017082833606.png)

显示成功！

### 3.2向上一个Activity传递数据

如何实现点击B的按钮再跳转A?并且传值使用launcher，因为这样A就能根据id号来准确判断分别来自于哪个页面的button值

（1）首先在ActivityA中设置ActivityResultLauncher：

首先，你需要在`FirstActivity`中定义一个`ActivityResultLauncher`。这通常是在你的Activity的`onCreate`方法中完成的。

```
// ActivityA.java  
private ActivityResultLauncher<Intent> someActivityResultLauncher = registerForActivityResult(  
    new ActivityResultContracts.StartActivityForResult(),  
    new ActivityResult.Callback<ActivityResult>() {  
        @Override  
        public void onActivityResult(ActivityResult result) {  
            if (result.getResultCode() == Activity.RESULT_OK) {  
                Intent data = result.getData();  
                if (data != null) {  
                    String returnedData = data.getStringExtra("resultKey");  
                    // 处理从SecondActivity返回的数据  
                }  
            }  
        }  
    }  
);
```

如图：

![image-20241017085614736](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017085614736.png)

> [!NOTE]
>
> 1. 注意：`ActivityResultContracts.StartActivityForResult()`是一个预定义的合同，用于启动一个Activity并期待结果。

（2）**在`FirstActivity`中启动`SecondActivity`**：使用你定义的`ActivityResultLauncher`来启动`SecondActivity`。

![image-20241017085737475](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017085737475.png)

（3）**在`SecondActivity`中设置结果数据并返回**：

在`SecondActivity`中，当用户点击按钮时，你需要创建一个Intent来携带返回数据，并调用`setResult`方法来结束`SecondActivity`并返回数据。

```
// ActivityB.java  
Button returnButton = findViewById(R.id.return_button);  
returnButton.setOnClickListener(new View.OnClickListener() {  
    @Override  
    public void onClick(View v) {  
        Intent returnIntent = new Intent();  
        returnIntent.putExtra("resultKey", "这是从SecondActivity传递回的数据");  
        setResult(Activity.RESULT_OK, returnIntent);  
        finish(); // 结束SecondActivity  
    }  
});
```

按照以上格式写我们自己的代码如下：

![image-20241017085823464](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017085823464.png)

运行结果（从B点击按钮跳转A）

![image-20241017083611450](C:\Users\张佳悦\AppData\Roaming\Typora\typora-user-images\image-20241017083611450.png)

跳转成功！

现在，当用户从`SecondActivity`点击按钮时，`SecondActivity`会结束，并且携带的数据会通过`ActivityResultLauncher`的回调方法返回给`FirstActivity`。

## 四：总结

这种方法的好处是，它使用了Android Jetpack中的`ActivityResultAPIs`，这些API旨在简化Activity之间的结果传递，并处理一些与请求代码和结果代码相关的常见错误。此外，使用`ActivityResultLauncher`还可以避免内存泄漏，因为它不需要你在Activity中显式地覆盖`onActivityResult`方法。

最后，推荐一个关于ActivityResultlauncher深入学校的博客：

https://github.com/SheTieJun/BaseKit/wiki/ActivityResultLauncher%E4%BD%BF%E7%94%A8