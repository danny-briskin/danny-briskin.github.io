---
layout: post
title: "How to get rid of boilerplate code. Adding more flexibility with AOP"
date:   2020-08-01 14:40:30 -0400
categories: java
tags: java lombok AOP
---
![](/images/aspectj.webp)
Danny Briskin, QA Consultants Senior Automation Engineer
August 2020

# The issue

[Project Lombok](https://projectlombok.org/) has its advantages ([see previous article](https://danny-briskin.github.io/java/2020/07/01/perspectives-of-automation-engineers-the-perils-of-boilerplate-code-even-in-test-automation.html)) but the drawbacks are present there too. The main one: you are limited to already created Lombok functionality.

When you need more flexibility, you may take a look to Aspect Oriented Programming (AOP)

The main idea of AOP is in usage of so-called aspects. Aspects are parts of code that can be automatically inserted (it is called “weaving“) into another part of code during compilation or even during runtime. You can define various rules of weaving aspects depend on your business logic

Aspects themselves do not do any specific business logic (like methods), moreover, there is no sense in using aspects standalone. They can be only weaved into another part of code (like a patch).

Thus, we can move our boilerplate code to aspects and it will be weaved automatically in our application.

Application areas of AOP are:

* Logging
* Data validation
* Authentication
* Performance measuring

We will use a popular library AspectJ to create and use aspects in our Java code.
Useful links
* See [this article](https://danny-briskin.github.io/java/2020/08/01/setup-aspectj.html) to setup AspectJ in your project.
* You can find full Maven project code [here](https://github.com/danny-briskin/aopArticle) (non Maven project code is [here](https://github.com/danny-briskin/aopArticleNonMaven.git))

# Definitions

Aspect
: A modularization of a concern that cuts across multiple objects. Each aspect focuses on a specific crosscutting functionality. A good example of aspect is logging. You need the same logging functionality everywhere in the app.
Join point
: A point during the execution of a script, such as the execution of a method or property access. A place where advice code will be inserted. It can be

* Before method is executed
* After method was executed
* After method was executed and had returned a value
* Around method (i.e. before + after join point together). You can wrap method execution in your code or even replace the execution with yours
* After exception was thrown

Advice
: Action taken by an aspect at a particular join point. It is a method to weave(execute) when joint point is met. For example: let’s do logging of method name and actual parameters values before a method is executed

Pointcut
: A regular expression that matches join points. An advice is associated with a pointcut expression and runs at any join point that matches the pointcut. You can think about it as a condition where to apply (weave) a certain part of code. For example:

* “apply it to all methods in all classes of a package”
* “all methods annotated with @AnnnotaionOne”
* “class methods with 2 parameters when first one is String”
    “all methods in packageOne or packageTwo except toString() method or annotated with @AnnnotaionOne”

# Creation of an aspect

We will create an aspect to weave logging functionality to methods of our application

1. Let’s create a class with @Aspect annotation 
{% highlight java %}
@Aspect public class LogAspect {
{% endhighlight %}
Assuming you have configured [Log4j](https://logging.apache.org/log4j/2.x/manual/configuration.html), add a logger to use:
{% highlight java %}
public static final Logger LOGGER = LogManager.getLogger("com.qaconsultants.core");
private static final Level LOGGING_LEVEL = Level.DEBUG;
{% endhighlight %}
2. Define a pointcut (see pointcut definition syntax [here](https://www.baeldung.com/spring-aop-pointcut-tutorial#pointcut)) 
{% highlight java %}
@Pointcut("execution(* org.example.core..*(..))")
public void pointcutOne() {
    // it is a pointcut
}     
{% endhighlight %}

The code in @Pointcut annotation means: run weaved code when a method is executed. Find all methods with any visibility (first asterisk) of *org.example.core package*, in any of package class (first two dots ..), any method name (second asterisk), any number and type of method parameters (second two dots).
**Important!**
Don’t place Aspect class in the same package(s) where you want aspects’ code to be weawed. It will lead to stack overflow exception (because methods will call itself to be weawed to itself and so on… )

<details>
  <summary>Expand to see other examples of pointcuts</summary> 
Another (more complicated) pointcut
{% highlight java %}
  @Pointcut("execution(* org.example.core..*(..)) && ! execution(* *.toBuilder(..))")
public void pointcutExecutionFramework() {
    // it is a pointcut
}         
{% endhighlight %}
 This pointcut means almost the same as previous one except it will not be used when method with the name toBuilder with any parameters, in any package with any visibility is executed.

And another two pointcuts :
{% highlight java %}
  @Pointcut("execution(* *.toString(..)) || execution(* *.lambda$*(..))   || execution(* *.hashCode(..)) || @annotation(org.example.aspects.NoAopInsideMethod) ")
public void noNeedMethods() {
    // it is a pointcut
} 
{% endhighlight %}
 This one means: execution of specific methods, any lambda expression or methods annotated with @NoAopInsideMethod
{% highlight java %}
  @Pointcut("@annotation(org.example.aspects.NoAopLog)")
public void noLog() {
    // it is a pointcut
}   
{% endhighlight %}
… a method annotated with @NoAopLog annotation
{% highlight java %}
  @Pointcut("@annotation(org.example.aspects.ReplaceAop)")
public void replaceAop() {
    // it is a pointcut
} 
{% endhighlight %}
… a method annotated with @ReplaceAop annotation.
</details>
3. Let’s define an advice with join point
* Before join point 
{% highlight java %}
@Before("pointcutExecutionFramework() && ! noLog()")
public void beforeLog(JoinPoint joinPoint) { 
{% endhighlight %}
The code of this method (*beforeLog()*, an advice) will be executed before execution of a method where pointcut condition is met. I.e., before any method defined in *pointcutExecutionFramework* pointcut and not for those defined in *noLog()* pointcut.

The parameter **JoinPoint joinPoint** encapsulates all information needed from method where we weave the code (like method name and parameters values).

Let’s retrieve those values
{% highlight java %}
Signature signature = joinPoint.getSignature();
String methodName = signature.getDeclaringType().getSimpleName() + "." + signature.getName();
Object[] arguments = joinPoint.getArgs();
{% endhighlight %}
And do the logging
{% highlight java %}
LOGGER.log(LOGGING_LEVEL, () -> "[>>] " + methodName + "(" + Arrays.toString(arguments) + ")");
{% endhighlight %}
 This advice will log a method name with parameters right before the method execution
* After join point
Work with @After join point is the same as with @Before, the only difference is in place of weaving – it will in the end of method rather than beginning.
* AfterReturning join point
There is a little difference in using @AfterRuturning join point. Usually it is used for methods returning a value (i.e. not void return type). 
{% highlight java %}
@AfterReturning(value = "pointcutExecutionFramework() && ! noLog()", returning = "result")
public void afterReturningLog(JoinPoint joinPoint, Object result) { 
{% endhighlight %}
As you can see, it will weave exactly to the same methods as previous one but to the end of method. More specifically, it will be run after base methods is executed and returned a value.

You can see an additional parameter of @AfterReturning annotation: **returning**. The value of this parameter means the name of your advice method parameter: **result**.

In that parameter base method’s returned value will be placed. Knowing the signature of base method (retrieved from joinPoint parameter) and a little bit of “dark magic” of Java – Reflection API you can cast the value of Object type to its real type if you need to, or just convert it into String.
{% highlight java %}
public void afterReturningLog(JoinPoint joinPoint, Object result) {
    Signature signature = joinPoint.getSignature();
    Method method = ((MethodSignature) signature).getMethod();
    if (!method.getGenericReturnType().toString().equals("void")) {
        String methodReturnedResultAsString = result.toString();
        LOGGER.log(LOGGING_LEVEL, "[o<] [{}] <== {}::{}()"
                        , methodReturnedResultAsString
                        , signature.getDeclaringType().getSimpleName()
                        , signature.getName());} 
{% endhighlight %}
* Around join point
A combination of @Before and @After join points is @Around join point. 
{% highlight java %}
@Around(value = "replaceAop()")
public Object aroundReplace(ProceedingJoinPoint proceedingJoinPoint)  {
   LOGGER.log(LOGGING_LEVEL, "Before " + proceedingJoinPoint.getSignature().getName());
   Object result = proceedingJoinPoint.proceed(proceedingJoinPoint.getArgs());
   LOGGER.log(LOGGING_LEVEL, "After " + proceedingJoinPoint.getSignature().getName());
   return result;
}  
{% endhighlight %}
 You can see that method parameter type is ProceedingJoinPoint not just JoinPoint as always. The idea is the same, but ProceedingJoinPoint can execute a *proceed()* method that in fact is a method where this advice was weaved.

So, this method will do logging instead of actual method execution
{% highlight java %}
@Around(value = "replaceAop()")
public Object aroundReplace(ProceedingJoinPoint proceedingJoinPoint)  {
    LOGGER.log(LOGGING_LEVEL, "We start working instead of " + proceedingJoinPoint.getSignature().getName());
    return null;
}  
{% endhighlight %}
* AfterThrowing join point
And the last annotation I would like to mention, is @AfterThrowing. This one is used for manipulation of methods that throw exceptions. It looks like **@SneakyThrows** from [Lombok](https://projectlombok.org/features/SneakyThrows). The value of **throwing** parameter of the annotaion is the name of Throwable argument of the advice method. 
{% highlight java %}
@AfterThrowing(value = "pointcutExecutionFramework()", throwing = "e")
public void afterThrowingFramework(JoinPoint joinPoint, Throwable e) { 
{% endhighlight %}
Using already known JoinPoint object and throwable object you can create a concise logging inside the method. 

# Drawbacks

There are a few hidden discrepancies in using AspectJ library:
* Some versions are not compatible with popular frameworks (like Spring and Lombok. The one used here (1.9.5) is fully compatible
* It is not easy to setup proper amount of logging. It is either too much or too few. Deep knowledge of logging technique is required. In addition, you can create several annotations to disable/enable logging process and make your logging in advice methods dependent on it.
* Some objects are hard to log because their toString() method does not exist or does logging in inappropriate way. You need to know Java Reflection API well to be able to convert complex objects into String correctly.
* The main drawback of AOP is that program flow is obscured and a very fragile sometimes. One should consider using plugins to popular IDE to visualize weaving and make sure that all changes made will not break another part of application

# Summary

AOP is a very powerful and flexible tool to code less. On the other hand, the tool is much more low-level that Lombok. Knowledge in additional spheres is required in addition to Java core. In combination with Java Reflection API and Spring framework (will be discussed in next articles) one can create a reliable, automatically reusable, hidden (from business logic part) code instead of hundred times repeatable boilerplate code.
