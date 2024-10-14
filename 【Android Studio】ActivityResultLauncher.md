---

title: Android Studio——ActivityResultLauncher

---

【Android Studio】ActivityResultLauncher

一：什么是ActivityResultLauncher

ActivityResultLauncher是Android Studio用于在应用程序中启动活动并接收其结果的组件，它是startActivityForResult的改进版本。

优点：

- 类型安全：结果处理逻辑和启动器绑定在一起，避免传统方法中可能出现的类型转换错误。
- 解耦：不需要重写onActivityResult（），逻辑更加清晰。
- 生命周期感知：启动器与生命周期相关联，在适当的时间注册和取消注册。