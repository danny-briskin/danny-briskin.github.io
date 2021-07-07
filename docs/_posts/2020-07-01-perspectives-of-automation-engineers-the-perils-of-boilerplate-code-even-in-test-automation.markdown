---
layout: post
title: "Perspectives of Automation Engineers: The Perils of Boilerplate code, even in test automation"
date:   2020-07-01 14:40:30 -0400
categories: java
tags: java lombok
---
![](/docs/images/java_code_top.webp)
Danny Briskin, QA Consultants Senior Automation Engineer
July 2020

Today, QA has become more and more reliant on automation. Sure, it saves time and increases coverage, but it also requires specific knowledge within test automation approaches, languages, and tools. Programming languages like Java are the most common choice to create test automation frameworks today. However, as automation frameworks and their test suites grow, more and more lines of code are required, and it may become cumbersome to maintain.  Without a focus on reducing code complexity and efficiency, automated test developers face the risk of perpetuating the same issues we find in code under test, but in our test automation code.   

Let’s discuss ways to decrease the number of lines without losing application functionality.  

This blog assumes the reader has a working knowledge and experience in java programming. 

When it comes to coding, some programming languages like C, C++, and Java require every little detail be fussed over, scrutinized, and reviewed with a fine-tooth comb. Learning to code with these intricate nuances becomes a craft in itself. This may require a coder to write a lot of sections of code during software development that becomes computer programming boilerplate code.  

According to computer science, the syntax of coding is a language the computer can understand and a set of rules that defines the combinations of symbols which are considered to be a correctly structured document or fragment in that language. 

Once you’ve mastered computer coding, and come to realize the headaches that come with it, you have to spend a lot of time creating absolutely obvious, repetitive, similar lines of code just because it is required. The process is painful even if you use helper functions of your IDE. 

There are other languages like Python and JavaScript that allow you to code less, removing that burden. Once you’ve evaluated and reviewed the code and everything is satisfactory, you can concentrate on programming business logic mode. 

Except, one never really knows what’s going on under the hood. It is quite possible that built-in routines work efficiently, but maybe they don’t? Moreover, when your application grows, and you would like to make it more flexible, language limitations limit that fluidity.

# That begs the question: What is the right choice?

If implementation speed is key to the project, then, the resulting application is relatively small and is not supposed to be maintained nor scaled in the future. In this case, the answer is to write the code in a simple manner. 

When it comes down to coding there are two schools of thought: developer-friendly vs hands-on languages. What does this mean? For example, in Python one command is executed and in the background, several commands run at once. While in Java, to achieve the same result, each command must be run independently. What it comes down to, is do you enjoy driving an automatic or the hands-on approach of a standard? 

Since some programming languages can execute so many commands by default, a developer is not able to fully control the code. Instead, one has to rely on the language with the hope that everything is working well and optimized. However, if you want to create a flexible, reliable, scalable and maintainable software it is better to tinker under the hood and get to the root of each line of code. 

There are a couple of ways to do so even in programming languages that were created 25 years ago when such issues didn’t exist. 

The beauty of Java language is in its extreme flexibility. Even if you cannot find an appropriate tool in the language to do the task, you can create it yourself. Yes, the Java language allows you to modify some aspects of language itself! 

This article (and the series to follow) will cover the topic “How to shorten your code and, at the same time, not to lose control”. 

# What is boilerplate code?

What is “boilerplate code”? Here is an example of Java code: 
{% highlight java %}
public class Customer {
	    private Integer id;
	    private String customerName;
	    private Double customerBalance;
	    private LocalDateTime customerActivated;
}
{% endhighlight %}
It is a POJO (Plain Old Java Object) like class to make a model of a Customer in our application. It looks clean and readable: any customer has an ID, name, balance, and date of activation. In addition, we can add customer-related methods like: 
{% highlight java %}
public boolean isPrivilegedCustomer(Double requiredBalance) {
    if (this.customerActivated == null) {
        logger.warn("Customer was not activated");
        return false;
    }
    if (requiredBalance == null) {
        logger.warn("Required Balance is null");
        return false;
    }        
    return this.customerBalance >= requiredBalance &&
    this.customerActivated.isBefore(LocalDateTime.now().minusYears(10));
}
{% endhighlight %}
However, according to Java standards and design patterns, we need to add several things there to make it work properly. These additions include a constructor, getters and setters, toString, equals, and hashCode methods. 

