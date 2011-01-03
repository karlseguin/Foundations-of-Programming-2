# Foundations of Programming 2 #
> "If at first you don't succeed, try, try, try again" - William E. Hickson

## License ##
Foundations of Programming 2 is licensed under the Attribution-NonCommercial 3.0 Unported license. **You should not have paid for this book.**

You are basically free to copy, distribute, modify or display the book. However, I ask that you always attribute the book to me, Karl Seguin and do not use it for commercial purposes.

You can see the full text of the license at:
<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

## Latest Version ##
This book is currently in development. The final release is expected to be in early February 2011.

To get the latest version, visit <http://github.com/karlseguin/foundations2>.

## About The Author ##
Karl Seguin is a developer with experience across various fields and technologies. He's an expert .NET developer and burgeoning Ruby developer. He's familiar with all aspects of web development, concurrent and socket programming and various SQL and NoSQL technologies. He's a semi active contributor to OSS projects, a technical writer and an occasional speaker.

His blog can be found at <http://openmymind.net>, and he tweets via [@karlseguin](http://twitter.com/karlseguin)

He's also the founder of the free services for casual game developers <http://mogade.com>.

## Introduction ##
I'm always amazed by the number of passionate programmers I run into. So much of our life is dedicated to work that we are fortunate for the opportunity to spend that time doing what we love. This isn't a unique property of programmers, but it's uncommon enough to be surprising to most people. "You go home and *work* every day?" we are asked. Only our closest friends, and passionate colleagues really understand that *work* has two meanings for us, neither of which is a 9-5 drudge.

You've probably noticed that enthusiasm and drive is a pretty critical thing to have in such a young field. Technology continues to change at a rapid pace, the field is growing, and systems are getting increasingly complex. The result is a clear divide between programmers who love their craft and those who don't. You're either learning, trying and sometimes failing, or you're stuck in the past, likely oblivious to just how far behind you've fallen.

Unfortunately, being passionate about what you do isn't always enough. We all have different strengths and weaknesses, different ways to learn, and different tolerance towards failure. There's also a constant stream of new technologies which makes it hard to figure out what to spend your time learning. The truth is that I'm not a great programmer. I'm a good programmer who likes to learn (and teach) and who enjoys playing with technology. I've seen people pick up new languages in days when its taken me months, or write algorithms that I had to pretend to agree with because I just wasn't following. The Foundations of Programming series is meant to help people like me, people who care about what they do, want to do it better, but aren't always sure how to proceed.

I hope this book will help you.

