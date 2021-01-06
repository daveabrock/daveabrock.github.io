---
date: "2021-01-05"
title: "More with Gruut: Use the Microsoft Bot Framework to analyze emotion with the Azure Face API"
subtitle: In this post, we use the Azure Face API to send Gruut attachments.
share-img: /assets/img/face-api.png
tags: [csharp]
---

Last summer—it was 2020, I think it was summer—I [published a post](https://daveabrock.com/2020/07/28/azure-bot-service-cognitive-services) that showed off how to use Azure text sentiment analysis with the Microsoft Bot Framework. I used it to build on [Shahed Chowdhuri](https://twitter.com/shahedC)'s [GruutChatbot project](https://github.com/shahedc/GruutChatbot). The gist was that Gruut would respond with *I am Gruut* differently based on the conveyed emotion of the text that a user enters. (As before, the project doesn't have a legal team so we're using the *Gruut* naming to avoid any issues from the Smisney Smorporation.)

Here's a thought: since the Bot Framework allows users to send attachments, what if we send him a picture? How would Gruut react to sending images of a happy-seeming person, or an upset-seeming person? We can do this with the [Azure Face API](https://azure.microsoft.com/services/cognitive-services/face/), which [allows us to detect](https://azure.microsoft.com/services/cognitive-services/face/#demo) *perceived* emotions from faces in images. That's what we'll do in this post.

**Note**: While we're using this service innocently, face detection is a serious topic. Make sure you understand what you are doing, and how the data is used. A good first step is [to check out the Azure Cognitive Services privacy policy](https://azure.microsoft.com/support/legal/cognitive-services-compliance-and-privacy/). For this post, we'll be using stock images.
{: .box-error}

# Before you start

In this post, we won't walk through installing the Bot Framework SDK and Emulator, creating the bot project, and so on—that was covered in the [first post on the topic](https://daveabrock.com/2020/07/28/azure-bot-service-cognitive-services). Check out that post if you need to get acclimated.

We'll need to [create a Face resource](https://portal.azure.com/#create/Microsoft.CognitiveServicesFace) in Azure (yes, that's what it's called). Once you create that, grab the `Endpoint` (from the resource in the Azure Portal) and also a key from the `Keys and Endpoint` section. You can then throw those into Azure Key Vault with the other values ([check out the previous post](https://daveabrock.com/2020/07/28/azure-bot-service-cognitive-services#second-pass-working-with-credentials-safely) for those instructions).

# Process the attachment

First, we need to process the attachment. This involves getting the location from the chat context, and downloading the image to a temporary path. Then, we'll be ready to pass it to the Cognitive Services API. In our renamed `GruutBot` class, we now need to conditionally work with attachments.

Here's how it looks. We'll dig deeper after reviewing the code.

```csharp
protected override async Task OnMessageActivityAsync(
            ITurnContext<IMessageActivity> turnContext,
            CancellationToken cancellationToken)
{
    string replyText;

    if (turnContext.Activity.Attachments is not null)
    {
      // get url from context, then download to pass along to the service
      var fileUrl = turnContext.Activity.Attachments[0].ContentUrl;
      var localFileName = Path.Combine(Path.GetTempPath(),
              turnContext.Activity.Attachments[0].Name);

      using var webClient = new WebClient();
      webClient.DownloadFile(fileUrl, localFileName);

      replyText = await _imageService.GetGruutResponse(localFileName);
    }
    else
    {
      replyText = await _textService.GetGruutResponse(turnContext.Activity.Text, cancellationToken);
    }

    await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);
}
```

Let's focus on what happens when we receive the attachment.

```csharp
if (turnContext.Activity.Attachments is not null)
{
    // get url from context, then download to pass along to the service
    var fileUrl = turnContext.Activity.Attachments[0].ContentUrl;
    var localFileName = Path.Combine(Path.GetTempPath(),
    turnContext.Activity.Attachments[0].Name);

    using var webClient = new WebClient();
    webClient.DownloadFile(fileUrl, localFileName);

    replyText = await _imageService.GetGruutResponse(localFileName);
}
```

For now, let's (blindly) assume we only have one attachment, and the attachment is an image. From the `turnContext` we can retrieve information about the sent message. In our case, we want the `ContentUrl` of the attachment. We'll then store it in a temporary path, with the context's `Name`. Then, [using the `WebClient` API](https://docs.microsoft.com/dotnet/api/system.net.webclient?view=net-5.0), we can download the file to the local temp directory. With a path set, we can now inspect our `GetGruutResponse` method in a new image service, which takes a path to where the image resides.

# Write a new `ImageAnalysisService`

Because we're now using text and analysis SDKs in this app, we should split them out into different services. Let's begin with a new `ImageAnalysisService`.

To kick off a new `ImageAnalysisService.cs` file, we'll read in the settings using the [ASP.NET Core Options pattern](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options?view=aspnetcore-5.0) with `ImageApiOptions`. These align with what I've got stored in the Azure Key Vault.

```csharp
namespace GruutChatbot.Services.Options
{
    public class ImageApiOptions
    {
        public string FaceEndpoint { get; set; }
        public string FaceCredential { get; set; }
    }
}
```

Then, after bringing in the `Microsoft.Azure.CognitiveServices.Vision.Face` NuGet library, we can use constructor dependency injection to "activate" our services. Here's the beginning of the class:

```csharp
namespace GruutChatbot.Services
{
    public class ImageAnalysisService
    {
        readonly ImageApiOptions _imageApiSettings;
        readonly ILogger<ImageAnalysisService> _logger;
        readonly FaceClient _imageClient;

        public ImageAnalysisService(
            IOptions<ImageApiOptions> options,
            ILogger<ImageAnalysisService> logger)
        {
            _imageApiSettings = options.Value ?? throw new
                ArgumentNullException(nameof(options),
                "Image API options are required.");
            _logger = logger;
            _imageClient = new FaceClient(
                new ApiKeyServiceClientCredentials(
                    _imageApiSettings.FaceCredential))
            {
                Endpoint = _imageApiSettings.FaceEndpoint
            };
        }
}
```

Now, we'll write a `GetGruutResponse` method that takes the file path to our downloaded attachment. Here's how the method looks (we'll dig in after):

```csharp
public async Task<string> GetGruutResponse(string filePath)
{
  try
  {
    var faceAttributes = new List<FaceAttributeType?>
       { FaceAttributeType.Emotion};

    using var imageStream = File.OpenRead(filePath);

    var result = await _imageClient.Face.DetectWithStreamAsync(
              imageStream, true, false, faceAttributes);

    return GetReplyText(GetHeaviestEmotion(result));
  }
  catch (Exception ex)
  {
    _logger.LogError(ex.Message, ex);
  }
  return string.Empty;
}
```

First, we need to [pass in a `List<FaceAttributeType?>`](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.models.faceattributetype?view=azure-dotnet), which tells the SDK which face attributes you want back. There's a ton of options, like `FacialHair`, `Gender`, `Hair`, `Blur`, and so on—we just need `Emotion`.

```csharp
var faceAttributes = new List<FaceAttributeType?>
                    { FaceAttributeType.Emotion};
```

Then, we'll open up a `FileStream` for the file in question.

```csharp
using var imageStream = File.OpenRead(filePath);
```

To make the Cognitive Services call, we can call the `DetectWithStreamAsync` method. The `true` switch is to return a `faceId`, and the `false` is for not returning landmarks.

```csharp
var result = await _imageClient.Face.DetectWithStreamAsync(
                    imageStream, true, false, faceAttributes);
```

When we call this, we're going to get a list of all different emotions and their values, from 0 to 1. For us, we'd like to pick the emotion that carries the most weight. To do that, I wrote a `GetHeaviestEmotion` method. This is a common use case, as the SDK has a `ToRankedList` extension method [that suits my needs](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.vision.face.emotionextensions.torankedlist?view=azure-dotnet).

```csharp
private string GetHeaviestEmotion(IList<DetectedFace> imageList) =>
            imageList.FirstOrDefault().FaceAttributes.Emotion.
                ToRankedList().FirstOrDefault().Key;
```

Now, all that's left is to figure out what to send to Gruut. I created a `GruutMoods` class that just has some `static readonly` strings for ease of use:

```csharp
namespace GruutChatbot.Services
{
    public static class GruutMoods
    {
        public readonly static string PositiveGruut = "I am Gruut.";
        public readonly static string NeutralGruut = "I am Gruut?";
        public readonly static string NegativeGruut = "I AM GRUUUUUTTT!!";
        public readonly static string FailedGruut = "I.AM.GRUUUUUT";
    }
}
```

Now, we can use a switch expression to determine what to send back:

**Note**: I classified the perceived emotions based on trial-and-error using a few images. Your mileage may vary.
{: .box-warning}

```csharp
private static string GetReplyText(string emotion) => emotion switch
{
  "Happiness" or "Surprise" => GruutMoods.PositiveGruut,
  "Anger" or "Contempt" or "Disgust" or "Sadness" =>
           GruutMoods.NegativeGruut,
  "Neutral" or "Fear" => GruutMoods.NeutralGruut,
  _ => GruutMoods.FailedGruut
};
```

So now, in `GetGruutResponse` this is what we'll return:

```csharp
return GetReplyText(GetHeaviestEmotion(result));
```

Again, here's all of `GetGruutResponse` for your reference:

```csharp
public async Task<string> GetGruutResponse(string filePath)
{
    try
    {
      var faceAttributes = new List<FaceAttributeType?>
        { FaceAttributeType.Emotion};

      using var imageStream = File.OpenRead(filePath);

      var result = await _imageClient.Face.DetectWithStreamAsync(
                  imageStream, true, false, faceAttributes);

      return GetReplyText(GetHeaviestEmotion(result));
    }
    catch (Exception ex)
    {
      _logger.LogError(ex.Message, ex);
    }

    return string.Empty;
}
```

And finally, the entirety of `ImageAnalysisService`:

```csharp
using GruutChatbot.Services.Options;
using Microsoft.Azure.CognitiveServices.Vision.Face;
using Microsoft.Azure.CognitiveServices.Vision.Face.Models;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;

namespace GruutChatbot.Services
{
    public class ImageAnalysisService
    {
        readonly ImageApiOptions _imageApiSettings;
        readonly ILogger<ImageAnalysisService> _logger;
        readonly FaceClient _imageClient;

        public ImageAnalysisService(
            IOptions<ImageApiOptions> options,
            ILogger<ImageAnalysisService> logger)
        {
            _imageApiSettings = options.Value ?? throw new
                ArgumentNullException(nameof(options),
                "Image API options are required.");
            _logger = logger;
            _imageClient = new FaceClient(
                new ApiKeyServiceClientCredentials(
                    _imageApiSettings.FaceCredential))
            {
                Endpoint = _imageApiSettings.FaceEndpoint
            };
        }

        public async Task<string> GetGruutResponse(string filePath)
        {
            try
            {
                var faceAttributes = new List<FaceAttributeType?>
                    { FaceAttributeType.Emotion};

                using var imageStream = File.OpenRead(filePath);

                var result = await _imageClient.Face.DetectWithStreamAsync(
                    imageStream, true, false, faceAttributes);

                return GetReplyText(GetHeaviestEmotion(result));
            }
            catch (Exception ex)
            {
                _logger.LogError(ex.Message, ex);

            }

            return string.Empty;
        }

        private string GetHeaviestEmotion(IList<DetectedFace> imageList) =>
            imageList.FirstOrDefault().FaceAttributes.Emotion.
                ToRankedList().FirstOrDefault().Key;

        private static string GetReplyText(string emotion) => emotion switch
        {
            "Happiness" or "Surprise" => GruutMoods.PositiveGruut,
            "Anger" or "Contempt" or "Disgust" or "Sadness" =>
              GruutMoods.NegativeGruut,
            "Neutral" or "Fear" => GruutMoods.NeutralGruut,
            _ => GruutMoods.FailedGruut
        };
    }
}
```

Also, if you aren't aware, you'll need to add the service to the DI container in `Startup.cs`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Create the Bot Framework Adapter with error handling enabled.
    services.AddSingleton<IBotFrameworkHttpAdapter, AdapterWithErrorHandler>();

    // Create the bot as a transient. In this case the ASP Controller is expecting an IBot.
    services.AddTransient<IBot, GruutBot>();

    // some stuff removed for brevity

    services.Configure<ImageApiOptions>(Configuration.GetSection(nameof(ImageApiOptions)));
    services.Configure<TextApiOptions>(Configuration.GetSection(nameof(TextApiOptions)));

    services.AddSingleton<ImageAnalysisService>();
    services.AddSingleton<TextAnalysisService>();    
}
```

# Repeat with `TextAnalysisService`

With this in place, a refactored `TextAnalysisService` takes the same shape and looks like this:

```csharp
using Azure;
using Azure.AI.TextAnalytics;
using GruutChatbot.Services.Options;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Options;
using System;
using System.Threading;
using System.Threading.Tasks;

namespace GruutChatbot.Services
{
    public class TextAnalysisService
    {
        readonly TextApiOptions _textApiOptions;
        readonly ILogger<TextAnalysisService> _logger;
        readonly TextAnalyticsClient _textClient;

        public TextAnalysisService(
            IOptions<TextApiOptions> options,
            ILogger<TextAnalysisService> logger)
        {
            _textApiOptions = options.Value ?? throw new
                ArgumentNullException(nameof(options),
                "Text API options are required.");
            _logger = logger;
            _textClient = new TextAnalyticsClient(
                new Uri(_textApiOptions.CognitiveServicesEndpoint),
                new AzureKeyCredential(_textApiOptions.AzureKeyCredential));
        }

        public async Task<string> GetGruutResponse(string inputText, 
                CancellationToken cancellationToken)
        {
            try
            {
                var result = await _textClient.AnalyzeSentimentAsync(
                    inputText,
                    cancellationToken: cancellationToken);

                return GetReplyText(result.Value.Sentiment);
            }
            catch (Exception ex)
            {
                _logger.LogError(ex.Message, ex);

            }

            return string.Empty;
        }

        static string GetReplyText(TextSentiment sentiment) => sentiment switch
        {
            TextSentiment.Positive => GruutMoods.PositiveGruut,
            TextSentiment.Negative => GruutMoods.NegativeGruut,
            TextSentiment.Neutral => GruutMoods.NeutralGruut,
            _ => GruutMoods.FailedGruut
        };
    }
}
```

# The final product

Now, with our SDK calls refactored to services, here's our full `GruutBot`:

```csharp
using GruutChatbot.Services;
using Microsoft.Bot.Builder;
using Microsoft.Bot.Schema;
using System.Collections.Generic;
using System.IO;
using System.Net;
using System.Threading;
using System.Threading.Tasks;

namespace GruutChatbot.Bots
{
    public class GruutBot : ActivityHandler
    {
        private readonly TextAnalysisService _textService;
        private readonly ImageAnalysisService _imageService;

        public GruutBot(TextAnalysisService textService, ImageAnalysisService imageService)
        {
            _textService = textService;
            _imageService = imageService;
        }

        protected override async Task OnMessageActivityAsync(
            ITurnContext<IMessageActivity> turnContext,
            CancellationToken cancellationToken)
        {
            string replyText;

            if (turnContext.Activity.Attachments is not null)
            {
                // get url from context, then download to pass along to the service
                var fileUrl = turnContext.Activity.Attachments[0].ContentUrl;
                var localFileName = Path.Combine(Path.GetTempPath(),
                    turnContext.Activity.Attachments[0].Name);

                using var webClient = new WebClient();
                webClient.DownloadFile(fileUrl, localFileName);

                replyText = await _imageService.GetGruutResponse(localFileName);
            }
            else
            {
                replyText = await _textService.GetGruutResponse(turnContext.Activity.Text, cancellationToken);
            }

            await turnContext.SendActivityAsync(MessageFactory.Text(replyText, replyText), cancellationToken);

        }

        protected override async Task OnMembersAddedAsync(
            IList<ChannelAccount> membersAdded,
            ITurnContext<IConversationUpdateActivity> turnContext,
            CancellationToken cancellationToken)
        {
            // welcome new users
        }
    }
}
```

# The bot in action

Now, let's see how the bot looks when we send Gruut text and images.

![GruutBot in action]({{ site.url }}{{ site.baseurl }}/assets/img/gruut-images.png)

# Wrap up

In this post, we showed how to use Azure Cognitive Services face emotion detection with the Microsoft Bot Framework. We processed and downloaded attachments, passed them to a new `ImageAnalysisService`, and called the Face API to detect perceived emotion. Then, we wrote logic to send a message back to Gruut. Finally, we showed a refactoring of the existing text analysis to a new `TextAnalysisService`, which allows us to manage calls to different APIs seamlessly.
