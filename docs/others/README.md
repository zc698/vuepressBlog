# TestNg框架总结
一、	TestNg的优点
1.	漂亮的html格式测试报告
2.	支持并发测试
3.	参数化测试更简单
4.	支持输出日志
5.	支持多功能的注解

二、	编写TestNg测试用例步骤
1.	使用编辑工具生成TestNg的测试框架
2.	在生成的测试程序框架中编写测试代码逻辑
3.	根据测试代码逻辑，插入TestNg注解标签
4.	配置Testng.xml文件，设定测试类、测试方法、测试分组的执行信息
5.	执行Test NG的测试程序

三、	TestNG常用注解：

执行顺序如下

@BeforeSuite >@BeforeTest>@BeforeMethod>@Test>@AfterMethod>@AfterTest>@AfterSuite

@BeforeSuite	表示会在当前测试集合中的任意一个测试用例开始运行前执行

@AfterSuite	表示会在当前测试集合中的所有测试程序运行结束之后执行

@BeforeTest	表示会在Test中的任意一个测试用例开始运行前执行

@AfterTest	表示会在Test中的所有测试用例运行结束后执行

@BeforeGroups	分组测试用例的任一测试用例开始运行前执行

@AfterGroups	分组测试用例的所有测试用例运行结束后执行

@BeforeClass	在当前测试类的任意一个测试用例开始运行前执行

@AfterClass	在当前测试类的所有测试用例运行结束后执行

@BeforeMethod	在每个测试方法开始运行前执行

@AfterMethod	在每个测试方法运行结束后执行

@Test	被认为是一个测试方法，既一个测试用例

@DataProvider
标志着一个方法，提供数据的一个测试方法。注解的方法必须返回一个Object[] []，其中每个对象[]的测试方法的参数列表中可以分配。
该@Test 方法，希望从这个DataProvider的接收数据，需要使用一个dataProvider名称等于这个注解的名字。

@Factory	作为一个工厂，返回TestNG的测试类的对象将被用于标记的方法。f该方法必须返回Object[]。

@Listeners	定义一个测试类的监听器。

@Parameters	介绍如何将参数传递给@Test方法。

@alwaysRun:   如果=true,表示即使该测试方法所依赖的前置测试有失败的情况，也要执行。

@dataProviderClass: 确定参数构造器的Class类。(参数构造器首先会在当前测试类里面查找，如果参数构造器不在当前测试类定义，那么必须使用该属性来执行它所在的Class类)。

@dependsOnGroups: 确定依赖的前置测试组别。

@dependsOnMethods: 确定依赖的前置测试方法。

@description: 测试方法描述信息。(建议为每个测试方法添加有意义的描述信息，这将会在最后的报告中展示出来)

@enabled: 默认为true，如果指定为false，表示不执行该测试方法。

@expectedExceptions: 指定期待测试方法抛出的异常，多个异常以逗号(,)隔开。

@groups: 指定该测试方法所属的组，可以指定多个组，以逗号隔开。

@invocationCount: 指定测试方法需要被调用的次数。

@invocationTimeOut: 每一次调用的超时时间，如果invocationCount没有指定，该参数会被忽略。应用场景可以为测试获取数据库连接，超时就认定为失败。单位是毫秒。

@priority: 指定测试方法的优先级，数值越低，优先级越高，将会优先与其他数值高的测试方法被调用。(注意是针对一个测试类的优先级)。

@timeout: 指定整个测试方法的超时时间。单位是毫秒。

四、	创建TestNG测试集合
1.	测试集合：在自动化测试的执行过程中，通常会产生批量运行多个测试用例的需求，此需求称为运行测试集合（Test Suite）
2.	Test NG的测试用例可以是相互独立的，也可以按照特定的顺序来执行（配置TestNG.xml）

3.	配置testNG.xml文件

执行多组分组时配置：

在testng.xml中，可以控制测试用例按顺序执行。  当preserve-order="true"是，可以保证节点下面的方法是按顺序执行的


在xml里面使用<include>指定需要执行的方法和顺序


五、	依赖测试（dependsOnMethods）
@Test(dependsOnMethods={“方法名称”})
被依赖的方法优先于此方法执行

六、	特定顺序执行测试用例（priority）
@Test（priority=0/1/2/3/4/…）
按照数字大小顺序优先执行，优先执行1，然后是2…

七、	跳过某个测试方法（enabled=false）
@Test（priority=0/1/2/3/4/…,enabled=false）
执行结束后，在测试报告中显示跳过的测试用例数，例如skip=1

八、	参数化测试（@Parameters-》通过xml文件从外部给测试方法传参）

TestNG 对testng.xml 的参数的类型指定的值会自动尝试转换。下面是支持的类型：
•	String

•	int/Integer

