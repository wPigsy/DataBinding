



1. 开启databinding

   ```groovy
       dataBinding {
           enabled = true
       }
   ```

   

2. 更改layout文件，添加layout标签

3. 添加变量



绑定方式

1. 方法引用

   在绑定的对象上创建和控件的方法相同的方法，然后将事件传递给绑定的对象

   ```xml
   <!--方法引用方式绑定-->
   <EditText
             android:id="@+id/et_1"
             android:layout_width="100dp"
             android:onTextChanged="@{javaBean.onTextChanged}"
             android:layout_height="wrap_content" />
   ```

   ```java
   public class Student {
   
       private static final String TAG = "Student";
   
       public String name;
   
       public Student(String name) {
           this.name = name;
       }
   
       public void onTextChanged(CharSequence text, int start, int lengthBefore, int lengthAfter) {
           Log.d(TAG, "onTextChanged: " + text);
           name = String.valueOf(text);
       }
   }
   ```

   > Student中的onTextChanged方法必须和EditText的方法名称参数相同

2. 监听器绑定

   将点击事件通过 lamda 表达式传递给绑定对象，可以传递其他参数。

   ```xml
   <!--方法引用方式绑定-->
   <EditText
   android:id="@+id/et_1"
   android:layout_width="100dp"
   android:onClick="@{() -> presenter.onClickEditView(javaBean)}"
   android:layout_height="wrap_content" />
   ```

   ```java
       public class Prenter {
           public void onClickEditView(Student student) {
               Toast.makeText(MainActivity.this, "student onclick" + student.name, Toast.LENGTH_SHORT).show();
           }
       }
   ```

   > 当点击EditText 的时候，实际响应事件的是presenter的onClickEditView方法，xml传递了一个javaBean给presenter



表达式

简单的表达式计算可以直接在xml中写

特殊运算符：

- `android:text="@{user.displayName??user.lastName}"`
  - 取第一个非空的结果，完全等同于三元运算符

空指针databinding一般会保护好，但数组索引越界使用时记得check





include

根布局必须为viewgroup

传入的binding，若要更改界面需要重绘

注意点：

- #### 需要添加命名空间

  - `xmlns:bind="http://schemas.android.com/apk/res-auto"`

- #### 若要更改界面则需要重绘

  - `mBinding.include.invalidateAll();`通过绑定的binding获取include的ID，然后重绘





viewStub

databinding实际通过ViewStubProxy监听inflate，

使用：`mBinding.vs.getViewStub().inflate();`





### Observable

- 对要变化的变量的get方法上标记bindable

  ```
   @Bindable
   public String getName() {
   	return name;
   }
   
   当set时调用
   notifyPropertyChanged(BR.name);
   
   即可动态刷新数据
  ```

  

Observable Fields

不继承BaseObservable的方式

ObservableArrayMap

ObservableArrayList

ObservableBoolean

。。。。



