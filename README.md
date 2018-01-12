# php-static
php静态的使用—单态设计模式
 
PHP单例模式，就是一个对象只被生成一次，但该对象可以被其它众多对象使用。
单例模式使用最多的场景，是数据库连接操作。
我们知道，生成一个对象的操作是用new函数来实现，但是new对象都会消耗内存，
而且有时候对同一个对象，在不同文件中可能会生成多次，
这就造成了系统资源的浪费。然而使用单例模式，则可以很好的避免这种情况。

以数据库为例，假设我们有一个数据库的类，要实现数据库连接。如果不使用单例模式，
那么在很多PHP文件中，我们可能到要创建这样的一个连接，这其实是对资源的很大浪费。


                        	static使用规则
  		1. 使用static可以修饰成员属性和成员方法，不能修饰类
 
  		2. 用static修饰的成员属性，可以被同一个类的所有对象共享
 		
  		3. 静态的数据是存在 内存中的 数据段中（初使化静态段）
 
 	  	4. 静态的数据是在类第一次加载时 分配到内存中的， 以后再用到类时就直接从数据段中获取
 
 		  5. 什么是类被加载？ 只要在程序中使用到这个类（有这个类名出现）

    	注意： 静态的成员都要使用类名去访问，不用创建对象, 不用对象去访问
 			
 			类名::静态成员
 
 		  如果在类中使用静态成员， 可以使用 self代表本类（$this是代表本对象）
 
  		
 
 
 		  6. 静态方法（static修饰的方法）， 不能访问非静态的成员（在非静态的方法中，可以访问静态成员）
 
 		   因为非静态的成员， 就必须用对象来访问，访问内部的成员使用的就是$this
 
 		   静态方法 不用使用对象来调用， 也就没有对象， $this也就不能代表什么对象， 非静态的成员还必须使用对象
 
 	    	如果你确定一个方法不使用非静态的成员， 则可以将这个方法声明为 静态方法（不能创建对象，直接使用类名就可以访问）		
 			
 
 			  静态成员： 类名::成员 ， 在类内部访问其它成员 self::成员
 
 

   
                          那么下面介绍单例模式实现方法:
       

                                  class Database
                                  {
                                     //定义一个属性，该属性是静态的保护或私有属性
                                     protected static $db;

                                     //这里构造函数一定要是私有方法
                                     private function __construct()
                                     {  

                                      }

                                     //声明一个获取类实例的方法
                                     static function getInstace()
                                     {  
                                         if(self::$db) {
                                            return self::$db;
                                         }else {
                                            //生成自己
                                            self::$db = new self();
                                            return self::$db;
                                         }  
                                      }
                                  }


                                //错误调用方法
                                //用new实例化private标记构造函数的类会报错
                                $db = new Database();

                                //正确获取实例方法
                                $db = Database::getInstace();
       
       


                                   再来一个例子 
                                       class Msqs{
                                           protected 	static $name="99999";
                                           private function __construct(){
                                             echo '连接数据库成功';	
                                           }
                                          static  function setData($sql){
                                            self::$name=$sql;
                                           } 

                                           static function getData(){
                                            if(self::$name){
                                             return self::$name;
                                            }else{
                                            self::$name=new self;
                                                return self::$name;	
                                            }
                                           }

                                          }
                                       修改静态成员
                                         Msqs::setData("蔺雨轩");

                                        $s=  Msqs::getData();
                                        echo $s;     //蔺雨轩

                              
                              
                              
   
 使用单例模式的好处是，当你在其他地方也要使用到这个类，比如上面的数据库类。
 那么你可以在其它地方直接调用 Database::getInstace(），而且该实例只会被生成一次，不会被重复生成，所以不会浪费系统资源。

  简单的说，单例模式生成的实例只被生成一次，而且只负责一个特定的任务       
       
    
使用单例模式有下面几个要求：

      1.构造函数需要标记为private（访问控制：防止外部代码使用new操作符创建对象），单例类不能在其他类中实例化，只能被其自身实例化;

      2.拥有一个保存类的实例的静态成员变量;

      3.拥有一个访问这个实例的公共的静态方法（常用getInstance()方法进行实例化单例类，通过instanceof操作符可以检测到类是否已经被实例化）;

      4.如果严谨的话，还需要创建__clone()方法防止对象被复制（克隆）。(我上面没创建)

使用单例模式好处，总结：

      1、php的应用主要在于数据库应用, 所以一个应用中会存在大量的数据库操作, 使用单例模式, 则可以避免大量的new 操作消耗的资源。

      2、如果系统中需要有一个类来全局控制某些配置信息, 那么使用单例模式可以很方便的实现。

      3、在一次页面请求中, 便于进行调试。    

       
       
       
       
       
       
       
       
       
       
       
       
       
       
       
       
       
