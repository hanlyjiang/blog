---
layout: post
title: "Android View状态的保存与恢复"
category :
- Android
tagline: "工作"
tags : [Android,自定义控件]
---

如何正确的恢复View的状态？

> 即使您什么都不做，也不实现 onSaveInstanceState()，Activity 类的 onSaveInstanceState() 默认实现也会恢复部分 Activity 状态。具体地讲，默认实现会为布局中的每个 View 调用相应的 onSaveInstanceState() 方法，让每个视图都能提供有关自身的应保存信息。Android 框架中几乎每个小部件都会根据需要实现此方法，以便在重建 Activity 时自动保存和恢复对 UI 所做的任何可见更改。例如，EditText 小部件保存用户输入的任何文本，CheckBox 小部件保存复选框的选中或未选中状态。您只需为想要保存其状态的每个小部件提供一个唯一的 ID（通过 android:id 属性）。如果小部件没有 ID，则系统无法保存其状态。


<!-- more -->



## 何时触发？
1. 配置变更-旋转屏幕
2. Fragment 重新恢复
3. 系统关闭App 之后重启从后台恢复


## View 保存与恢复流程    
![store and restore](save.png)

## 条件
1. View拥有id(且为唯一id)
2. 调用 setSaveEnabled(true)


## 通用做法    
以下代码记录mapView的中心点和层级，并在重启时恢复中心点和层级。    
```java
@Nullable
    @Override
    protected Parcelable onSaveInstanceState() {
        // 保存ParentState
        Bundle state = new Bundle();
        
        Parcelable superState = super.onSaveInstanceState();
        state.putParcelable(PARENT_STATE, superState);

        // 扩展自己的state存储
        state.putFloat(STATE_LEVEL, mapView.getLevel());
        state.putDouble(STATE_CENTER_X, mapView.getCenter().getX());
        state.putDouble(STATE_CENTER_Y, mapView.getCenter().getY());
        return state;
    }


    @Override
    protected void onRestoreInstanceState(Parcelable state) {
        Bundle bundle = (Bundle) state;
        // 恢复Parent state
        Parcelable parentState = bundle.getParcelable(PARENT_STATE);
        super.onRestoreInstanceState(parentState);

        // 恢复自己的 state
        float level = bundle.getFloat(STATE_LEVEL);
        double centerX = bundle.getDouble(STATE_CENTER_X);
        double centerY = bundle.getDouble(STATE_CENTER_Y);

        mapView.setCenter(new Point(centerX, centerY));
        mapView.setLevel(level);
    }
```

## 如果id相同呢？    
> 看下之前的条件，需要view拥有id，而且id唯一，这是为什么呢？    
> 具体请参考： [trickyandroid.com]    

因为所有的View的状态都被存储在一个 `SparseArray<Parcelable> container`里面，不论其层级。那么不同层级的id有一样的情况下如何处理？    

比方说，我有一个如下界面：
![布局效果](view.png)

其中地类名称和面积是动态生成并加入到视图体系的。每一行的结构都一样，是一个自定义的 `PropSetView` :
![Layout结构](layout.png)    

由于每一行内部的空间id都一样，会导致同样id的view只有一个存储成功，恢复后所有的Item的状态都和最后一个item一致    

### 解决方法   
1. 自动生成 `PropSetView` 后，为其分配一个唯一id(API17之后可以通过`View.generateViewId()`方法生成，但是要注意id的恢复)    
2. `PropSetView` 中重写若干方法，在保存流程中，自己负责存储并恢复自己的子View的状态，不存储到总的SpareArray中，避免id相同覆盖要恢复的状态 :   
```java
@Override
    public Parcelable onSaveInstanceState() {
        Parcelable superState = super.onSaveInstanceState();
        MySavedState ss = new MySavedState(superState);
        ss.childrenStates = new SparseArray();
        for (int i = 0; i < getChildCount(); i++) {
            getChildAt(i).saveHierarchyState(ss.childrenStates);
        }
        return ss;
    }

    @Override
    public void onRestoreInstanceState(Parcelable state) {
        MySavedState ss = (MySavedState) state;
        super.onRestoreInstanceState(ss.getSuperState());
        for (int i = 0; i < getChildCount(); i++) {
            getChildAt(i).restoreHierarchyState(ss.childrenStates);
        }
    }

    @Override
    protected void dispatchSaveInstanceState(SparseArray<Parcelable> container) {
        dispatchFreezeSelfOnly(container);
    }

    @Override
    protected void dispatchRestoreInstanceState(SparseArray<Parcelable> container) {
        dispatchThawSelfOnly(container);
    }

    static class MySavedState extends BaseSavedState {
        public static final ClassLoaderCreator<MySavedState> CREATOR
                = new ClassLoaderCreator<MySavedState>() {
            @Override
            public MySavedState createFromParcel(Parcel source, ClassLoader loader) {
                return new MySavedState(source, loader);
            }

            @Override
            public MySavedState createFromParcel(Parcel source) {
                return createFromParcel(null);
            }

            public MySavedState[] newArray(int size) {
                return new MySavedState[size];
            }
        };
        SparseArray childrenStates;

        MySavedState(Parcelable superState) {
            super(superState);
        }

        private MySavedState(Parcel in, ClassLoader classLoader) {
            super(in);
            childrenStates = in.readSparseArray(classLoader);
        }

        @Override
        public void writeToParcel(Parcel out, int flags) {
            super.writeToParcel(out, flags);
            out.writeSparseArray(childrenStates);
        }
    }
```

## 参考：
1. [Google-在配置变更期间保留对象
](https://developer.android.com/guide/topics/resources/runtime-changes.html#RetainingAnObject)
2. [Saving Android View state correctly - trickyandroid.com](http://trickyandroid.com/saving-android-view-state-correctly/)
3. [Google-保存 Activity 状态](https://developer.android.com/guide/components/activities.html)    


[trickyandroid.com]:http://trickyandroid.com/saving-android-view-state-correctly/