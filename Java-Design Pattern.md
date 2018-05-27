
设计模式（Design pattern）是一套被反复使用、多数人知晓的、经过分类编目的、代码设计经验的总结。使用设计模式是为了可重用代码、让代码更容易被他人理解、保证代码可靠性

----------------------------------------------------------------------------------------------------------------------------------------

java的设计模式大体上分为三大类：

创建型模式（5种）：工厂方法模式，抽象工厂模式，单例模式，建造者模式，原型模式。

结构型模式（7种）：适配器模式，装饰器模式，代理模式，外观模式，桥接模式，组合模式，享元模式。

行为型模式（11种）：策略模式、模板方法模式、观察者模式、迭代子模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

----------------------------------------------------------------------------------------------------------------------------------------

设计模式遵循的原则有6个：

1、开闭原则（Open Close Principle）

　　对扩展开放，对修改关闭。

2、里氏代换原则（Liskov Substitution Principle）

　　只有当衍生类可以替换掉基类，软件单位的功能不受到影响时，基类才能真正被复用，而衍生类也能够在基类的基础上增加新的行为。

3、依赖倒转原则（Dependence Inversion Principle）

　　这个是开闭原则的基础，对接口编程，依赖于抽象而不依赖于具体。

4、接口隔离原则（Interface Segregation Principle）

　　使用多个隔离的借口来降低耦合度。

5、迪米特法则（最少知道原则）（Demeter Principle）

　　一个实体应当尽量少的与其他实体之间发生相互作用，使得系统功能模块相对独立。

6、合成复用原则（Composite Reuse Principle）

　　原则是尽量使用合成/聚合的方式，而不是使用继承。继承实际上破坏了类的封装性，超类的方法可能会被子类修改。

----------------------------------------------------------------------------------------------------------------------------------------

单例模式
--
所谓单例设计模式简单说就是无论程序如何运行，采用单例设计模式的类（Singleton类）永远只会有一个实例化对象产生。具体实现步骤如下：

      (1) 将采用单例设计模式的类的构造方法私有化（采用private修饰）。

      (2) 在其内部产生该类的实例化对象，并将其封装成private static类型。

      (3) 定义一个静态方法返回该类的实例。
      
四种单例模式：

1. 单例模式的实现：饿汉式,线程安全 但效率比较低  
     
          public class SingletonTest {   
              private SingletonTest() {   
              }   
              private static final SingletonTest instance = new SingletonTest();   
              public static SingletonTest getInstancei() {   
                  return instance;   
              }   
          } 


2. 单例模式的实现：饱汉式,非线程安全   

            public class SingletonTest {   
                private SingletonTest() {   
                }   
                private static SingletonTest instance;   
                public static SingletonTest getInstance() {   
                    if (instance == null)   
                        instance = new SingletonTest();   
                    return instance;   
                }   
            }  
            
3.线程安全，但是效率非常低  

            public class SingletonTest {   
                private SingletonTest() {   
                }   
                private static SingletonTest instance;   
                public static synchronized SingletonTest getInstance() {   
                    if (instance == null)   
                        instance = new SingletonTest();   
                    return instance;   
                }   
            }  
            
4.线程安全  并且效率高  

      public class SingletonTest {   
          private static SingletonTest instance;   
          private SingletonTest() {   
          }   
          public static SingletonTest getIstance() {   
              if (instance == null) {   
                  synchronized (SingletonTest.class) {   
                      if (instance == null) {   
                          instance = new SingletonTest();   
                      }   
                  }   
              }   
              return instance;   
          }   
      }  
      
----------------------------------------------------------------------------------------------------------------------------------------
工厂方法模式
--

程序在接口和子类之间加入了一个过渡端，通过此过渡端可以动态取得实现了共同接口的子类实例化对象。


    interface Animal { // 定义一个动物的接口  
        public void say(); // 说话方法  
    }   

    class Cat implements Animal { // 定义子类Cat  
        @Override  
        public void say() { // 覆写say()方法  
            System.out.println("我是猫咪，喵呜！");   
        }   
    }   

    class Dog implements Animal { // 定义子类Dog  

        @Override  
        public void say() { // 覆写say()方法  
            System.out.println("我是小狗，汪汪！");   
        }   
    }   

    class Factory { // 定义工厂类  
        public static Animal getInstance(String className) {   
            Animal a = null; // 定义接口对象  
            if ("Cat".equals(className)) { // 判断是哪个子类的标记  
                a = new Cat(); // 通过Cat子类实例化接口  
            }   
            if ("Dog".equals(className)) { // 判断是哪个子类的标记  
                a = new Dog(); // 通过Dog子类实例化接口  
            }   
            return a;   
        }   
    }   

    public class FactoryDemo {   

        public static void main(String[] args) {   
            Animal a = null; // 定义接口对象  
            a = Factory.getInstance(args[0]); // 通过工厂获取实例  
            if (a != null) { // 判断对象是否为空  
                a.say(); // 调用方法   
            }   
        }   
    }  