•	boolean/Boolean

•	byte/Byte

•	char/Character

•	double/Double

•	float/Float

•	long/Long

•	short/Short

九、	数据驱动（@DataProvider）
实例：
1.	创建一个java类PrimeNumberChecker.java
      public class PrimeNumberChecker {
      public Boolean validate(final Integer primeNumber) {
      for (int i = 2; i < (primeNumber / 2); i++) {
      if (primeNumber % i == 0) {
      return false;
      }
      }
      return true;
      }
      }
2.	创建测试案例类
      创建一个Java测试类 ParamTestWithDataProvider1.java.

定义方法primeNumbers（），其定义为DataProvider 使用注释。此方法返回的对象数组的数组。

测试方法testPrimeNumberChecker（）添加到测试类中。此方法需要一个整数和布尔值作为输入参数。这个方法验证，如果传递的参数是一个素数。

添加注释 @Test(dataProvider = "test1") 到此方法。dataProvider的属性被映射到"test1".

创建Java类文件名ParamTestWithDataProvider1.java
import org.testng.Assert;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Test;

public class ParamTestWithDataProvider1 {
private PrimeNumberChecker primeNumberChecker;

    @BeforeMethod
    public void initialize() {
        primeNumberChecker = new PrimeNumberChecker();
    }

    @DataProvider(name = "test1")
    public static Object[][] primeNumbers() {
        return new Object[][] { { 2, true }, { 6, false }, { 19, true },
        	{ 22, false }, { 23, true } };
    }

    // This test will run 4 times since we have 5 parameters defined
    @Test(dataProvider = "test1")
    public void testPrimeNumberChecker(Integer inputNumber,
        Boolean expectedResult) {
	System.out.println(inputNumber + " " + expectedResult);
	Assert.assertEquals(expectedResult,
        	primeNumberChecker.validate(inputNumber));
    }
}
3.	创建TESTNG.XML
      创建 testng.xml suite name="Suite1"
      
    <test name="test1">
	<classes>
	    <class name="ParamTestWithDataProvider1" />
	    </classes>
    </test>

/suite


十、	TestNG异常测试
````
@Test(expectedExceptions = IllegalArgumentException.class,expectedExceptionsMessageRegExp="NullPoint")
public void testException(){
       throw new IllegalArgumentException("NullPoint");}
````
十一、TestNG测试报告
TestNG默认情况下，会生产两种类型的测试报告HTML的和XML的。 测试报告位于 "test-output" 目录下.
当然我们也可以设置测试报告的内容级别.
verbose="2" 标识的就是记录的日志级别，共有0-10的级别，其中0表示无，10表示最详细

    <test name="test12" verbose="2">
        <classes>
            <class name="TankLearn2.Learn.TestNGLearn1" />
        </classes>
Reporter.log（“输入自定义内容”）
主要有两种方法来生成报告使用TestNG：
•	监听器: 为了实现一个监听类，类有实现org.testng. ITestListener接口。这些类在运行时通知了TestNG测试开始时，结束后，失败，跳过或传递。
•	记录器: 为了实现一个报表类，实现一个org.testng.IReporter接口。这些类一整套运行结束时调用。调用时，该对象包含整个测试运行的信息传递到这个类。

十二、多线程测试
1.	注解实现

2.	xml实现

1.	alwaysRun ——某些依赖的用例失败了，导致用例被跳过。对于一些为了保持环境干净而 “扫尾” 的测试类，如果我们想强制执行可以使用此标签。
2.	singleThreaded ——强制一个 class 类里的用例在一个线程执行，忽视 method 级别并发
      TestNG 默认会使用线程池的方式创建线程，减小了程序的开销。必须要指出的是，通过多线程执行用例时虽然可以大大提升用例的执行效率，但是我们在设计用例时也要考虑到这些用例是否适合并发执行，以及要注意多线程方式的通病：线程安全与共享变量的问题。建议是在测试代码中，尽可能地避免使用共享变量。如果真的用到了，要慎用 synchronized 关键字来对共享变量进行加锁同步。否则，难免你的用例执行时可能会出现不稳定的情景（经常听到有人提到用例执行地不稳定，有时 100% 通过，有时只有 90% 通过，猜测可能有一部分原因也是这个导致的）。

十三、TestNG常用断言方法

(1) assertTrue：判断是否为True。

(2) assertFalse：判断是否为false。

(3) assertSame：判断引用地址是否相同。

(4) assertNotSame：判断引用地址是否不相同。

(5) assertNull：判断是否为null。

(6) assertNotNull：判断是否不为null。

(7) assertEquals：判断是否相等，Object类型的对象需要实现haseCode及equals方法。

(8) assertNotEquals：判断是否不相等。

(9) assertEqualsNoOrder：判断忽略顺序是否相等。

