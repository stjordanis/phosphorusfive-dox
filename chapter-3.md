# Ajax widgets

This chapter contains some fairly advanced study topics. If this is your first encounter with P5, you would probably benefit from skipping it for now, and rather move on the to [next chapter](chapter-4.md). Later, when you wish to dive deep into P5 - You can come back to this chapter, to acquire an understanding of all the gory details.

An Ajax widget is always defined as one single HTML element. Advanced extension widgets, which we will have a look at later, might have children widgets of themselves. But all the native main widgets, corresponds to a single HTML element on your page. There are 3 main widgets in P5.

- **[literal]** - Used to render simple text or HTML through its **[innerValue]** property
- **[container]** - Serves as a container for other widgets through its **[widgets]** collection
- **[void]** - Has no content, besides its attributes

By cleverly combining the **[void]**, **[literal]** and **[container]** widgets, you can create any HTML markup you wish. Contrary to other Ajax frameworks, you have 100% perfect control over your resulting HTML markup in P5.

## Widget attribues

When you create a widget, you often want to associate HTML attributes with it. This is easily done, by simply adding the attributes as a key/value argument to your widget. Consider the following code, which creates an HTML5 video element for you.

```
create-widget:my_video
  element:video
  width:320
  height:240
  controls
  src:http://www.w3schools.com/html/movie.ogg
  type:video/ogg
  innerValue:Your browser needs to be updated
```

The above Hyperlambda will result in something like the following for you.

![alt screenshot of hello world](screenshots/chapter-3-1.png)

In the above code, almost all arguments will create some sort of attribute. The resulting HTML will resemble something like the following.

```xml
<video 
  id="my_video"
  width="320"
  height="240"
  controls
  src="//www.w3schools.com/html/movie.ogg"
  type="video/ogg">
      Your browser needs to be updated
  </video>
```

All nodes you add to a widget, that does not have some special meaning, will automatically translate into an HTML attribute. P5 actually knows nothing of any *"video"* element, but the ability to dynamically declare any attributes you wish, combined with being able to control the HTML element used for rendering the widget, allows you to easily create any HTML you wish.

In fact, in the above code, the only arguments that are handled by P5 as *"non-attribute arguments"* are **[element]** and **[innerValue]**. All the other arguments are considered attributes to the resulting HTML. Notice the **[controls]** declaration above, that translates into an *"empty attribute"*. If you wish to create XHTML compliant HTML, you can exchange it with `controls:controls`.

### Widget Ajax events

You can associate Ajax events with your widgets the same way. P5 will determine if what you're creating is an attribute, or an Ajax event, depending upon its name. If your attribute starts out with the text *"on"*, it will be considered an Ajax event. When you create an Ajax event, the lambda object your Ajax event contains, will be evaluated on the server when the Ajax event is raised.

Notice, you are responsible yourselves for making sure your Ajax event is associated with a legal DOM event on the client side. You could create an Ajax event called **[onfoobar]**. However, this will result in non-conforming HTML, and serve no purpose for you. The same is true for attributes.

In our previous chapter, we created an **[onclick]** Ajax event using this syntax. Further down in this chapter, we will add a similar Ajax event to our video element.

You can also create JavaScript events with similar syntax. Although if you wish to create a piece of client-side JavaScript that is executed when your DOM event is raised - You will need to put your JavaScript into the _value_ of your event handler's node, instead of adding it as a child lambda object. The following is an example of that.

```
create-widget
  element:button
  innerValue:Click me
  onclick:"alert('Hello JavaScript!')"
```

### Widget arguments

Below are the special arguments you can apply to your widgets.

* **[visible]** - Controls a widget's visibility
* **[element]** - Declares which HTML element to render your widget with
* **[widgets]** - Declares its children widgets collection
* **[innerValue]** - Declares the widget's HTML content
* **[events]** - Associates custom *"widget lambda events"* with your widget
* **[parent]** - Parent widget to insert widget into
* **[position]** - Positions your widgets as n'th child of its parent's widgets collection
* **[before]** - Inject widget *"before"* specified widget
* **[after]** - Inject widget *"after"* specified widget
* **[oninit]** - A custom lambda object to execute when your widget is displayed

#### Positioning your widget