In this instance, if a proper logging is required, then that means a logger needs to be added. And the simple and straightforward code becomes an ugly mess that may not be necessary. 
{% highlight java %}
public class Customer {
    private static final Logger logger = LogManager.getLogger(Customer.class);
    private Integer id;
    private String customerName;
    private Double customerBalance;
    private LocalDateTime customerActivated;

    public Customer(Integer id, String customerName, Double customerBalance, LocalDateTime customerActivated) {
        this.id = id;
        this.customerName = customerName;
        this.customerBalance = customerBalance;
        this.customerActivated = customerActivated;
    }
    public Customer() {
    }

public boolean isPrivilegedCustomer(Double requiredBalance) {
    if (this.customerActivated == null) {
        logger.warn("Customer was not activated");
        return false;
    }
    if (requiredBalance == null) {
        logger.warn("Required Balance is null");
        return false;
    }        
    return this.customerBalance >= requiredBalance &&
 this.customerActivated.isBefore(LocalDateTime.now().minusYears(10));
}
    public Integer getId() {
        return id;
    }
    public void setId(Integer id) {
        this.id = id;
    }
    public String getCustomerName() {
        return customerName;
    }
    public void setCustomerName(String customerName) {
        this.customerName = customerName;
    }
    public Double getCustomerBalance() {
        return customerBalance;
    }
    public void setCustomerBalance(Double customerBalance) {
        this.customerBalance = customerBalance;
    }
    public LocalDateTime getCustomerActivated() {
        return customerActivated;
    }
    public void setCustomerActivated(LocalDateTime customerActivated) {
        this.customerActivated = customerActivated;
    }
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Customer customer = (Customer) o;
        return Objects.equals(id, customer.id) &&
                Objects.equals(customerName, customer.customerName) &&
                Objects.equals(customerBalance, customer.customerBalance) &&
                Objects.equals(customerActivated, customer.customerActivated);
    }
    @Override
    public int hashCode() {
        return Objects.hash(id, customerName, customerBalance, customerActivated);
    }
    @Override
    public String toString() {
        return "Customer{" +
                "id=" + id +
                ", customerName='" + customerName + '\'' +
                ", customerBalance=" + customerBalance +
                ", customerActivated=" + customerActivated +
                '}';
    }
}
{% endhighlight %}
# Project Lombok

First, let’s look at existing solutions. One such helpful tool is Project Lombok; it’s a framework that can greatly decrease the size of your code. The main idea of Lombok is in its usage of annotations. 

You can annotate almost anything in Java and Lombok will generate all of the required code during compilation. This means you’ll never see that code in the source code. 

Let’s apply Lombok to the previous example: 
{% highlight java %}
@RequiredArgsConstructor
@NoArgsConstructor
@Getter
@Setter
@EqualsAndHashCode
@ToString
@Log4j2
public class Customer {
    private Integer id;
    private String customerName;
    private Double customerBalance;
    private LocalDateTime customerActivated;

public boolean isPrivilegedCustomer(Double requiredBalance) {
    if (this.customerActivated == null) {
        logger.warn("Customer was not activated");
        return false;
    }
    if (requiredBalance == null) {
        logger.warn("Required Balance is null");
        return false;
    }        
    return this.customerBalance >= requiredBalance &&
 this.customerActivated.isBefore(LocalDateTime.now().minusYears(10));
}
}// yes, that's all
{% endhighlight %}
As a result, several annotations have been added to the class and voila! 

Lombok is a one-stop source for everything in regards to methods and constructors. In fact, it’s right there in the compiled class file, except you just don’t see it in source code. Nonetheless, you can use it during development thanks to the Intellij Lombok plugin. 

