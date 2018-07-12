
---
layout:     post
title:      "Hello 2018"
subtitle:   " \"Hello World, Hello Blog\""
date:       2015-07-11 12:00:00
author:     "SuXi"
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Kotlin
---

> “开启Kotlin之路”

## 前言

最近发现要学的东西有点多，早就打算来学习Kotlin，但没有真正的提上行程，这两天终于将手上的一些事，以及之前的学习计划都做完了，故打算用一段时间来认真的学习Kotlin。

## 正文

首先表明的是，在这里不会刻意的去讲kotlin的基础语法，我会结合一些简单的android项目，在实际的使用中来介绍Kotlin。

首先讲一个可以说是在几乎所有的APP中都会用到的列表，这里使用RecycleView。在新建项目中写布局文件的时候，根据Studio的提示意外的发现一个与RecycleView极其相似的类RecycleListView，此处不做过多解析，感兴趣的可以自行了解。

> activity_main.xml


```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context="project.suxi.com.kotlindemo.MainActivity">

    <Button
        android:id="@+id/button"
        android:layout_width="match_parent"
        android:layout_height="48dp" />

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recycleView"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/support_simple_spinner_dropdown_item" />

</LinearLayout>

```

> MainActivity.kt


```

class MainActivity : AppCompatActivity(){

    private lateinit var myAdatper: MyAdatper //lateinit 用到的时候才初始化， var为变量 val相当于final

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        button.setOnClickListener{e -> System.out.print(e.toString())}

        // 可以直接通过xml中的id直接调用
        recycleView.layoutManager = LinearLayoutManager(application)

        var stringList = ArrayList<String>()
        for (i in 1..30) {
            stringList.add("测试条目" + i)
        }
        
        myAdatper = MyAdatper(this, stringList)
        // 使用如下这种方式实现Item的点击事件，会报错，暂时没有找到答案，待后续
        //        myAdatper.setOnItemClickListener { position -> Toast.makeText(this, stringList[position], Toast.LENGTH_SHORT).show() }
        recycleView.adapter = myAdatper

        recycleView.addItemDecoration(DividerItemDecoration(this, DividerItemDecoration.VERTICAL))
    }

    // 构造函数里的参数必须要使用val来修饰
    class MyAdatper(private val mContext : Context, private val listData: List<String>) : RecyclerView.Adapter<MyAdatper.MyViewHolder>() {

        private lateinit var mListener: OnItemClickListener
        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
            return MyViewHolder(TextView(parent!!.context))
        }

        override fun getItemCount(): Int = listData.size

        override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        
            // List也可以使用索引
            holder.textView.text = listData[position]
            
            // lamabda 表达式
            holder.textView.setOnClickListener{
                Toast.makeText(mContext,listData[position], Toast.LENGTH_SHORT).show()
            }
//            holder.textView.setOnClickListener{ mListener.onItemClick(position) }
        }

        class MyViewHolder(val textView: TextView) : RecyclerView.ViewHolder(textView) {
        }

        fun setOnItemClickListener(listener: OnItemClickListener) {
                mListener = listener
        }

        interface OnItemClickListener {
            fun onItemClick(position: Int)
        }
    }
}
```

## 后话

这样一个简单实例就算完成了，如此看来kotlin确实要比java简单一些，直观的提现就是：

- 不需要强制转换
- 许多情况下不需要显式写出构造函数
- 减少手动判空的操作（在后台不规范的情况下，这能省许多精力）

但留有一问，就是在MyAdapter的外面通过接口回调的方式来实现Item的点击事件，不论是使用lamabda还是匿名内部类的方式，都没有成功，这里的坑待以后来填。


