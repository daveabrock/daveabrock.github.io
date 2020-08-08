---
date: "2020-08-04"
title: "C# named arguments: not new, but still great"
excerpt: "We discuss the value of named arguments in the C# language."
tags: [dotnet-stacks]
header:
    overlay_image: /assets/images/stacks-10-card.png
    overlay_filter: 0.8

---

I hope you enjoyed my posts about all the cool, new C# 9 features [that are coming with .NET 5](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits). For this post, I want to talk about something that's been around for years, since C# 4, but have been using a lot of late: named arguments.

Until recently, I've been using them when I *had to*: when working with optional arguments. Now, I'm appreciating how they can help with readability as well.


## Why named arguments?

They help with booleans

If you are forced to use a bunch of parameters

But positional parameters doesn’t follow Named Parameters. It throws compilation error “Named argument specifications must appear after all fixed arguments have been specified”.

OPtional = default values, [Optional] or params

## What are named and optional arguments?

Named arguments enable you to specify an argument for a particular parameter by associating the argument with the parameter's name rather than with the parameter's position in the parameter list. Optional arguments enable you to omit arguments for some parameters. Both techniques can be used with methods, indexers, constructors, and delegates.

When you use named and optional arguments, the arguments are evaluated in the order in which they appear in the argument list, not the parameter list.

Named and optional parameters, when used together, enable you to supply arguments for only a few parameters from a list of optional parameters.

## What are named arguments?

Named arguments free you from the need to remember or to look up the order of parameters in the parameter lists of called methods. The parameter for each argument can be specified by parameter name. For example, a function that prints order details (such as, seller name, order number & product name) can be called in the standard way by sending arguments by position, in the order defined by the function.

## What are optional arguments?

The definition of a method, constructor, indexer, or delegate can specify that its parameters are required or that they are optional. Any call must provide arguments for all required parameters, but can omit arguments for optional parameters.

Each optional parameter has a default value as part of its definition. If no argument is sent for that parameter, the default value is used. A default value must be one of the following types of expressions:

a constant expression;

an expression of the form new ValType(), where ValType is a value type, such as an enum or a struct;

an expression of the form default(ValType), where ValType is a value type.

Optional parameters are defined at the end of the parameter list, after any required parameters. If the caller provides an argument for any one of a succession of optional parameters, it must provide arguments for all preceding optional parameters. Comma-separated gaps in the argument list are not supported. For example, in the following code, instance method ExampleMethod is defined with one required and two optional parameters.

## Overload resolution

Use of named and optional arguments affects overload resolution in the following ways:

A method, indexer, or constructor is a candidate for execution if each of its parameters either is optional or corresponds, by name or by position, to a single argument in the calling statement, and that argument can be converted to the type of the parameter.

If more than one candidate is found, overload resolution rules for preferred conversions are applied to the arguments that are explicitly specified. Omitted arguments for optional parameters are ignored.

If two candidates are judged to be equally good, preference goes to a candidate that does not have optional parameters for which arguments were omitted in the call. This is a consequence of a general preference in overload resolution for candidates that have fewer parameters.

## Wrapping up
