# 单例模式
## 概述
单例模式是编程中最简单的、最常用的模式之一，它要求应用中只有一个对象，所有的地方都使用这一个实例，因此构造函数必须使用private修饰，内部需要提供一个外界可以访问的实例，这个实例需要使用public static修饰。内部提供实例的方法很多，有饿汉模式、懒汉模式、双检锁/双重校验锁、登记式/静态内部类、枚举等方式来进行实现，有的复杂有得简单，这些实现的方式可以满足不同的场景中的需要
## 饿汉模式
饿汉模式是不管是否有地方需要使用，只要程序一启动就实例化一个对象.这中方适用于耗内存少、实例化快的情况，是绝对的线程安全的单例模式
```
package com.design.mode.singleton;

/**
 * 饿汉模式
 *
 * 在变更量声明的时候就把对象给实例化出来了
 *
 * @author user
 *
 */
public class SingletonOne {
	private static SingletonOne mInstance = new SingletonOne();

	private SingletonOne() {
		System.out.println("Constructor SingletonOne");
	}

	public static SingletonOne getInstance() {
		return mInstance;
	}

}

```
## 懒汉模式
懒汉模式是在有使用的时候才进行实例化，分为线程安全的和线程不安全两种，但是先方式大体相同
### 线程不安全
这是一种线程不安全的方式，在多线程访问中容易返回null
```
package com.design.mode.singleton;

/**
 * 懒汉模式
 *
 * 在第一次使用的时候才实例化
 * 是一种线程不安全的单例模式
 *
 * @author user
 *
 */
public class SingletonTwo {
	private static SingletonTwo mInstance;

	private SingletonTwo() {
		System.out.println("Constructor SingletonTwo");
	}

	public static SingletonTwo getInstance() {
		if (mInstance == null) {
			mInstance = new SingletonTwo();
		}
		return mInstance;
	}

}
```
### 线程安全
这中方是是线程安全的，但是在多线程访问中需要同步，所以会降低访问你的效率
```
package com.design.mode.singleton;

/**
 * 懒汉模式
 *
 * 在第一次使用的时候才实例化
 * 是一种线程不安全的单例模式
 *
 * @author user
 *
 */
public class SingletonTwo {
	private static SingletonTwo mInstance;

	private SingletonTwo() {
		System.out.println("Constructor SingletonTwo");
	}

	public static synchronized SingletonTwo getInstance() {
		if (mInstance == null) {
			mInstance = new SingletonTwo();
		}
		return mInstance;
	}

}
```
## 双检锁/双重校验锁
这中方式是中和了两种懒汉模式的有点实现的一种双重校验锁方式，它是在第一次使用的时候才进行实例化，效率高、线程安全，可以满足大部分系统的需求，但是不能使用与高并发的系统当中
```
package com.design.mode.singleton;

/**
 * 双检锁/双重校验锁
 *
 * 在第一次使用的时候才实例化 是線程安全的，但是不能使用于高并发的线程中
 *
 * @author user
 *
 */
public class SingletonThree {
	private static SingletonThree mInstance;

	private SingletonThree() {
		System.out.println("Constructor SingletonThree");
	}

	public static SingletonThree getInstance() {
		if (mInstance == null) {//高并发的时候可能会出错
			synchronized (SingletonThree.class) {
				if (mInstance == null) {
					mInstance = new SingletonThree();
				}
			}
		}
		return mInstance;
	}
}

```
## 登记式/静态内部类
这中方式是采用静态类部类来实现的，它是在第一次使用的时候才实例化、高效率、线程安全的、可以用与高并发的系统中的单例模式。也是编者推荐使用的一种方式。
```
package com.design.mode.singleton;

/**
 * 登记式/静态内部类
 *
 * 这种方式是线程安全的，推荐使用
 *
 * @author user
 *
 */
public class SingletonFour {
	private SingletonFour() {
		System.out.println("Constructor SingletonFour");
	}

	public static final SingletonFour getInstance() {
		return SingletonHolder.mInstance;
	}

	private static class SingletonHolder {
		private static final SingletonFour mInstance = new SingletonFour();
	}
}

```
## 枚举
才用枚举的方式来实现但里，枚举的对象在使用的时候才会别实例化，这种方式也是拥有前面所有的优点，而且可以有多个定义的实例,不过多个实例会在第一次使用的时候全部创建出来，并且代码也相当的简洁
```
package com.design.mode.singleton;

/**
 * 枚举
 *
 * @author user
 *
 */
public enum SingletonFive {

	mInstance;
	private SingletonFive() {
		System.out.println("Constructor SingletonFive");
	}

}

```
## 总结
单例模式的实现方式有很多种，选用那一种应该根据场景的需要而选择，推荐尽量选择线程安全和效率高的实现方式