### With Respect To The Original ###
The [original Foundations of Programming](https://github.com/karlseguin/Foundations-of-Programming-Ebook) has been, by my own standards, a success. It seems to have genuinely helped developers and served a need which clearly needed serving. Each book, the original and this one, stand on their own; they are independent from each other. However, I would suggest that if you plan on reading the original, you do so before continuing with this book. My views have matured and in some cases changed or clarified. I think much can be gained by seeing how I've evolved as a programmer.

## Chapter 1 - Quality and Efficiency ##
>"Quality is never an accident; it is always the result of intelligent effort." - John Ruskin

Pragmatically, the reason I, and I assume most programmers, are keen to learn is to improve the quality of the code we write as well as our efficiency. I don't know any programmer who spends time learning new approaches and technologies with the explicit goal of being able to write worse code, or find ways to slow his or her pace. How do we measure efficiency and quality though? What, as creators, do we consider to be our goals?

Management-type might solely align quality with client/user satisfaction. While that's certainly important, good programmers are almost always the stakeholders with the highest quality expectation as well as being the most critical. As DeMarco and Lister said in Peopleware, **We all tend to tie our self-esteem strongly to the quality of the product we produce - not the quantity of the product, but the quality;.** I find it difficult to put quality, from a programmers perspective, into words; yet, I'm sure most of us not only have a similar definition, we can all easily recognize it (or the lack of it). Quality is about coming up with an elegant solution to a problem - both at the micro (an individual behavior) and macro levels (the system as a whole). What's *elegant* then? It varies on the context of the problem, but generally, an elegant solution is both the simplest possible one as well as the most explicit and easy to understand.

The more I've program, the more I've notice something truly startling. Not only is the simplest solution obviously the easiest to implement, its almost certainly the easiest to understand, maintain and change. Simple solutions also tend to have as good or better performance characteristics than more complex approaches. This is particularly astounding when you find yourself knee-deep in an over-engineered mess where the original intention for flexibility, performance and efficiency are sabotaged by artificial complexity. That doesn't mean I advocate programming without consideration of design. Instead, I believe that given some experience and reflection, a balance between forward-thinking design and pragmatism isn't only achievable, its quite natural?

I wish I could tell you the answer. It isn't that I don't know. The problem is that I have to preface it with a word of warning; else I risk having you roll your eyes and give up on this book. I know a lot of you are sick of hearing about it, but I beg that you hear me out and recognize that I'm probably not saying what you think I'm saying. Deal? So the secret to writing quality code is to make sure your code is testable. WAIT...don't roll those eyes, I'm not saying that you have to test your code (and I'm certainly not saying that you shouldn't). I'm just saying that if, theoretically, you were to test your code, it shouldn't be hard to do. Code can be testable without actually testing it - and ultimately, at the most fundamental level, that's what you should be aiming for.

For a long time I thought the solution really was that simple - write tests, and the quality of your code will greatly improve. More recently though I've realized that not all tests are created equally and it's entirely possible, if not common, to have really poor tests. Understanding what makes a good test, and thus what makes code testable, comes with experience, and hopefully, through future chapters, I can help you avoid some of the pitfalls I ran into. Therefore, much of this book is going to be devoted to both writing tests and testability. Do not mix testability as a quality metrics with writing tests. It's a lot like writing maintainable code that doesn't really require maintenance - it's a goal, not an actual activity.  Now it's true that the best way to discover whether a solution is testable is to actually write tests. I do hope that I can convince you to give it a try; in the last three years, becoming an effective test writer has been one of the two things which has most helped me elevate my craft. Nonetheless, it's entirely possible to look at a method, or a system, and gauge testability, and thus quality, without actually writing a test.

### Testability as a Quality Metric ###
I don't blame you if you're skeptical. All I can do is promise that, while we might spend some time on writing effective tests, I won't try to make you feel like you are doing something wrong if you choose not to test. Also, I can show you a simple example of what I mean:

	//An example of hard to test, and thus low-quality, code
	private void LoginButton_Click(object sender, EventArgs e)
	{
		User user = SqlServerDataStore.FindUser(UserName.Text, Password.Text);
		if (user != null)
		{
			Session["userId"] = user.Id;
			Response.Redirect("~/members/index.aspx");
		}
		else
		{
			ErrorMessage.Text = "Invalid UserName or Password";
		}
	}
(If this was a short-lived/throw away app, we might forgive it. Although I must say that in my experience, skilled programmers write clean and thoughtful code regardless of the expected life expectancy of our code. This is likely due to the fact that we all know *prototype* code can end up having an embarrassingly long shelf-life.)
 
This snippet is far from perfect. If we specifically look at it from a testing point of view, we'll see a number of dependencies which will make this hard to test, such as `SqlServerDataStore.FindUser` and `Response.Redirect`, to name a few. Sadly, many of those are baked into the framework - which is a one of the reasons many .NET developers feel so strongly against WebForms. In this particular case, the near impossibility of testing tells me that the code will be difficult to change and maintain.

A lot of the best practices you hear thrown around, like YAGNI (you aren't going to need it), low coupling and high cohesion can be measured by looking at your code and thinking of how you'd test it. A method that does too much, potentially violating both YAGNI and high cohesion, will require a painful amount of setup code to test (as well as prove easy to break).

There's a programming acronym that gets used a lot called SOLID. It stands for five important principles of object oriented design: single responsibility principle, open/close principle, Liskov substitution principle, Interface segregation principle and dependency inversion principle. These are all topics worthy of their own chapter (there's an ideal), but suffice it to say that most of them are somewhat ambiguous. At what point is a new behavior or enhancement considered an additional responsibility of a given component? When has an interface become too complex? These are important questions and the wrong answer can will have consequences. Testability and experience are the tools you use to remove this ambiguity. (A good place to start to learn more about SOLID is <a href="http://en.wikipedia.org/wiki/Solid_(object-oriented_design)">Wikipedia</a>.)

### Efficiency: Not As Simple As It Seems ###
I might not have sold you on the testability as a quality metric (yet), but I'm confident you agree that quality is a supremely important aspect. Our other pragmatic-driven goal is efficiency, because great code can't wait forever. You'd hope that efficiency, with respect to learning and improving, would be easy to measure: can I achieve XYZ now faster than I could before? This isn't the case. 

First, and most dangerously, programmers don't always realize that a better way may exist. Let's be honest, a lot of programmers refuse to accept that very possibility. It's hard for people, especially those not passionate about what they do, to accept that a better way exists - it not only threatens their comfort zone, but potentially their careers. I was in school when Java started gaining popularity and was impressed by the close-mindedness of programmers who refused to accept that automatic garbage collection might, after working out some quirks, be a useful (let alone standard) general purpose solution. I find it particularly ironic now when Java developers refuse to accept that dynamic languages might become the new Java (especially since they often raise the same concerns which C++ developers once had about Java).

Secondly, regardless of how much more efficient a new technology or approach is, you'll be more efficient in what you know versus what you're learning. My strategy to surmount this is to take my time and recognize the process as a long term investment. Also if you are clever about it, you can probably fit some R&D into your day to day work. By clever, I don't mean deceitful. I mean finding secondary, non-critical systems. That monitoring system you've talked about with colleagues but never had time to do, or that prototype you've been asked to write. The exact approach you take will be personalized to your preferred way to learn. All I can say is that you shouldn't expect an increase in efficiency overnight.

### In This Chapter ###
That was a lot of text with too little code. This chapter was the groundwork for following chapters, which rely heavily on code and examples. We defined what quality means to us as well as the metric we'll use to measure it (testability). We looked at efficiency and discovered that while it might seem easy to gauge, it really isn't. Take a break and consider some of the code you've recently written. If you did test it, what could have made your tests simpler and less likely to fail as your system changes? If you didn't, can you think of any pain points you might have run into if you decided to test it now?

## Chapter 2 - Yet Another IoC Introduction ##
> "The purpose of software engineering is to control complexity, not to create it." - Pamela Zave

Depending on what technology you use, Inversion of Control (IoC) and Dependency Injection (DI) may or may not be something you read about and spend energy on. In some languages IoC and DI play a significant and explicit role in development. In others, it's barely visible. That doesn't mean that IoC is less important or fundamental in some languages than in others. What is different is the mechanism used to achieve IoC, which is what's truly fascinating about the subject and why we are going to spend the next couple chapters learning more about it.

Our plan is to start with a brief introduction on IoC in this chapter. Then we'll look at the problem from different perspectives. The purpose isn't to label one approach better than another. Rather, our goal is to expand how we see, and possibly approach, a core challenge we constantly face while coding. When I finally saw IoC beyond the narrow understanding that I was introduced to, I felt like a new world had been opened up to me. Not because this is earth shattering knowledge - in fact, it probably won't even change how you code. What it did for me was validate the importance of expanding my knowledge beyond my comfort zone. It reinforced how ignorant I was (and still am), and knowing that you are ignorant is key to being a successful programmer.

### A Word on Coupling ###
Before we look at IoC, let's understand the problem we are trying to solve. Coupling is what you get when something is dependent on something else. That something can be an assignment, a method, a class or even a whole system. Some examples:

	#The simplest code can couple us to another class or implementation
	time = Time.now
	
	//...Something slightly more complex
	$('#logs').load('/orders/history', {id: _id});
	
	//...Or something a lot bigger
	var richList = Session.Query<Account>().Where(a => a.Amount > 10000000).List();

In each of the above examples, our code is dependent on some other implementation. Practically every line of code you write will, technically speaking, generate coupling. A lot of the time coupling is benign - no one's suggesting that you wrap every core library/type. However, more complex cases often lead to testability challenges, which indicate that code is hard to change and maintain.  The last example is the most obvious as it's impossible to test without actually hitting the database (we'll talk more about that in a future chapter, because hitting the database isn't at all a bad idea).

The kinds of tight coupling we want to avoid are those that introduce dependencies between independent components or systems. Admittedly, saying that coupling makes it difficult to change the implementation isn't always compelling - sometimes you can be reasonably certain that the implementation is **not** going to change. However, tight coupling will also make it more difficult to maintain, reuse and refactor your code.

### Inversion of Control Basics ###
If coupling (having X depend on Y) is the problem, then Inversion of Control is the solution. IoC is an umbrella term for various solutions that help us decouple or, at the very least, loosen coupling. The general idea is to change the normal (procedural) flow for something you have greater control over. To better understand the concept, let's look at the most common form of IoC in the .NET/Java world: Dependency Injection (DI).

The name *Dependency Injection* is pretty telling of what the practice entails: injecting dependencies into our code rather than statically defining them (hard coding). We look at this form of IoC first not because it's better or simpler, but because the result is particularly explicit and because it's an approach that is independent of the language/framework/technology we are using.

Take the following example:

	public class UserRepository
	{
		public User FindByCredentials(string username, string password)
		{
			var user = SqlDataStore.FindOneByNamedQuery("FindUserByUserName", new {username = username});
			if (user == null) { return null; }
			return BCrypt.CheckPassword(user.Password, password) ? user : null;
		}
	}

This code has two dependencies which we'd do well to decouple: the first being `SqlDataStore` and the other the `BCrypt` library. The idea behind Dependency Injection is to take those two dependencies and supply them to our class/method, rather than having them hard coded. This can be done by passing them as arguments to our method, setting properties of our class, or, the most common approach, supplying them as constructor arguments. Each approach has its own advantages and drawbacks. They all provide the same benefits though: we externalize our dependencies and, in a statically typed world, can program against an interface. Here's the same code using Dependency Injection at the constructor level:

	public class UserRepository : IUserRepository
	{
		private IDataStore _store;
		private IEncryption _encryption;
		public UserRepository(IDataStore store, IEncryption encryption)
		{
			_store = store;
			_encryption = encryption;
		}
		
		public User FindByCredentials(string username, string password)
		{
			var user = _store.FindOneByNamedQuery("FindUserByUserName", new {username = username});
			if (user == null) { return null; }
			return _encryption.CheckPassword(user.Password, password) ? user : null;
		}	
	}

(For completeness sake, we made `UserRepository` implement an interface as well so that it too can now be injected into calling code).

Our code now shields us from direct implementations, making it easier to change, maintain and test. Also, while the `FindByCredentials` method might be seen by some as slightly less explicit (which I agree with), if you really think about it, you'll find that the `UserRepository` class as a whole is now more explicit. You can quickly look at the `UserRepository` constructor and gain a good understanding of *how* it achieves what it does. Yet another benefit, which we'll talk more about in a following chapter, is that constructor injection helps keep our classes cohesive (having a narrow, defined, purpose) - as having too many dependencies often means having a class that does too much.

From the above example you should be able to guess what method and property injection look like. Injecting into a method is useful when only that method has a specific dependency (though it should be used sparingly as it's generally not a great sign about the cohesiveness between the method and its class). Property injection is great for optional dependencies (which is also quite rare). Property injection is also useful for internal framework code when you don't want or need inheriting classes having to expose complex constructors.

### Dependency Injection Frameworks ###
In the communities where DI is a common pattern, DI frameworks are readily available and talked about; thus we'll keep this rather brief. What you need to know is that the DI example we looked at above, when manually done, can add a noticeable amount of overhead to our coding. If, from user input to external service (database, web service, etc.), our code is roughly 4-5 levels deep, and your average class might have a dependency on 1-3 other classes/components, then tracking and instantiating our objects isn't going to be a whole lot of fun.

This is where DI frameworks come into play. You configure them with the dependencies you want to use, and let the framework manage them handle object instantiation. In a way, you can think of them like the `new` keyword on steroids - able to figure out what parameters an object's constructor requires and how to create them (because they themselves might have dependencies which need to be resolved). This is known as auto-wiring.

Let's look at an example using [Ninject](http://ninject.org/). The first thing we do is configure our DI framework. The details of this will vary based on the framework you use, Ninject uses a fluent interface.

	private sealed class BoostrapDependencyModule : NinjectModule
	{
		public override void Load()
		{
			Bind<IUserRepository>().To<UserRepository>();
			Bind<IDataStore>().To<SqlDataStore>();
				
			//makes this a singleton
			Bind<IEncryptor>().To<Encryptor>().InSingletonScope();
		}
	}

We can now create an Ninject kernel and ask it for instances:

	var kernel = new StandardKernel(new BoostrapDependencyModule());
	var repository = kernel.Get<IUserRepository();

The DI framework will see that the constructor for `UserRepository` (the configured instance for `IUserRepository` that we are asking for) requires two dependencies which it is aware of and thus be able to create an instance for us.

Keep in mind that the goal of DI frameworks isn't to make your code dynamically pluggable. The idea isn't to be able to hot-swap your `SQLServerDataStore` with a `PostgreSQLServerDatastore`. It's simpler than that. We want to program against interfaces and have those interfaces injected where necessary. It's something we can do manually, but even simple examples can be a pain. The DI framework automates a very small, yet important, part of the process (object creation with auto-wiring).

It's hard to pass up an opportunity to complain about XML-based configuration, so...Most .NET frameworks provide both code-based (as seen above) and XML-based configuration. The benefit of code-based is that you are able to refactor as well as test your configuration. There's no advantage to XML-based configuration, though some developers will state that with XML they don't need to recompile and retest their code. I say that's crap: a change to your configuration, regardless of where it's stored, needs to go through the same QA and deployment procedures as any other code change.

### Dependency Injection Framework Anti Pattern ###
Ending our introduction on Dependency Injection with what we covered above would be a disservice. We've covered the mechanics of DI and DI Frameworks, but in focusing on the *what*, we've introduced some pretty nasty *hows*. Our `kernel` instance from the last example is thread-safe, and we could create a static class and call something like `Factory.Get<T>` everywhere in our code. As we suggested above, DI frameworks can be seen as a replacement for `new`, so that might seem a logical approach. 
	
Using our DI framework in such a way is known as the Service Locator pattern, but it is generally viewed as an anti-pattern. You want to try to limit directly using the DI framework. If you are using a modern framework, there should be hooks where the framework stops and your code starts to make this possible. For example, ASP.NET MVC 1 and 2 allows you to provide a custom controller factory which can then be used as a starting point for the dependency resolution process (version 3 has even simpler to use hooks). In fact, most DI frameworks will provide these hooks for various frameworks, such as Ninject's `NinjectHttpApplication` which you simply inherit from, tell it about your modules, and move on. As a simple check, search your solution for how many times you are importing your DI's namespace (if you are using Ninject, you could search for `using Ninject`), hopefully you won't find it in more than 4 or 5 places.

The point is that with static languages, DI should be more of a configuration/setup exercise than a coding one. You might end up calling it directly at the lowest levels, but the automatic resolution should flow from there. If you are using a framework that doesn't make this possible in all but a few cases, use a different framework.

### In This Chapter ###
In this chapter we looked at what Inversion of Control is as well as the problem we are trying to solve with it (reducing coupling). We also saw a common IoC pattern, Dependency Injection. For a lot of developers this is a different way of programming and thinking. Remember though that we do gain a lot from it: code is easier to change and refactor, classes with poor cohesion are easier to spot and tests are easier to write. 
