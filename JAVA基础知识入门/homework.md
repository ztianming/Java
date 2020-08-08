##

# Java编程基础
## 客观练习题
在java中下列关于自动类型转换说法正确的是（）

## 函数练习题
### 打印输出手机的相关信息
描述
学习变量类型，使用合适的变量类型接收变量的值，打印输出手机的相关信息。
```java
public class HuaweiPhone {
	public static void main(String[] args) {
		String brand = "华为pro20"; // 品牌
		double weight = 12.4; // 重量
		String type = "内置锂电池"; // 型号
		int price = 2499; // 价格
		System.out.println("手机:  ");
		System.out.println("名称：\t" + brand);
		System.out.println("重量：\t" + weight);
		System.out.println("电池：\t" + type);
		System.out.println("价格：\t" + price);
	}
}
```
### Java 类型转换
描述  
根据TODO要求，修改错误代码
```java
import java.io.*;
import java.util.*;

public class Main {
  public static void main(String args[]) {
    //TODO:请仔细阅读代码，在标注了有错误的语句处进行改正。
		double score=89.3;
		int scoreInt=(int)score;    //该行代码有错误，请改正。
		System.out.println(score);
		System.out.println(scoreInt);
  }
}
```
### Java 数据类型
描述  
根据TODO要求，完成不同数据类型的变量的定义和赋值

```java
import java.io.*;
import java.util.*;

public class Main {
  public static void main(String args[]) {
        /*TODO：按照提示添加变量定义和初始化代码。
        1.定义字符串变量name，初始化为“张三”
        2.定义年龄变量age，初始化为18
        3.定义性别变量sex，初始化为'男'
        4.定义分数变量score，初始化为 66.6f
        5.定义是否及格变量isPass，初始化为true
        */
        String name = "张三";
        int age = 18;
        String sex = "男";
        double score = 66.6;
        boolean isPass = true;
        
		System.out.println(name + age + "岁" + "，性别：" + sex + "，这次考了" + score + "分，是否及格" + isPass + "。");
  }
}
```

### 分别对两个浮点数据进行计算加、减、乘、除运算
描述  
定义两个double型的变量a和b，获取键盘输入的两个数赋值给a和b，对a和b进行加、减、乘、除运算，然后输出结果
```java
import java.util.Scanner;

public class Example2_2_4 {
	public static void main(String args[ ]) {
		   System.out.println("输入两个浮点型数据，分别计算加、减、乘、除");
		   double a ,b;
           double c,d,e,f;
		   Scanner scanner = new Scanner(System.in);
		   a = scanner.nextDouble();
		   b = scanner.nextDouble();
		   /*
		   TODO:对a,b依次进行加减乘除运算
           并把结果分别赋给变量c d e f
		   */
           c = a+b;
           d = a-b;
           e = a*b;
           f = a/b;
		   System.out.println("结果分别为：\n"+c+"\n"+d+"\n"+e+"\n"+f);
	}
}

```
### 将float类型强制转换int类型
描述  
用户输入1个float类型的数值，赋值给float型的变量a，然后强制转换为int型并输出。
```java

import java.util.Scanner;

/*
 随机输入1个float类型的数值，强制转换int然后输出
 如：输入12.34，强制转换int后输出：12.
 */
class Example7_65 {

	public static void main(String[] args) {

		Scanner input = new Scanner(System.in);
		System.out.println("输入1个float数值:");
		float a = input.nextFloat();
		
		//TODO：强制将float型的a转换成int型
        int b = (int) a;
		System.out.print(b);
		
		
	}
}

```
### 字符与整数类型转换
描述  
定义一个int型变量、一个char型变量，输入一个汉字或日文赋值给char型变量，然后打印出该变量在Unicode表的位置。再输入一个数字代表Unicode表的位置，赋值给int型变量，然后打印出该位置的字符。