The **[parent]**, **[before]** and **[after]** arguments, are mutually exclusive, and you can only apply one of these. If you choose to use a **[parent]** argument, then your widget will be appended into this widget's children collection. If you use a **[parent]** argument, you can optionally apply a **[position]** argument, containing an integer number, declaring at which position you want your widget to be injected, instead of being appended at the end.

If you apply a **[before]** or **[after]** argument, you cannot apply a **[parent]**, or a **[position]** argument. You can also only use one of **[before]** of **[after]**. These arguments refers to an existing widget on your page, which you want to insert your widget *"before"* or *"after"*. You can insert your widget exactly where you wish, by cleverly using the right positioning argument(s).

Notice how we in the above Hyperlambda, actually completely omitted any positioning arguments. This results in that a default value of *"cnt"* as **[parent]** will be used. If no **[parent]**, **[before]** or **[after]** arguments are supplied, its default position becomes **[parent]** of *"cnt"*. The *"cnt"* container, is the main root widget on your page.

#### Which HTML element to render

The **[element]** declares which HTML element or *"tagName"* you want to use. There are no checks in P5 in regards to HTML validity. This allows you to create an HTML element with the name of *"foo-bar"*. Such an element, would obviously not conform to any HTML standards, and would be considered invalid HTML. You are responsible for making sure your code creates valid HTML markup. The **[element]** argument, if omitted, defaults to *"div"* for **[container]** widgets, *"p"* for **[literal]** widgets, and *"input"* for **[void]** widgets.

#### Controlling the visibility of your widget

The **[visible]** argument, is a boolean, and can take either *"true"* or *"false"*. This declares whether or not your widget should be initially visible.

If you create your widget as initially invisible, an invisible wrapper HTML element will be created for you, at the position you choose to insert your widget. This allows you to later make the widget become visible, automatically exchanging this invisible HTML markup, with your widget's actual content. When your widget becomes visible, it will be automatically rendered according to the properties you supplied during creation. This allows you to declare the widget in its entirety, yet still without actually rendering it on your page, until you're ready to display it.

Notice that your **[oninit]** lambda callback, will only be invoked when the widget becomes visible, and it will be invoked *every time* your widget becomes visible. The **[visible]** argument defaults to *"true"* if omitted.

#### Types of widgets you can create

The **[widgets]** and **[innerValue]** arguments are mutually exclusive, and only one of these can be supplied. If you supply a **[widgets]** argument, your widget will be created as a **[container]** widget. If you supply an **[innerValue]** argument, your widget will become a **[literal]** widget. If both of these are omitted, your widget will become a **[void]** widget.

You can also choose to explicitly choose the type of widget you create, by using one of the overload Active Events, instead of the generic one called **[create-widget]**. You can find these explicit overloads below.

- **[create-void-widget]** - Creates a void widget
- **[create-literal-widget]** - Creates a literal widget
- **[create-container-widget]** - Creates a container widget
- **[p5.web.widgets.create]** - Alias to **[create-widget]**
- **[p5.web.widgets.create-void]** - Alias to **[create-void-widget]**
- **[p5.web.widgets.create-literal]** - Alias to **[create-literal-widget]**
- **[p5.web.widgets.create-container]** - Alias to **[create-container-widget]**

The first 3 in our list above, does the exact same thing as the last 3 above. The reason why we have the **[p5.web.widgets.xxx]** overloads, is to make it easier to retrieve your list of Active Events, according to which namespace they belong to. Try typing out e.g. `p5.web.` into your System42's App/Executor, and then click CTRL+SPACE to see an example of how this might be beneficial for you.

#### Initializing your widget

The **[oninit]** argument declares a lambda object, which will be executed when the widget is displayed. This lambda will be executed on the server, like all other lambda objects. It allows you to create initialization logic. The **[oninit]** lambda will be executed only when the widget becomes visible, and in fact, once *every time* your widget becomes visible. This means that if you hide a widget, for then to display it again - Your **[oninit]** will be executed every time you show your widget.

#### Uniquely IDentifying your widgets

As previously discussed, the ID of your widget is the value of your widget creation node. Every widget you create on the same page must have a unique ID.  This might create a problem for you, as you create reusable lambda objects, intended to be repeatedly executed, that creates some sort of widget for you. For such situations, you can simply omitt the ID, and have an automatically created ID assigned to your widget. This ID will in most cases consist of an *"x"*, followed by a random 7 digits hexadecimal number, resembling something like the following *"x0123adf"*.

