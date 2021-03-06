# TabBarViewForAndroid
非常简单的为项目设置上TabBar
### 效果展示
  ![效果1](https://github.com/wp529/TabBarViewForAndroid/blob/master/pic/a.PNG)
  ![效果2](https://github.com/wp529/TabBarViewForAndroid/blob/master/pic/b.PNG)
### 导入步骤
* 下载项目，将tabbarview做为moudle导入
* 在gradle的dependencies添加compile project(':tabbarview')
### 使用步骤
1. 在需要使用TabBar的Activity中的布局文件加入TabBarView标签: 
```
  <com.pzhu.tabbarview.TabBarView
      android:layout_width="match_parent"
      android:layout_height="match_parent">
  </com.pzhu.tabbarview.TabBarView>
```
2. 添加命名空间
  xmlns:app="http://schemas.android.com/apk/res-auto"
3. 在TabBarView中加入子标签
 ```
  <com.pzhu.tabbarview.Page
            android:layout_width="100dp"
            android:layout_height="100dp"
            app:page="com.pzhu.test.fragment.AFragment"
            app:text="首页"
            app:icon_normal="@drawable/tab1"
            app:icon_select="@drawable/tab_click1"
            app:checked="true"
            app:text_color_normal="#777"
            app:text_color_select="#f00"
            />
```
4. 完整代码：
```
  <com.pzhu.tabbarview.TabBarView
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <com.pzhu.tabbarview.Page
            android:layout_width="100dp"
            android:layout_height="100dp"
            app:page="com.pzhu.tabbartest.fragment.AFragment"
            app:text="首页"
            app:icon_normal="@drawable/tab1"
            app:icon_select="@drawable/tab_click1"
            app:checked="true"
            app:text_color_normal="#777"
            app:text_color_select="#f00"
            />

        <com.pzhu.tabbarview.Page
            android:layout_width="100dp"
            android:layout_height="100dp"
            app:page="com.pzhu.tabbartest.fragment.BFragment"
            app:text="个人中心"
            app:icon_normal="@drawable/tab2"
            app:icon_select="@drawable/tab_click2"
            app:text_color_normal="#777"
            app:text_color_select="#f00" />

    </com.pzhu.tabbarview.TabBarView>
```
### Page里属性含义
```
    text -> tab文字  
    icon_normal -> tab未选中时的图标
    icon_select -> tab选中时的图标
    checked -> 初始化选中项
    text_color_normal -> tab未选中时文字颜色
    text_color_select -> tab选中时文字颜色
    page -> tab对应的fragment tips:fragment包名加类名
```
### 排列方式
```
  TabBar按照子节点排列顺序从左到右显示
```
### 需要注意的地方
fragment在每次显示到界面的时候都要进行重绘(了解fragment的应该知道这个机制)，就是会多次调用onCreateView()方法，为了避免这个重复的重绘，所以需要在fragment里进行缓存，参考代码
```
public class AFragment extends Fragment {
    private View rootView;
    private ProgressDialog myDialog;

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        if (rootView == null) {
            TextView tv = new TextView(getActivity());
            tv.setText("AFragment");
            myDialog = new ProgressDialog(getActivity());
            myDialog.setProgressStyle(ProgressDialog.STYLE_SPINNER);
            myDialog.setMessage("处理中……");
            myDialog.setIndeterminate(false);
            myDialog.setCancelable(false);
            myDialog.show();
            Handler handler = new Handler();
            handler.postDelayed(new Runnable() {
                @Override
                public void run() {
                    myDialog.dismiss();
                }
            },3000);
            rootView = tv;
        }
        ViewGroup parent = (ViewGroup) rootView.getParent();
        if (parent != null) {
            parent.removeView(rootView);
        }
        return rootView;
    }
}
```
这是我测试用的，你们可以根据自己的需求来进行这部分。
### 多余的话
```
  这个目前还有许多缺点，希望各位在使用的过程中有想要实现的需求或者不足的地方与我联系
  15680351591@163.com
  谢谢
```
 
