---
layout: post
title: "Effective Management of Graphical Documentation with Version Control"
date:   2023-08-02 14:40:30 -0400
categories: testing, documentation 
tags: testing, documentation, uml, plantuml
---
![](/images/tn_component-diagram-test-framework.jpg)
Danny Briskin, Quality Engineering Practice Manager


# The issue
Maintaining comprehensive documentation in a project is a fundamental concept. For more insights, refer to this [article](https://danny-briskin.github.io/documentation/2021/08/05/project-documentation.html). However, the majority of documents become difficult to manage over time. As the project progresses, outdated documents must be discarded and replaced with new versions. The primary issue lies in the diversity of document formats. From the worst-case scenario of password-protected PDFs to various MS Word formats and even Markdown pages, managing versioning becomes either unviable or severely limited for such documents, particularly for graphical elements.

# Implementing Version Control
Being software developers, we are well aware of version control, specifically Git. A clever approach is to leverage Git for managing documents as well. Surprisingly, even binary formats like PDFs and Word documents can be tracked using Git-like versioning. By adopting Markdown for documentation, one can fully exploit Git's functionality, including branching, merging, and history with changes attributed to specific usernames. This solution can be implemented by maintaining these documents in a separate Git repository, preferably, or even within the codebase if the number of documents is manageable.

# The Role of Visuals
Text often falls short in conveying complex information, and thus, we frequently rely on images to provide additional clarity to our content. Technical diagrams are an integral part of project documentation and must be effectively stored and versioned.

# Pictures as Text
One viable solution is utilizing the [SVG](https://en.wikipedia.org/wiki/SVG) format, which offers a text-based (XML) structure, allowing for easy depiction of various images. However, most project documentation comprises technical diagrams like UML diagrams, and switching to SVG might not be the most practical approach. Another solution is to use [LaTeX](https://en.wikipedia.org/wiki/LaTeX) for document creation, but the learning curve and tools needed for the solution is an overkill for simple diagrams. Instead, we recommend using PlantUML, a simple tool that generates UML diagrams from formatted text documents. PlantUML utilizes SVG conversion in the background, ensuring seamless integration without additional complexities.

# Introducing PlantUML
[PlantUML](https://plantuml.com/) is a powerful tool that enables the creation of various UML diagrams along with non-UML diagrams, offering the flexibility to depict virtually anything effortlessly. From Component diagrams with JSON to Gantt diagrams and MindMaps, PlantUML proves to be a versatile solution with a minimal learning curve.
They use SVG conversion in background, so there is no extra magic.

# Example: Test Framework Component Diagram
Let's take the example of creating a component diagram for a test automation framework. You can access the complete file [here](/files/test_framework.puml).

We can define several components within a package as follows:
{% highlight xml %}
package "Virtual Machine environment" {
  component [Jenkins CI] as jenkins
  component [Java Development Kit (ver. 17)] as java17
  component [Email Server] as local_email
  database "Redis in-memory DB" as redis
}
{% endhighlight %}
We can also create packages inside other packages, and define connections between components:
{% highlight xml %}
package "Test Automation Framework" {
  component [Cucumber] as cucumber
  package "Page objects" {
      component [Page Object 1]
{% endhighlight %}

This allows us to define bilateral and unilateral connections between components and packages, resulting in a comprehensive component diagram for the test automation framework.
{% highlight xml %}
  junit <-> cucumber
  junit <-> spring

  "Test Automation Framework" -d.> "System under test":tests
{% endhighlight %}
Look what we have as a result:
![](/images/component-diagram-test-framework.jpg)

#Example: Usecase Diagram
Another application of PlantUML is in creating usecase diagrams, such as the invoicing process diagram. You can download the complete file [here](/files/invoice_processing.puml) .

PlantUML allows the definition of complex elements, like loops and comments, to provide detailed context within the diagram.

Here's an excerpt from the usecase diagram:
{% highlight xml %}
group Initialization. Cleaning possible errors.
:Try to run **faTransactionServiceTrial** request;
if (faTransactionServiceTrial failed?) then (<color:red>yes)
  :<color:red>Delete</color> **P** status records from rdr_sxm_feed;
else (<color:green>no)
endif
end group
{% endhighlight %}
A loop and a comment
{% highlight xml %}
while (For ID in Invoice file)
  :Send **RDR** request with respect of 29th day;
  note right
    If Today is less 29th of the month
    RDR will be sent with Today's date,
    otherwise - it will be 1st day of the **next** month
    ====
    see <b>Utilities#getNon29thDate()</b>
  end note
endwhile
{% endhighlight %}

Here is the result
![](/images/use-case-diagram-invoicing-process.jpg)


# Summary
By embracing PlantUML and incorporating version control practices, we can effectively manage graphical documentation, ensuring a clear and streamlined approach to project documentation.