----------------------------------------------------------------------------------------------------------------------------------------
代理设计模式
--

   指由一个代理主题来操作真实主题，真实主题执行具体的业务操作，而代理主题负责其他相关业务的处理。比如生活中的通过代理访问网络，客户通过网络代理连接网络（具体业务），由代理服务器完成用户权限和访问限制等与上网相关的其他操作（相关业务）。

    interface Network { // 定义Network接口  
        public void browse(); // 定义浏览的抽象方法  
    }   

    class Real implements Network { // 真实的上网操作  
        public void browse() { // 覆写抽象方法  
            System.out.println("上网浏览信息！");   
        }   
    }   

    class Proxy implements Network { // 代理上网  
        private Network network;   

        public Proxy(Network network) {// 设置代理的真实操作  
            this.network = network; // 设置代理的子类  
        }   

        public void check() { // 身份验证操作  
            System.out.println("检查用户是否合法！");   
        }   

        public void browse() {   
            this.check(); // 调用具体的代理业务操作  
            this.network.browse(); // 调用真实的上网操作  
        }   
    }   

    public class ProxyDemo {   
        public static void main(String args[]) {   
            Network net = null; // 定义接口对象  
            net = new Proxy(new Real()); // 实例化代理，同时传入代理的真实操作  
            net.browse(); // 调用代理的上网操作   
        }   
    }  

----------------------------------------------------------------------------------------------------------------------------------------
观察者设计模式
--

所谓观察者模式，举个例子现在许多购房者都密切观察者房价的变化，当房价变化时，所有购房者都能观察到，以上的购房者属于观察者，这便是观察者模式。
java中可以借助Observable类和Observer接口轻松实现以上功能。当然此种模式的实现也不仅仅局限于采用这两个类。

    import java.util.Observable;   
    import java.util.Observer;   

    class House extends Observable {   
        private float price;   

        public void setPrice(float price) {   
            this.setChanged();// 设置变化点  
            this.notifyObservers(price);// 通知所有观察者价格改变  
            this.price = price;   
        }   

        public float getPrice() {   
            return this.price;   
        }   

        public House(float price) {   
            this.price = price;   
        }   

        public String toString() {   
            return "房子价格为: " + this.price;   
        }   
    }   

    class HousePriceObserver implements Observer {   
        private String name;   

        public HousePriceObserver(String name) {   
            super();   
            this.name = name;   
        }   

        @Override  
        public void update(Observable o, Object arg) {// 只要改变了 observable 对象就调用此方法  
            if (arg instanceof Float) {   
                System.out.println(this.name + "观察的价格更改为:"  
                        + ((Float) arg).floatValue());   
            }   

        }   

    }   

    public class ObserDeom {   
        public static void main(String[] args) {   
            House h = new House(1000000);   
            HousePriceObserver hpo1 = new HousePriceObserver("购房者A");   
            HousePriceObserver hpo2 = new HousePriceObserver("购房者B");   
            HousePriceObserver hpo3 = new HousePriceObserver("购房者C");   
            h.addObserver(hpo1);// 给房子注册观察者   
            h.addObserver(hpo2);// 给房子注册观察者   
            h.addObserver(hpo3);// 给房子注册观察者   
            System.out.println(h);// 输出房子价格   
            // 修改房子价格，会触发update(Observable o, Object arg)方法通知购房者新的房价信息  
            h.setPrice(2222222);//   
            System.out.println(h);// 再次输出房子价格   
        }   
    }  

----------------------------------------------------------------------------------------------------------------------------------------
适配器设计模式
--

如果一个类要实现一个具有很多抽象方法的接口，但是本身只需要实现接口中的部分方法便可以达成目的，所以此时就需要一个中间的过渡类，但此过渡类又不希望直接使用，所以将此类定义为抽象类最为合适，再让以后的子类直接继承该抽象类便可选择性的覆写所需要的方法，而此抽象类便是适配器类

    interface Window {// 定义Window窗口接口，表示窗口操作  
        public void open();// 窗口打开  

        public void close();// 窗口关闭  

        public void iconified();// 窗口最小化  

        public void deiconified();// 窗口恢复  

        public void activated();// 窗口活动  
    }   

    // 定义抽象类实现接口，在此类中覆写方法，但是所有的方法体为空   
    abstract class WindowAdapter implements Window {   
        public void open() {   
        };// 窗口打开   

        public void close() {   
        };// 窗口关闭   

        public void iconified() {   
        };// 窗口最小化   

        public void deiconified() {   
        };// 窗口恢复   

        public void activated() {   
        };// 窗口活动   
    }   

    // 子类继承WindowAdapter抽象类，选择性实现需要的方法   
    class WindowImpl extends WindowAdapter {   
        public void open() {   
            System.out.println("窗口打开");// 实现open()方法  
        }   

        public void close() {   
            System.out.println("窗口关闭");// 实现close()方法  
        }   
    }   

    public class AdapterDemo {   
        public static void main(String args[]) {   
            Window win = new WindowImpl(); // 实现接口对象  
            // 调用方法   
            win.open();   
            win.close();   
        }   
    }  

------------------------------------------------------------------------------------------------------------------------------------------


------------------------------------------------------------------------------------------------------------------------------------------
