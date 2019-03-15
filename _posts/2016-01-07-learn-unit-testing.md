---
title: Learn Unit Testing
comments: true
---

> Even you can code doesn't mean you are an engineer.

It is true. Some expert said it a loud and then slap my face. Software Engineering isn't just about the code itself, but including unit testing, source control, and a lot other things.

## Unit Testing

... and it is better to late than never. Now i decide give unit testing a try. Yes, maybe i am a coder, but not an engineer. Until now, i am still do unit testing manually. 

But later, i feel, more complex our program, we need more a lot time to test our program. As my main programming language is C# and my IDE is Visual Studio, i start to learn Unit Test.

First, i create a Class Library Project via menu File > Create Project. Then create a simple class, just called it Human (or Hooman).

{% highlight csharp %}
public class Human
{
    public int Energy { get; private set; }
    public bool Tired { get; private set; }

    public void Eat(int calory)
    {
        Energy += calory;
    }

    public void Work(int calory)
    {
        Energy -= calory;

        if (Energy < 0)
        {
            Energy = 0;
            Tired = true;
        }
    }

    public void Rest()
    {
        Tired = false;
    }
}
{% endhighlight %}

Only that ? Of course yes. Sorry for my laziness lol. Next i create Unit Test Project (right click Solution Explorer > Add > New Project), and then create test class. 

There are several conditions to be tested. Like Work exceeds the remaining energy and much more.

{% highlight csharp %}
[TestClass]
public class HumanClassTest
{
    [TestMethod]
    public void TooMuchWorkAndBecomeTired()
    {
        var human = new Human();
        human.Eat(100);

        human.Work(200);

        Assert.AreEqual(true, human.Tired);
        Assert.AreEqual(0, human.Energy);
    }

    [TestMethod]
    public void EatSomethingAndEnergyIncreased()
    {
        var human = new Human();
        var calory = 100;
        var beginEnergy = human.Energy;

        human.Eat(calory);

        var endEnergy = human.Energy;

        Assert.AreEqual(calory, endEnergy - beginEnergy);
    }

    [TestMethod]
    public void AfterRestNotTiredAgain()
    {
        var human = new Human();
        human.Work(100);

        Assert.AreEqual(true, human.Tired);

        human.Rest();

        Assert.AreEqual(false, human.Tired);
    } 
}
{% endhighlight %}

I open test explorer via menu Test > Windows > Test Explorer. I look all test method available there, and run all. Magically all test passed.

![test_unit_example.JPG]({{site.baseurl}}/assets/images/test_unit_example.JPG)

Well, that is my simple try to learn Unit Testing in Visual Studio with C# Programming Language. I Hope in the future i always use it for my upcoming projects. Cheers!
