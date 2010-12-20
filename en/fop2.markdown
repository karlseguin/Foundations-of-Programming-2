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

## Chapter 1 - Quality and Efficiency ##
>"Quality is never an accident; it is always the result of intelligent effort." - John Ruskin

Pragmatically, the reason I, and I assume most programmers, are keen to learn is to improve the quality of the code we write as well as our efficiency. I don't know any programmer who spends time learning new approaches and technologies with the explicit goal of being able to write worse code, or find ways to slow his or her pace. How do we measure efficiency and quality though? What, as creators, do we consider to be our goals?

Management-type might solely align quality with client/user satisfaction. While that's certainly important, good programmers are almost always the stakeholders with the highest quality expectation as well as being the most critical. I find it difficult to put quality, from a programmers perspective, into words; yet, I'm sure most of us not only have a similar definition, we can all easily recognize it (or the lack of it). Quality is about coming up with an elegant solution to a problem - both at the micro (an individual behavior) and macro levels (the system as a whole). What's *elegant*, then? It varies on the context of the problem, but generally, an elegant solution is both the simplest possible one as well as the most explicit and easy to understand.

The more I've programmed, the more I've noticed something truly startling. Not only is the simplest solution obviously the easiest to implement, its almost certainly the easiest to understand, maintain and change. Simple solutions also tend to have better performance characteristics than more complex approaches. This is particularly astounding when you find yourself knee deep in an over-engineered mess where the original intention for flexibility, performance and efficiency are sabotaged by artificial complexity. That doesn't mean I advocate, what is amusingly termed, duct-tape programming (though I am more forgiving of it since duct tape programmers just don't know any better, whereas architecture astronauts fail precisely for the opposite reason, they think they know better). How then do we know whether a given solution is simple enough? How do we avoid being duct tape programmers while not turning into architecture astronauts?

I wish I could tell you the answer. It isn't that I don't know. The problem is that I have to preface it with a word of warning - else I risk having you roll your eyes and give up on this book. I know a lot of you are sick of hearing about it, but I beg that you hear me out and recognize that I'm probably not saying what you think I'm saying. Deal? So the secret to writing quality code is to make sure your code is testable. WAIT...don't roll those eyes, I'm not saying that you have to test your code (and I'm certainly not saying that you shouldn't), I'm just saying that if, theoretically, you were to test your code, it shouldn't be hard to do. Code can be testable without actually testing it - and ultimately, at the most fundamental level, that's what you should be aiming for.

Much of this book is going to be devoted to testability. However, we largely do so as a measure of quality. We'll certainly cover the other benefits of writing tests, but do not mix testability as a quality metrics with writing tests. Its a lot like writing maintainable code that doesn't really require maintenance - its a goal, not an actual activity.  Now its true that the best way to discover whether a solution is testable is to actually write tests. I do hope that I can convince you to give it a try; in the last three years, becoming an effective test writer has been one of the two things which has most helped me elevate my craft. Nonetheless, its entirely possible to look at a method, or a system, and gauge testability, and thus quality, without actually writing a test.

For a long time I thought the solution really was that simple - write tests, and the quality of your code will greatly improve. More recently though I've realized that not all tests are created equally and its entirely possible, if not common, to have really poor tests. Understanding what makes a good test, and thus what makes code testable, comes with experience, and hopefully, through future chapters, I can help you avoid some of the pitfalls I ran into.

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
(If this was a short-lived/throwaway app, we might forgive it. Although I must say that in my experience, skilled programmers write clean and thoughtful code regardless of the expected life expectancy of our code. This is likely due to the fact that we all know *prototype* code can end up having an embarrassingly long shelf-life.)
 
This snippet is far from perfect. If we specifically look at it from a testing point of view, we'll see a number of dependencies which'll make this hard to test, such as `SqlServerDataStore.FindUser` and `Response.Redirect`, to name a few. Sadly, many of those are baked into the framework - which is a one of the reasons many .NET developers feel so strongly against WebForms. In this particular case, the near impossibility of testing tells me that the code will be difficult to change and maintain.

A lot of the best practices you hear thrown around, like YAGNI (you aren't going to need it), low coupling and high cohesion, or SOLID, can be measured by looking at your code and thinking of how you'd test it. A method that does too much, potentially violating both YAGNI and high cohesion, will require a painful amount of setup code to test (as well as prove easy to break).

### Efficiency: Not As Simple As It Seems ###
I might not have sold you on the testability as a quality metric (yet), but I'm confident you agree that quality is a supremely important aspect. Our other pragmatic-driven goal is efficiency, because great code can't wait forever. You'd hope that efficiency, with respect to learning and improving, would be easy to measure: can I achieve XYZ now faster than I could before? This isn't the case. 

First, and most dangerously, programmers don't always realize that a better way might exist. Let's be honest, a lot of programmers refuse to accept that very possibility. Its hard for people, especially those not passionate about what they do, to accept that a better way exists - it not only threatens their comfort zone, but potentially their careers. I was in school when Java started gaining popularity and was impressed by the close-mindedness of programmers who refused to accept that automatic garbage collection might, after working out some quirks, be a useful (let alone standard) general purpose solution. I find it particularly ironic now when Java developers refuse to accept that that dynamic languages might become the new Java (especially since they often raise the same concerns which C++ developers once had about Java).

Second, regardless of how much more efficient a new technology or approach is, you'll be more efficient in what you know versus what you're learning. My strategy to surmount this is to take my time and recognize the process as a long term investment. Also if you are clever about it, you can probably fit some R&D into your day to day work. By clever, I don't mean deceitful. I mean finding secondary, none critical systems. That monitoring system you've talked about with colleagues for a while but never have the time to do, or that prototype you've been asked to write. The exact approach you take will be personalized to your preferred way to learn. All I can say is that you shouldn't expect an increase in efficiency over night.

### In This Chapter ###
That was a lot of text with too little code. This chapter was the groundwork for following chapters, which rely heavily on code and examples. We defined what quality means to us as well as the metric we'll use to measure it. We looked at efficiency and discovered that while it might seem easy to gauge, it really isn't. Take a break and consider some of the code you've recently written. If you did test it what could have made your tests simpler and less likely to fail as your system changes? If you didn't, can you think of any pain points you might have run into if you decided to test it now?