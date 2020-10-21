### Navigation Component 入门
#### 添加的Navigetion Component依赖
 - 在相对应模块的build.gradle下添加导航组件的相关依赖
 ```build.gradle
        dependencies {
        def nav_version = "2.3.0"

        // Java language implementation
        implementation "androidx.navigation:navigation-fragment:$nav_version"
        implementation "androidx.navigation:navigation-ui:$nav_version"

        // Kotlin
        implementation "androidx.navigation:navigation-fragment-ktx:$nav_version"
        implementation "androidx.navigation:navigation-ui-ktx:$nav_version"

        // Feature module Support
        implementation "androidx.navigation:navigation-dynamic-features-fragment:$nav_version"

        // Testing Navigation
        androidTestImplementation "androidx.navigation:navigation-testing:$nav_version"
        }
 ```
#### 创建导航图
1. 导航图：导航发生在应用中的各个**目的地**（即您的应用中用户可以导航到的任意位置）之间。这些目的地是通过操作连接的。导航图是一种资源文件，其中包含您的所有目的地和操作。该图表会显示应用的所有导航路径。
   - 从文档中可以看出导航图由两个方面组成的
     - 第一个方面即所有的**目的地**，这里目的地不可以理解称为 ` Activity `。每一个的` Activity `都会存在一个导航图。
     - 第二个方面是描述目的地之间的关系。
2. 如何创建导航图：
   - 在` Project `窗口中，右键点击 `res` 目录，然后依次选择 `New` > `Android Resource File`。此时系统会显示 New Resource File 对话框。
   - 在 `File name` 字段中输入名称，例如“`nav_graph`”。
   - 从 `Resource type` 下拉列表中选择 `Navigation`，然后点击 OK。
   - 添加首个导航图时，Android Studio 会在 res 目录内创建一个 navigation 资源目录。该目录包含您的导航图资源文件（例如 `nav_graph.xml`）
    ***
    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto" android:id="@+id/nav_res" />
    ```
    上面即创建导航图资源文件` xml Code `形式。目前没有添加任何的目的地和操作。

#### 导航图的控制者：导航宿主/主机（NvaHost）
1. 第二步骤中创建了资源文件导航图。导航图中描述的操作由导航宿主来控制，导航宿主是导航组件中最重要的一部分。导航宿主是一个空容器，用户在您的应用中导航时，目的地会在该容器中交换进出。导航宿主必须派生于 NavHost。Navigation 组件的默认 NavHost 实现 (NavHostFragment) 负责处理 Fragment 目的地的交换。
***
2. 新建导航宿主：
    ```kotlin
    import androidx.navigation.fragment.NavHostFragment
    class CustomNavHost: NavHostFragment() {
        //TODO()
    }
    ```
    目前导航的宿主中没有实现导航宿主控制任何方法。导航宿主和导航图之间不存在任何关联。

#### 主 Activity 引用导航宿主和导航图
1. 在了解 Acticity 如何引用导航宿主、导航图之前，就需要知晓Navigation Component的作用。
   - Navigation 组件旨在用于具有一个主 **Activity 和多个 Fragment** 目的地的应用。主 Activity 与导航图相关联，且包含一个负责根据需要交换目的地的 NavHostFragment。**在具有多个 Activity 目的地的应用中，每个 Activity 均拥有其自己的导航图**。
   - 所有单个导航图导航不是 ` Activity `之间的操作，而是主` Activity `和其` Fragment ` 之间的操作。
***
2. 使用组件来引用导航宿主和导航图: 在主` Activity ` 当中 `FragmentContainerView` 属性来引用导航宿主的和导航图
  ```xml
    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/nav_host_fragment"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"

        app:defaultNavHost="true"
        app:navGraph="@navigation/nav_graph" />
  ```
3. 属性引用
    |       属性名    |作用    |
    |    --------      | -----:| 
    |   android:name |指定导航宿主。|
    |   android:navGraph |指定导航图。|
    |   android:defaultNavHost | 会拦截系统返回按钮|
 #### 向导航图添加目的地、操作
- 在 Navigation Editor 中，点击 New Destination 图标 ，然后点击 Create new destination。
    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:id="@+id/nav_res"
            app:startDestination="@id/blankFragment">
            <fragment
                android:id="@+id/blankFragment"
                android:name="top.wangdf.navigationcomponent.fragment.ui.BlankFragment"
                android:label="fragment_blank"
                tools:layout="@layout/fragment_blank" />
            <fragment
                android:id="@+id/itemFragment"
                android:name="top.wangdf.navigationcomponent.fragment.ui.ItemFragment"
                android:label="fragment_item_list"
                tools:layout="@layout/fragment_item_list" />
            <activity
                android:id="@+id/mainActivity"
                android:name="top.wangdf.navigationcomponent.MainActivity"
                android:label="activity_main"
                tools:layout="@layout/activity_main" />
        </navigation>
    ```
***
- 以上便是添加主`Activity`和`Fragment`之后的xml文件。
- 加上操作之后
        ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <navigation xmlns:android="http://schemas.android.com/apk/res/android"
            xmlns:app="http://schemas.android.com/apk/res-auto"
            xmlns:tools="http://schemas.android.com/tools"
            android:id="@+id/nav_res"
            app:startDestination="@id/mainActivity">
            <fragment
                android:id="@+id/blankFragment"
                android:name="top.wangdf.navigationcomponent.fragment.ui.BlankFragment"
                android:label="fragment_blank"
                tools:layout="@layout/fragment_blank" >
                <action
                    android:id="@+id/action_blankFragment_to_itemFragment"
                    app:destination="@id/itemFragment" />
            </fragment>
            <fragment
                android:id="@+id/itemFragment"
                android:name="top.wangdf.navigationcomponent.fragment.ui.ItemFragment"
                android:label="fragment_item_list"
                tools:layout="@layout/fragment_item_list" />
            <activity
                android:id="@+id/mainActivity"
                android:name="top.wangdf.navigationcomponent.MainActivity"
                android:label="activity_main"
                tools:layout="@layout/activity_main" />
        </navigation>
        ```
- 对比可以知道，操作是由`<action>` 标签来指定。
      ```xml
        <action
            android:id="@+id/action_blankFragment_to_itemFragment"
            app:destination="@id/itemFragment" />
        ```
    在导航图中，操作由 <action> 元素表示。操作至少应包含自己的 ID 和用户应转到的目的地的 ID。上面的`fragment`即从 blankFragment 转到 itemFragment 
    属性` app:destination `来指定目标目的地。

