# 建造者模式
## 概述
大部分的计算机语言都是面向对象编程，所有的数据和方法都封装在一个类当中，如果我们想要实例化一个类获取到一个对象，我们可以使用new Object（\.\.\.）方法来实例化，这样的方式看起来是不是很简单。但是，你不要忘记了（\.\.\.)里面的参数，如果这里面需要10个或者更多，而且每一个参数也都是一个对象，那么你的代码可能会变成这样
```
new Object（new Object(\.\.\.),new Object(\.\.\.)new Object(\.\.\.)new Object(\.\.\.),\.\.\.)
```
也许你觉得这样的形式会比较夸张，但是在编码中确实存在这样的类。那么我们怎样去快速又简单的去new这样的一个对象出来呢？也许你会想到把这个new的过程封装为一个方法然后自己去调用就是了，但是你还是需要“痛苦”一次才能完成这个过程，并且灵活性不是很高，一旦参数需要修改，你又得添加一个方法，这样不利于代码的重用。也需你还会想到，自己封装一个类，然后先提供默认的参数，接着提供设置这些参数的方法，这样不就满足了所有的需求了吗！确实是功能实现了，灵活性也提高了，但是你有没有想过这样开发者要对这个类特别了解，而且还会写很多代码。其实这已经有一点构造者模式的味道在里面了，我们只需要把种方法需要编写的代码交给这个类的提供者这去实现，而使用者只需要知道这个类有一个构造器Builder就可以了。例如Android中的AlertDialog就需要很多的参数，这里面提供了一个Builder去创建和设置这些参数，这也是典型Builder的应用，让AlerDialog的使用者能够很快的构建出一个对象。接下来我们看一个例子
## 例子
一个人有头（Header）、身体（Body）、脚（Foot）构成，如果我们要实例化一个Person我们就需要传入上面这三个参数
#### 没有使用Builder
如果参数变多，这个构造方法也会变大
```
Person man = new Person(new Header("man header"), new Body("man body"), new Foot("man foot"));
Person woman = new Person(new Header("woman header"), new Body("woman body"), new Foot("woman foot"));
```
#### 使用了Builder
这里我们只需要出传递一个参数就可以快速的创建一个想要的对象了
```
Person manPerson = Person.Builder.createBuilder(new ManBuilder());
Person womanPerson = Person.Builder.createBuilder(new WomanBuilder());
```
## 代码
### 编写一个Header类
这里面只简单的放置了一些参数
#### Header.java
```
package com.design.pattern.builder;

public class Header {
	private String mDescription;

	public Header(String mDescription) {
		this.mDescription = mDescription;
	}

	@Override
	public String toString() {
		return "Header [mDescription=" + mDescription + "]";
	}

}
```
### 编写一个Body类
这里面只简单的放置了一些参数
#### Body.java
```
package com.design.pattern.builder;

public class Body {
	private String mDescription;

	public Body(String mDescription) {
		this.mDescription = mDescription;
	}

	@Override
	public String toString() {
		return "Body [mDescription=" + mDescription + "]";
	}

}
```
### 编写一个Foot类
这里面只简单的放置了一些参数
#### Foot.java
```
package com.design.pattern.builder;

public class Foot {
	private String mDescription;

	public Foot(String mDescription) {
		this.mDescription = mDescription;
	}

	@Override
	public String toString() {
		return "Foot [mDescription=" + mDescription + "]";
	}

}
```
这上面实现了三个需要传入的参数，例子都比较简单，如果是在真是都需求中，这三个类会封装很多的参数在里面，可能会达到上万行的代码。因此实例化也会传入很多的参数
### Person类说明
这里先把Person类给实现了，可以看到构造方法中有很多的参数，还有一个protected的构造方法，但是只能在同包中调用。还可以看到里面有很多的set方法，如果一个一个去设置的话也会有设置很多参数，跟带参数的构造方法差不多。最重要的是里面有一个static class Builder类，里面有一个方法来构造一个Person对象，但是它需要一个PersonBuilder的参数，而这个类的实现就是类的提供这开发的。
#### Person.java

```
package com.design.pattern.builder;

public class Person {
	private Header mHeader;
	private Body mByody;
	private Foot mFoot;

	public Person(Header mHeader, Body mByody, Foot mFoot) {
		this.mHeader = mHeader;
		this.mByody = mByody;
		this.mFoot = mFoot;
	}

	protected Person() {
		super();
	}

	public Header getmHeader() {
		return mHeader;
	}

	public void setmHeader(Header mHeader) {
		this.mHeader = mHeader;
	}

	public Body getmByody() {
		return mByody;
	}

	public void setmByody(Body mByody) {
		this.mByody = mByody;
	}

	public Foot getmFoot() {
		return mFoot;
	}

	public void setmFoot(Foot mFoot) {
		this.mFoot = mFoot;
	}


	@Override
	public String toString() {
		return "Person [mHeader=" + mHeader + ", mByody=" + mByody + ", mFoot=" + mFoot + "]";
	}


	static class Builder {
		public static Person createBuilder(PersonBuilder pb) {
			pb.buildHeader();
			pb.buildBody();
			pb.buildFoot();
			return pb.buildePerson();
		}
	}
}

```
### 接口PersonBuilder
定义了一个接口，这样是的Person的createBuilder可以传入不同的构造器
#### PersonBuilder.java
```
package com.design.pattern.builder;

public interface PersonBuilder {
	void buildHeader();

	void buildBody();

	void buildFoot();

	Person buildePerson();
}

```
### 构造其ManBuilder
ManBuilder实现了PersonBuilder接口
#### ManBuilder.java
```
package com.design.pattern.builder;

import java.util.Set;

public class ManBuilder implements PersonBuilder {
	private Person mPerson;

	public ManBuilder() {
		mPerson = new Person();
	}

	@Override
	public void buildHeader() {
		// TODO Auto-generated method stub
		mPerson.setmHeader(new Header("man header"));
	}

	@Override
	public void buildBody() {
		mPerson.setmByody(new Body("man body"));
	}

	@Override
	public void buildFoot() {
		mPerson.setmFoot(new Foot("man foot"));
	}

	@Override
	public Person buildePerson() {

		return mPerson;
	}

}
```
### 构造其WomanBuilder
WomanBuilder实现了PersonBuilder接口
####WomanBuilder.java
```
package com.design.pattern.builder;

import java.util.Set;

public class WomanBuilder implements PersonBuilder {
	private Person mPerson;

	public WomanBuilder() {
		mPerson = new Person();
	}

	@Override
	public void buildHeader() {
		// TODO Auto-generated method stub
		mPerson.setmHeader(new Header("woman header"));
	}

	@Override
	public void buildBody() {
		mPerson.setmByody(new Body("woman body"));
	}

	@Override
	public void buildFoot() {
		mPerson.setmFoot(new Foot("woman foot"));
	}

	@Override
	public Person buildePerson() {

		return mPerson;
	}

}
```
## 总结
这上面的例子只是Builder实现的方式的一种，它还有很多变形的实现方式，但是万变不离其踪，其目的都是为了让开发者方便使用这个类构，并且快速构造一个想要的对象出来