This makes it impossible for you to know the ID of your widgets as you create your widget's lambda objects. However, this is not a problem for you, since the ID of your widget is always passed into your Ajax event handlers automatically. The name of this node is **[_event]**. We will take a look at some examples later in this book, where we take advantage of this automatically generated ID.

#### Widget lambda events

The **[events]** argument, allows you to associate Active Events with your widget. These are often referred to as *"widget lambda events"* in P5. We will go into more details of exactly what this is later, but for now, think of these as *"widget methods"*, *"functions"*, or pieces of logic, you can invoke, which are associated with your widget, and only exists for the life time of your widget. If you've done some development in C#, think of them as your widget's methods.

To declare an **[events]** collection, you can add a child node, having the name of **[events]**. This declares a *"widget lambda event collection"*. The name of the children node beneath your **[events]** node, becomes the name of the Active Event associated with your widget. Raising such an event, is similar to raising any other types of Active Events, and there are no semantic differences between a predefined event, or a widget lambda event. Below is an example.

```
create-widget
  element:button
  class:btn btn-primary
  innerValue:Click me
  onclick

    // Raising the widget's lambda event
    sys42.examples.foo-event

  events

    // Declaring a widget lambda event
    sys42.examples.foo-event
      sys42.windows.info-tip:Your widget's lambda event says hello!
```

### Invisible properties and Ajax events

Sometimes, you might need to associate some piece of data with your widget, that you do not want to render as attributes to the client. This is easily done, by prepending your attribute name with either an underscore *"_"*, or a period *"."*. This makes it possible for you to associate an *"attribute value"* with your widget, that is actually never passed on to the client. This *"attribute"*, or *"property"* to be more accurate, can easily be retrieved and modified on the server side, the same way normal attributes can be accessed and modified. However, such *"invisible attributes"* are never transferred to the client in any ways, and not possible to neither read nor change by the browser/client.

You can also create invisible Ajax events using the same technique, by for instance creating an Ajax event named *".onfoo"*, which is never rendered to the client, but can be invoked, by tapping into the JavaScript API of P5. This allows you to create what's often referred to as *"web methods"*, associate these with some widget, and invoke these web methods from JavaScript.

### Changing attributes and properties

To change an attribute, you can use **[p5.web.widgets.property.set]**. To retrieve the value of an attribute, you can use **[p5.web.widgets.property.get]**. The naming of these Active Events, is a common convention in P5, where the collection type you wish to access is given as a major part of the event - While the operation you wish to perform is supplied as the latter parts of its name. Normally these Active Events have a namespace before their collection name, associating them with a particular part of P5. Think of *"p5.web.widgets"* as the namespace, *"property"* as the collection type, and *"get"* or *"set"* as the operation you want to perform. However, both of the above events have convenience overloads, which you'd probably want to use instead. These are called respectively;

* __[set-widget-property]__
* __[get-widget-property]__

Both of the above mentioned Active Events, takes one or more IDs, and reacts upon the specified widget(s) accordingly. Below is a piece of Hyperlambda that creates a widget, which once the mouse is hovered above it, changes a couple of attributes. When the mouse leaves the widget, it retrieves a couple of attributes, and shows a debug window.

```
create-widget:my_video
  element:video
  width:320
  height:240
  controls
  src:http://www.w3schools.com/html/movie.ogg
  type:video/ogg
  innerValue:Your browser needs to be updated
  onmouseover
    p5.web.widgets.property.set:my_video
      width:640
      height:480
  onmouseout
    p5.web.widgets.property.get:my_video
      src
      type
    sys42.windows.show-lambda:x:/..
```

As you can see in our example above, you can change and retrieve multiple attributes in one invocation. You can also change and retrieve attributes for multiple widgets in one invocation, by supplying a lambda expression, leading to multiple valid IDs to existing widgets. We will have a look at the latter in a later chapter.

## Debugging your Ajax events

Often you need to know the state of some piece of lambda during the execution of an Ajax event. This is easily done, by invoking **[sys42.windows.show-lambda]**, and passing in an expression, leading to whatever it is you wish to inspect. Above in our **[onmouseout]** example, there is an example of how to view the entire lambda object for our *"onmouseout"* Ajax event handler.

Don't worry if you do not understand the `:x:/..` expression above. It will be thoroughly explained in a later chapter.

[Chapter 4, Your first Ajax form](chapter-4.md)