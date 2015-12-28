---
layout: post
title: "Stand the test of time"
date: 2013-08-01 20:39
comments: true
categories: coding testing java
---
Sooner or later you will get to the point that have to test time base conditions in your application.

Look at the following example:

Suppose you are developing an accounting system. Invoices are created in billing cycles which are allowed to run
at the beginning of every month.

{% codeblock lang:java %}
import org.joda.time.*;

import java.util.Arrays;
import java.util.Date;
import java.util.List;

public class BillingCycle {

    public static final List<Integer> BILLING_CYCLE_DAYS = Arrays.asList(1, 2, 3);

    public boolean isAllowedToRun() {
        // a lot of complex accounting logic omitted...

        DateTime today = new DateTime();
        return BILLING_CYCLE_DAYS.contains(today.getDayOfMonth());
    }
}
{% endcodeblock %}


{% codeblock lang:java %}

import static org.hamcrest.CoreMatchers.is;
import org.joda.time.DateTime;
import static org.junit.Assert.assertThat;
import org.junit.Before;
import org.junit.Test;

public class BillingCycleTest {

    private BillingCycle billingCycle;

    @Before
    public void setUp(){
        billingCycle = new BillingCycle();
    }

    // tests are fragile and not working since you can´t run them on an explicit day...

    @Test
    public void isAllowedToRun_onTheFirstOfEveryMonth_returnsTrue(){
        assertThat(billingCycle.isAllowedToRun(), is(true));
    }

    @Test
    public void isActivated_onTheFourthOfEveryMonth_returnsFalse(){
        assertThat(billingCycle.isAllowedToRun(), is(false));
    }

}

{% endcodeblock %}

How to test this method? Since <code>new DateTime()</code> is not accesible you have no chance to test the true/false
cases in an isolated, consistent way. You have a dependency to time and it is a moving window.

The easiest way would be to pass the current date to the method or to inject it from the test in the
billing cycle.

{% codeblock lang:java %}

public class BillingCycle {

    public static final List<Integer> BILLING_CYCLE_DAYS = Arrays.asList(1, 2, 3);
    private DateTime now;

    public BillingCycle(){
        // create a 'now' here
        now = new DateTime();
    }

    public boolean isAllowedToRun() {
        // lot complex accounting logic omitted...

        return BILLING_CYCLE_DAYS.contains(now.getDayOfMonth());
    }

    // inject another date from test here, or pass it to isAllowedToRun
    void setNow(DateTime now){
        this.now = now;
    }
}

{% endcodeblock %}

{% codeblock lang:java %}

@Test
public void isAllowedToRun_onTheFirstOfEveryMonth_returnsTrue(){
    billingCycle.setNow(DateTime.parse("2013-11-01"));
    assertThat(billingCycle.isAllowedToRun(), is(true));
}

@Test
public void isActivated_onTheFourthOfEveryMonth_returnsFalse(){
    billingCycle.setNow(DateTime.parse("2013-11-04"));
    assertThat(billingCycle.isAllowedToRun(), is(false));
}

{% endcodeblock %}

Sligthly better, but both approaches have a downsides: if you create the date in the constructor, you bind the
now to the creation of your object which could be a source of hard to find bugs especially if you
have long living objects.

If you pass your current date to the method you don´t have that problem, but
you pollute your interface with quite specific information about what your method does.

An another approach could be to create a subclass from billing cycle, extract <code>new DateTime()</code> into a method and 
override it a test specific subclass.

{% codeblock lang:java %}

public class BillingCycle {

    public static final List<Integer> BILLING_CYCLE_DAYS = Arrays.asList(1, 2, 3);
    
    public boolean isAllowedToRun() {
        // lot complex accounting logic omitted...

        return BILLING_CYCLE_DAYS.contains(getNow().getDayOfMonth());
    }

    protected DateTime getNow() {
        return new DateTime();
    }
}
{% endcodeblock %}

{% codeblock lang:java %}

import org.joda.time.DateTime;

public class TestBillingCycle extends BillingCycle {

    private final DateTime now;

    public TestBillingCycle(DateTime now){
        this.now = now;
    }
    
    @Override
    protected DateTime getNow() {
        return now;
    }
}
{% endcodeblock %}

Now you can use the <code>TestBillingCycle</code> instead of <code>BillingCycle</code> in your unit tests and use a specific
date for each test case.

Seems to be straight forward, but as the system grows you will get many classes where you have to
handle issues. At some point it is important to travel with the whole system into a different time and
that is where subclassing has its shortcomings.

Java has no open classes, so simply mocking the genereal time mechanism like in [timecop][1] for ruby
is not possible. With powermock you can mock static methods and therefore <code>System.currentTimeMillis()</code> but
I don´t think that this a good solution since you can possibly break unrelated tests. And tests should be independent!

I prefer composition over inheritence, so an alternative approach could be:


{% codeblock lang:java %}

public class BillingCycle {

    public static final List<Integer> BILLING_CYCLE_DAYS = Arrays.asList(1, 2, 3);

    // dependency to time is modeled explicitly
    private Clock clock = EnterpriseClock.create();

    public BillingCycle(){}

    // you can inject a different clock since it is an interface
    public BillingCycle(Clock clock) {
        this.clock = clock;
    }

    public boolean isAllowedToRun() {
        // lot complex accounting logic omitted...

        return BILLING_CYCLE_DAYS.contains(clock.getNow().getDayOfMonth());
    }
}
{% endcodeblock %}

{% codeblock lang:java %}

public interface Clock {

    public DateTime getNow();

}
{% endcodeblock %}

{% codeblock lang:java %}

// use the FrozenClock inside your tests where you need a fixed date for 
// consistent test results
public class FrozenClock implements Clock{

    private DateTime now = new DateTime();

    public void adjustTo(DateTime now){
        this.now = now;
    }

    @Override
    public DateTime getNow() {
        return now;
    }
}
{% endcodeblock %}

{% codeblock lang:java %}

public class BillingCycleTest {

    private BillingCycle billingCycle;

    FrozenClock frozenClock;

    @Before
    public void setUp(){
        frozenClock = new FrozenClock();
        billingCycle = new BillingCycle(frozenClock);
    }

    @Test
    public void isAllowedToRun_onTheFirstOfEveryMonth_returnsTrue(){
        frozenClock.adjustTo(DateTime.parse("2013-11-01"));
        assertThat(billingCycle.isAllowedToRun(), is(true));
    }

    @Test
    public void isActivated_onTheFourthOfEveryMonth_returnsFalse(){
        frozenClock.adjustTo(DateTime.parse("2013-11-04"));
        assertThat(billingCycle.isAllowedToRun(), is(false));
    }

}
{% endcodeblock %}

With an object representing the time you got the full power of an OO design.  Even if you are in the enterprise world, you can simply inject a 
"ClockService" from your test context into your services, repositories, components, mapper or however.

And yes, you can use a plain old mockito mock instead of a FrozenClock.

Personally I like this solution since it models the dependency to time in an elegant way and it helps to test
time specific code.


[1]: https://github.com/travisjeffery/timecop
[2]: https://code.google.com/p/powermock/wiki/MockitoUsage