Note:此处题目默认代码只有方法，没有完整的类，需要自己补充
```java
import java.util.Scanner;

public class test {
    public static void main (String[] args) {
        System.out.println("请输入一个汉字或日文：");
        Scanner scanner = new Scanner(System.in);            //TODO:输入一个字符，然后使用charAt函数转化为char类型的变量c
        String s = scanner.next();
        char c = s.charAt(0);
        System.out.println("汉字或日文:" + c + "的位置:" + (int) c);
        System.out.println("请输入Unicode表的位置（数字）");
        //TODO:使用Scanner输入一个整数，并赋值给变量n
        int n = scanner.nextInt();
        System.out.println(n + "位置上的字符是:" + (char) n);
    }
}
```
### 输入两个变量a,b均是整型变量，写出将a,b两个变量中的值互换的程序
描述  
定义两个整型变量a,b,获取键盘输入的两个整数赋值给a，b，使用临时变量temp交换a,b的值。并打印输出交换后a和b的值。
```java
import java.util.Scanner;

public class Example2_2 {
   
public static void main(String args[ ]) {
	   int a=2;

	   int b=3;
	   System.out.println("请输入两个整数赋值给a,b：");
	   Scanner scanner = new Scanner(System.in);
	   a = scanner.nextInt();
	   b = scanner.nextInt();
	   System.out.println("交换前的值是 a="+a+",b="+b);
	   
	    /*
		TODO:使用中间变量交换ab值
        例：定义temp变量暂时存储a值
	    */
        int temp;
        temp = a;
        a =b;
        b=temp;
	   
	   
	   System.out.println("交换后的值是 a="+a+","+"b="+b);
   }
}
```
### byte与int、float与double类型转换运算
描述  
定义byte、int、float、double类型的变量，并输入对应的数据给四种类型的变量赋值，将int转换为byte,将float转换为double，打印输出4个类型数据的值和转换后的数值。
```java
import java.util.Scanner;

public class Example2_2 { 
    public static void main (String args[]) {
      byte b = 22; 
      int  n = 129;  
      float f =123456.6789f ;
      double d=123456789.123456789;
      System.out.println("请输入byte(-128到127) int(大于127) float double四种类型数据：");
      Scanner scanner = new Scanner(System.in);
      try{
		//TODO:使用Scanner输入byte、int、float、double类型的数据，分别赋值给b、n、f、d
        b = scanner.nextByte();
        n = scanner.nextInt();
        f = scanner.nextFloat();
        d = scanner.nextDouble();
      }catch (Exception e) {
    	  System.out.println("输入有误！");
      }
      System.out.println("---------输出------------");
      System.out.println("byte=  "+b);   
      System.out.println("int=  "+n);
      System.out.println("float=  "+f);   
      System.out.println("double=  "+d); 
	  /*
	  TODO:将int强转化成byte
      int型的n强转后赋值给byte型的b
      将float强转化成double
      float型的f强转后赋值给double型的d
	  */
      b = (byte)n;
      d = (float) f;
      System.out.println("将int转化为byte导致精度缺失        int=  "+b);
      System.out.println("将float转化为double导致精度缺失        double=  "+d);
   }
}
```
### 类型转换
描述  
输入一个浮点数，将其分别转换成int和float，输出该数转换为int、float类型后损失的精度，请将转换的代码补全。
```java
import java.util.Scanner;

/**
 * TODO:  输入一个浮点数，分别将其转换成int和float，输出该数转换为int、float类型后损失的精度，请将转换的代码补全。
 * 例如：输入：2.1 输出: double转换成int损失的精度值：0.10000000000000009
 *                   double转换成float损失的精度值：9.536743172944284E-8

 */
public class Example7_153 {
	 
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		System.out.println("输入一个浮点数");
		 
		double num = sc.nextDouble(); 
		
		int num1 = 0;
		//TODO:补全double类型num强制转换成int类型num1的代码  
		num1 = (int) num;
		//计算double转换成int损失的精度值
		double value1 = num - num1;
		
		System.out.println( "double转换成int损失的精度值："+ value1);
		
		float num2 = 0f;
		
	    //TODO:补全double类型num强制转换成float类型num2的代码 
		num2 = (float) num;
		//计算double转换成float损失的精度值 
		double value2 = num - num2;
		
		System.out.println("double转换成float损失的精度值："+ value2);
	}
}
```
### 将float类型强制转换int类型
描述  
用户输入1个float类型的数值，赋值给float型的变量a，然后强制转换为int型并输出。
```java
import java.util.Scanner;
/*
 随机输入1个float类型的数值，强制转换int然后输出
 如：输入12.34，强制转换int后输出：12.
 */
class Example7_65 {

	public static void main(String[] args) {

		Scanner input = new Scanner(System.in);
		System.out.println("输入1个float数值:");
		float a = input.nextFloat();
		
		//TODO：强制将float型的a转换成int型
        int b = (int)a;
		System.out.print(b);
		
		
	}
}
```

```java

```
```java

```
```java

```
