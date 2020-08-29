---
date: "2020-08-28"
title: "Dev Discussions - Luis Quintanilla (2 of 2)"
excerpt: In the latest interview, we talk with Luis Quintanilla about ML.NET.
tags: [dotnet-stacks, dev-discussions]
header:
    overlay_image: /assets/images/dev-discussions-luis.png
    overlay_filter: 0.8
---

This is the full interview from my discussion with Luis Quintanilla in my weekly (free!) newsletter, *The .NET Stacks*. Consider [subscribing today](https://dotnetstacks.com) to get this content right away!
{: .notice--success}

This is an introduction.

![Luis Quintanilla]({{ site.url }}{{ site.baseurl }}/images/luisquintanilla-picture.jpg)

## How did you get to Microsoft, and what do you do there these days?

Before joining Microsoft, I was consulting in the artificial intelligence space for companies in various industries. I was fortunate enough to work on a wide variety of projects using different technologies. Most of these technologies were not .NET technologies. At Build 2018, ML.NET was announced. I had done limited .NET Framework development in the past. Coincidentally, around the same time, .NET Core 2.1 was released so I thought it was a good time to jump in with both feet. I was immediately hooked and since I wanted a break from the technologies I used every day at work, I started using it in my spare time.

I found myself being productive with ML.NET and would document what I was experimenting with on my blog. Additionally, I gave several talks at local user groups and conferences on ML.NET. Around the same time, Microsoft was looking for someone to join the Microsoft Docs team to focus on ML.NET so I happened to naturally fit the role. I was fortunate enough to connect with the folks on the hiring team and the rest is history.

The past year and a half, while my focus has been ML.NET, I have recently expanded my responsibilities to also create content for Azure Machine Learning and .NET for Apache Spark. Like with ML.NET, I've had a great experience working with these technologies which are integral of data and machine learning workflows.

## What made you focus on ML.NET over other development tech?

I could write an entire essay on why ML.NET but all of the reasons can be summarized in a single word, .NET. Now, to expand on that, here are a few reasons why I enjoy ML.NET so much!

### Languages

.NET is made up of several fantastic languages, C#, F#, and Visual Basic.

Though not unique to .NET, I like statically-typed languages. I'm sure many of the readers are able to build their applications and successfully run them without errors on the first try 😉. That however is usually not my experience. Therefore, I prefer catching as many errors as possible at compile time. Another reason I like types is they provide a way of documenting your code.

Sometimes, it may be the case that you step away from your code for a day or two and have to refresh your memory. Types provide some form of annotation that at the very least provide you with a hint of what data types are used in functions and their overall logic. Annotations by themselves are useful, but having the compiler check those type annotations makes me feel extra safe. This is of extreme importance when working in data science and machine learning scenarios. Although ultimately the data used by machine learning algorithms to train models is encoded as numbers, knowing your data schema and checking it at compile time may help reduce the number of errors in your code as you transform your data. With languages like Python, although you can do type checking and conversions at runtime as well as use type annotations and static type checkers, it's not the default way of working with the language. In fact, I’ve noticed various Python libraries working to introduce type annotations. I’m sure this is no easy task since it’s all being done retroactively but in my opinion it’s a worthwhile effort in the long term.

Lately I've been doing more F# development and the more I use it, the more I like it. F# for me provides a nice balance between Python and C#. F# gives you the productivity and succinctness of a language like Python, while still having the compiler and many other neat features at your disposal. Typically, data is represented as a list or collection of items. In F#, functions and collections are integral features of the language. Therefore, you can work with them in easy and powerful ways. When performing data transformations, you can leverage the ​fact that your data is immutable by default and by using function composition, you can chain together multiple operations into a data transformation pipeline while limiting side effects. Although F# is a functional-first language, that doesn't mean you can't take advantage of object-oriented concepts like interfaces and classes. Because F# is a language that runs on .NET, you're also able to use existing .NET libraries and NuGet packages, such as ML.NET in your applications.

### Runtime

The .NET runtime is fast and performant. This is important in two scenarios, training machine learning models and deploying them. A good part of training machine learning models involves performing operations on vectors and matrices. .NET provides Single nstruction Multiple Data (SIMD) enabled types via the [System.Numerics](https://docs.microsoft.com//dotnet/standard/simd) namespace. ML.NET leverages these types where possible to increase the throughput of the training operations making training fast and efficient.

A common deployment target for machine learning models is the web. In .NET, this means deploying your models in an ASP.NET application. In benchmarks, such as [TechEmpower](https://www.techempower.com/benchmarks/#section=data-r19&hw=ph&test=composite), ASP.NET Core ranks in the top 10 making it a great option to run fast and scalable machine learning applications on.

### Tooling

.NET has world class tooling across the board and you can't go wrong with any of your choices. [Visual Studio](https://visualstudio.microsoft.com/) is an excellent IDE packed with tons of functionality to help developers be more productive. Alternatively, another great IDE for .NET is [Jetbrains Rider](https://www.jetbrains.com/rider/). If you're looking for a more lightweight development environment, you can also use [Visual Studio Code](https://code.visualstudio.com/). When working with F#, you can use the [Ionide](http://ionide.io/) extension which makes F# development a pleasant experience.

Data science and machine learning workflows are very experimental. This means that you sometimes may want to have an interactive environment where you get feedback in near real-time of the outputs generated by your code. You’d also like a way to visualize your data inline. Within the data science community, a popular interactive computing environment is Jupyter Notebooks. You can leverage this interactive environment in .NET through .NET Interactive, which provides among many things, a kernel for you to run .NET code interactively.

### Deployment Targets

.NET Core was a big step towards .NET everywhere. That's not to say you couldn't run .NET outside of Windows prior to .NET Core, but .NET Core was able to cohesively package things together so that you didn't have that extra cognitive load of choosing where you want your applications to run.

Now, if you have .NET everywhere, this means at least from a deployment standpoint, you can run your machine learning models across various platforms, depending on where your users are. As mentioned previously, a common deployment scenario is hosting your machine learning model in a web API. .NET, however gives you the ability to take that same model and also run it in desktop, mobile, IoT and many other deployment targets.

The other neat thing about .NET everywhere is, you're using the languages and tooling you're used to. Whether you're developing WPF, Xamarin or Unity applications, you're typically using the same .NET languages and tooling. Therefore, you're able to leverage your existing skills and be productive almost immediately.

### Modernization

While the latest and greatest features are going into .NET Core, there are still many applications built in .NET Framework that are key to the operations of many organizations. Because ML.NET is 
built on .NET Standard, developers are able to add machine learning to their existing .NET Framework applications to enhance and modernize the capabilities of that application.

### Extensible

Although .NET is great, a large portion of the data science and machine learning space is predominantly made up of libraries and frameworks built in Python. That however does not limit ML.NET because it is extensible. ML.NET supports working with TensorFlow and Open Neural Network Exchange (ONNX) models. [TensorFlow](https://www.tensorflow.org/) is a popular platform for building machine learning models. Using [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET), a set of C# bindings for TensorFlow, users can train and deploy TensorFlow models with ML.NET. [ONNX](https://onnx.ai/) is an open format built to represent machine learning models. This means that you can train a model in other popular tools and frameworks like Azure Custom Vision, PyTorch, Scikit Learn. Then, you can export or convert that model to ONNX and consume it using ML.NET.

### Open Source & Community

ML.NET, like .NET, is open source. This allows for the community to collaborate and contribute to it. Users have various ways of contributing to ML.NET, whether it's raising issues, updating documentation or submitting pull requests, they're all valuable contributions that only help make the framework that much better for everyone to use.

ML.NET has a vibrant and passionate community. Being open source gives visibility into the work being done which allows community members to identify opportunities to improve the core ML.NET code, build solutions around it, or provide training materials. One example of this is [MLOps.NET](https://github.com/aslotte/MLOps.NET), an open source library for tracking the lifecycle of ML.NET machine learning models. Others include [videos](https://www.youtube.com/channel/UCrDke-1ToEZOAPDfrPGNdQw), [blog posts](https://xamlbrewer.wordpress.com/2020/02/20/getting-started-with-ml-net-in-jupyter-notebooks/), and even its very own [conference](https://www.youtube.com/channel/UClv1sloNF4mzWQiQbemHXRw) (make sure to stay tuned for updates on an upcoming hackathon).

These are only a few of the many reasons why I enjoy ML.NET. While it's still relatively new, I foresee healthy growth and a bright future as more individuals use and contribute to it.

## Correct me if I'm wrong, but I believe a big mission of ML.NET is making machine learning accessible—that is, I shouldn't have to be an expert in machine learning to do it in .NET. Even still: how much should I know before I get started?

That's right! ML.NET provides many ways of interacting with it depending on what you're most comfortable with. The easiest way to get started is by using the tooling. The tooling provides a low-
code way of training and consuming ML.NET models. If you prefer a graphical user interface, you can try [Model Builder](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet/model-builder), a Visual Studio extension that guides you through the steps involved in
training a machine learning model. As long as you have a general sense of the problem you're trying to solve (classify text, predict a number, categorize images) and you have a dataset, Model Builder takes care of the rest. Once your model is trained, code to retrain and consume your model is automatically generated for you, making it even easier to integrate your model into your end ​user applications. Some scenarios like image classification are resource intensive. As a result, you have the option of using a GPU locally or in Azure.

Alternatively, if you prefer working on the command line you can use the [ML.NET CLI](https://www.nuget.org/packages/MLNet/), a .NET command line tool for training ML.NET model models and generating consumption code. The idea is very much the same as Model Builder, except now you interact with the tooling via the command line. The CLI is also a great choice for Machine Learning Operations (MLOps) scenarios where model training and deployment is done as part of a continuous integration (CI) or continuous deployment (CD) pipeline.

For folks who want more control, prefer a code-first approach, or are more familiar with machine learning concepts, there's other ways of using ML.NET. One is with the ML.NET Automated ML (Auto ML) API. The AutoML API is leveraged by the tooling to try to find the "best" model. The best model for your problem depends on many factors such as the quantity and distribution of your data and time to train. Therefore, it helps to try different algorithms with different parameters. Exploring various algorithms can be time consuming and inefficient if done by brute force. By using
the AutoML API, you can automate this exploratory phase to find you the best model while at the same time gaining access to various settings provided by the API. Often times, it's good to use the 
AutoML API as a starting point to help guide you in choosing the best algorithm to solve your problem. You can then choose to deploy the trained model or further refine the model using the 
ML.NET API. If you want full control over your machine learning pipeline, you can use the ML.NET API. The API provides you with direct access to data loaders, transformations, trainers, and
prediction components that you can configure as needed to solve your problem.

One of the nice things is, none of the ways of using ML.NET is mutually exclusive. You can start off with the tooling to bootstrap the model training process and from there use the ML.NET API to make further refinements. In the end, it's all about choice and depending on your experience with machine learning and preferred workflow, there's an option for you.

## Where is the dividing line for when I should use machine learning, or use Azure Cognitive Services?

This is a really tough one to answer because there's so many ways you can make the comparison. Both are great products and in some areas, the lines can blur. Here are a few of them that I think might be helpful.

### Custom Training vs Consumption

If you're looking to add machine learning into your application to solve a fairly generic problem, such as language translation or identifying popular landmarks, Azure Cognitive Services is an excellent option. The only knowledge you need to have is how to call an API over HTTP. Being able to work via HTTP also provides you with flexibility over what you use to make the requests to the API. If you want to run your machine learning workflow with Azure Cognitive Services via cURL as part of a background Cron job, that's perfectly acceptable.

Azure Cognitive Services provides a set of robust, tate-of-the-art, pretrained models for a wide variety of scenarios. However, there’s always edge cases. Suppose you have a set of documents that you want to classify and the terminology in your industry is rare or niche. In that scenario, the performance of Azure Cognitive Services may vary because the pretrained models most likely have never encountered some of your industry terms. At that point, training your own model may be a better option, which for this particular scenario, Azure Cognitive Services does not allow you to do.

That's not to say Azure Cognitive Services does not allow you to train custom models. Some scenarios like language understanding and image classification have training capabilities. The difference is, training custom models is not the default for Azure Cognitive Services. Instead, you are encouraged to consume the pretrained models. Conversely, with ML.NET, for training purposes, you're always training custom models using your data.

### Online vs Offline

By default, Azure Cognitive Services requires some form of internet connectivity. In scenarios where there is strong network connectivity, this is not a problem. However, for offline or air-
gapped environments, this is not an option. Although in some scenarios, you can deploy your Azure Cognitive Services model as a container, therefore reducing the number of requests made over the network, you still need some form of internet connectivity for billing purposes. Additionally, not all scenarios support container deployments therefore the types of models you can deploy is limited.

While Azure in general makes sure to responsibly protect the privacy and security of users data in the cloud, in some cases whether it's a regulatory or organizational decision, putting your data in the cloud may not be an option. In those cases, you can leverage Azure Cognitive Services container deployments. Like with the offline scenario, the types of models you can deploy is limited. Additionally, you most likely would not be training custom models since you wouldn't want to send your data to the cloud.

ML.NET is offline first, which means you can train and deploy your models locally without ever interacting with a cloud environment. That being said, you always have the option to scale your training and consumption by provisioning cloud resources. Another benefit of being offline first is, you don't have to containerize your application in order to run it locally. You can take your model and deploy it as part of a WPF application without the additional overhead of a container or connecting over to the internet.

### Cost

Cost is always a tricky thing to talk about because what exactly do you account for when calculating cost? With Azure Cognitive Services, you pay for your consumption. While there are very different tiers you can subscribe to depending on your usage and the free tier is fairly generous, you always pay for what you use. Also, because all the resources are managed for you, you don't have to spend time maintaining any of them. Because most of the models you use are pretrained, it also means you don’t have to spend your time or resources training a model.

ML.NET is always free both the library and the tooling. Since the resources for training and deployment are not managed for you, you have to think about that. However, if your machine learning model is deployed alongside your existing applications, perhaps the management, resources, and overhead may be negligible. In terms of training, since you're always training a ​custom model, the time and resources devoted to that have to be taken into account. The benefit of that though is that your model is fine tuned to your specific problem. Revisiting the offline scenario, if your model is deployed in an offline environment or perhaps inside of an existing WPF application, your costs at that point are practically zero because any usage of the model isn't costing you anything extra. So again, there's many ways you can look at cost and it all depends on what you decide to take into account.

## Do you have some practical use cases for using ML.NET in business apps today?

Definitely! If you have a machine learning problem, the framework you use is fairly agnostic as long as the scenario is supported. Since ML.NET supports classical machine learning as well as deep learning scenarios, it practically can help solve a wide variety of problems. Some examples include:

* Sentiment analysis
* Sales forecasting
* Image classification
* Spam detection
* Predictive maintenance
* Fraud detection
* Web ranking
* Product recommendations
* Document classification
* Demand prediction
* And many others...
  
I would suggest for users interested in seeing how companies are using ML.NET in production today, visit the [ML.NET customer showcase](https://dotnet.microsoft.com/apps/machinelearning-ai/ml-dotnet/customers).

## What kind of projects are you hacking away at now?

My interests are too many. If only there was enough time to do them all! Most recently we’ve started doing the [Machine Learning Community Standup](https://www.youtube.com/watch?v=2gjMrZ9XbRQ). This is a place where folks who are interested in machine learning within the .NET ecosystem can come and meet other members of the community, learn what’s new in the space and ask questions. You can tune in every other Wednesday to check that out. If you’re working on projects with ML.NET or other .NET machine learning libraries, we’d love to hear from you!​

Also, I mentioned it earlier but folks should stay tuned for the next [Virtual ML.NET Conference](https://www.youtube.com/channel/UClv1sloNF4mzWQiQbemHXRw). The first one took place earlier this year and it was a great success. Not only was engagement high
during the conference but interest and excitement has increased since then which can only mean good things for ML.NET. For this next version, instead of workshops and presentations, it’ll be
hands-on in the form of a hackathon. Again, at the moment there aren’t many details, but make sure to stay in touch on [Twitter](https://twitter.com/virtualmlnet) or join the [Slack](https://join.slack.com/t/virtual-mlnet/shared_invite/zt-galp4khg-gUJiri5yvEfTD2vkLa9v0w) community to get the latest updates.

Personally, I've been dabbling in reinforcement learning, which is another form of artificial intelligence. Though many of the concepts don't transfer over, there are some areas where my prior knowledge is applicable, especially when it comes to the use of neural networks as part of deep reinforcement learning.

Much of that work takes me away from .NET, which I always try to get a healthy dose of. As a result, I've been doing more F# development. I still have a ways to go, but I really like the language and always try to get better at it.

For throwaway projects and learning exercises, I've been playing around with Racket. In some ways, my learnings from F# have made me a better Lisp developer. I've always been fond of the language considering for the longest time it was THE language for artificial intelligence. Though I'm far from building Lisp bindings for ML.NET (which I think is feasible), it's always fun to get outside
of your comfort zone.

Of course, ML.NET is never far away either and I'm always trying to find new ways of creatively using ML.NET.

From time to time, I'll post some of the things I'm experimenting with on my [blog](https://www.luisquintanilla.me/) so feel free to check it out.

## What kind of stuff do you work on over at your Twitch stream? When is it?

Currently I stream on Monday and Wednesday mornings at 10 AM EST. My stream is a live learning session where folks sometimes drop in to learn together. On stream, I focus on building data and machine learning solutions using .NET and other technologies. The language of choice on stream is F# though I don’t strictly abide by that and will use what works best for getting the solution working.

Most recently, I built a deep learning model for malware detection using ML.NET. I've been trying to build .NET pipelines with Kubeflow, a machine learning framework for Kubernetes on stream as well. I've had some trouble with that, but that's what the stream is about, learning from mistakes.

Inspired by a reddit post which asked how to get started with data analytics in F#, I’ve started working on using F# and various .NET tools like .NET Interactive to analyze the arXiv dataset.

If any of that sounds remotely interesting, feel free to check out and follow the channel on [Twitch](https://www.twitch.tv/lqdev1). You can also catch the recordings from previous streams on [YouTube](https://www.youtube.com/channel/UCkA5fHdQ4cf3D1J19UNgV7A).

## What is your one piece of programming advice?

Just do it! Everyone has different learning styles, but I strongly believe no amount of videos, books or blog posts compare to actually getting your hands dirty. It can definitely be daunting at first, but no matter how small or basic the application is, uilding things is always a good learning experience. Often there is a lot of work that goes into producing content, so end users typically get the polished product and the happy path. When you're not sure of where to start or would like to go more in depth, these resources are excellent. However, once you stray from that guided environment, you start making mistakes. Embrace these mistakes because they’re a learning experience.