All annotations names are self-explanatory. You can see a full list of available annotations on Lombok website. 

For example, let’s facilitate solving a well-known Java development issue: Null Pointer Exception. With Lombok you can greatly ease the pain. 

Change this 
{% highlight java %}
public boolean isPrivilegedCustomer(Double requiredBalance) { 
    if (requiredBalance == null) { 
        logger.warn("Required Balance is null"); 
        return false;     
}
{% endhighlight %}
to
{% highlight java %}
public boolean isPrivilegedCustomer(@NonNull Double requiredBalance){ 
<...skipped...>
{% endhighlight %}
and Lombok, with the help of [@NonNull](https://projectlombok.org/features/NonNull) annotation, will do the rest for you.

Need a logging? Not a problem. Just add an [@Log4j](https://projectlombok.org/features/log) annotation to your class and don’t forget to [configure](https://logging.apache.org/log4j/2.x/manual/configuration.html) your Log4j.

Now, you have a predefined log object. Use this as a logger:
{% highlight java %}
log.warn("Customer was not activated");
{% endhighlight %}
Some frequently used annotations can be replaced with one annotation that covers all of them. For example, [@Data](https://projectlombok.org/features/Data) or [@Value](https://projectlombok.org/features/Value) are just containers of other annotations. And now, the code becomes even shorter than the initial one!
{% highlight java %}
@NoArgsConstructor 
@Data 
@Log4j2 
public class Customer { 
    private Integer id; 
    private String customerName; 
    private Double customerBalance; 
    private LocalDateTime customerActivated; 
    public boolean isPrivilegedCustomer(@NonNull Double requiredBalance) { 
        if (this.customerActivated == null) { 
            log.warn("Customer was not activated"); 
            return false; 
        } 
        return this.customerBalance >= requiredBalance && 
    this.customerActivated.isBefore(LocalDateTime.now().minusYears(10)); 
    } 
} // yes, that's all
{% endhighlight %}
Do you want to use a design pattern [Builder](https://dzone.com/articles/creational-design-patterns-builder-pattern) but think that it requires too much code? Use this easy solution instead:
{% highlight java %}
@Builder 
public class Customer { 
    private Integer id; 
<...> 

@Log4j2 
public class Starter { 
    public static void main(String[] args) { 
        Customer customer = Customer.builder() 
                .customerName("John") 
                .customerBalance(123.45) 
                .build(); 
        log.debug(customer); 
   }
}
{% endhighlight %}
Don’t like the same type of exception handling used everywhere?
{% highlight java %}
@Builder 
public class Customer { 
<...> 
    public boolean isPrivilegedCustomer(@NonNull Double requiredBalance) throws CustomerException { 
        if (this.customerActivated == null) { 
           throw new CustomerException("Customer was not activated"); 
        } 
        return this.customerBalance >= requiredBalance && 
                this.customerActivated.isBefore(LocalDateTime.now().minusYears(10)); 
    } 
} 

<...> 

public void metodOne(Customer customer) { 
    try { 
        boolean isPrivilegedCustomer = customer.isPrivilegedCustomer(100.45); 
        log.debug(isPrivilegedCustomer); 
    } catch (CustomerException e) { 
        log.catching(e); 
    } 
}
{% endhighlight %}
Just look at that:
{% highlight java %}
@SneakyThrows(CustomerException.class) 
public void metodTwo(Customer customer) { 
    boolean isPrivilegedCustomer = customer.isPrivilegedCustomer(100.45); 
    log.debug(isPrivilegedCustomer); 
}
{% endhighlight %}
# Summary

Project Lombok is a very powerful tool for any developer. A broad set of useful annotations will eliminate a great amount of boilerplate code from your Java application. A few words can replace hundreds of lines of code. As a result your Java classes become clean, concise, and easy to maintain. 

Project Lombok can do a lot of things but not everything you might need to get rid of boilerplate code. 

Another option to automatically inject portions of code to designated places is the famous Aspect Oriented Programming approach. But it is a topic of the next article. 
