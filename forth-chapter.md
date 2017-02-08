# 概述：

> 算法+数据结构=程序
>
> 面向过程是先确定算法在决定数据结构
>
> 面向对象\(oop\)是先确定数据结构在确定算法

# 类之间的关系：

* 依赖\(**user a**\)
* 聚合\(**has a**\)
* 继承\(**is a**\)![](/assets/图像 1.png)

# 小技巧：

可以在一个类中加入main方法。然后再javac和java该类。可以测试这个类

# java方法

方法参数的传递时值的传递，不是引用的传递

```
public class Test1 {
    public static void main(String[] args) {
        String test="涨价00";
        String test1="00";
        int sss=100;
        test(sss);
        test(test);
        Test test2=new Test();
        test(test2);
        System.out.println(sss);//输出：100 修改失败
        System.out.println(test);//输出：涨价00 修改失败
        System.out.println(test2.name);修改成功：234333
    }

    public static void test(int x){
        x++;
    }   public static void test(String x){
        x=x+"xxx";
    }
    public static void test(Test x){
       x.name="234333";
    }
    static class Test{
        String name="xx==x";
    }
}
```

# 类的创建

在构造方法中对变量进行初始化操作，防止遇到空指针问题，或者使用工厂模式进行创建



