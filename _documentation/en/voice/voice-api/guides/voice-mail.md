---
title: Voice Mail using .NET
navigation_weight: 11
description: Learn how to record audio from inbound calls using .NET and the Vonage Voice API.
---

# Voice Mail using .NET

## Introduction

The [Vonage Voice API](/voice/voice-api/overview) is the easiest way to build high-quality voice applications in the Cloud. In this guide you will learn how to record audio from inbound calls using .NET and the Vonage Voice API.

This guide provides step by step instructions showing you how to:

1. Set up a Vonage account
2. Create a Vonage Application
3. Configure a Vonage Application 
4. Create an ASP.NET Core application
5. Add the Vonage package to it
6. Receive a call and record it
7. Retrieve the recording from Vonage

## Prerequisites

There are a few prerequisites that you need to complete before you can work through this guide. If you've already completed any of them, feel free to skip that step.

### Create Vonage Account

You can create a Vonage account via the [Dashboard](https://ui.idp.vonage.com/ui/auth/registration?icid=tryitfree_adpdocs_nexmodashbdfreetrialsignup_inpagelink). Within the Dashboard you can create Applications and purchase Vonage numbers. You can also perform these tasks using the [Vonage CLI](https://github.com/vonage/vonage-cli).

### Install the Vonage CLI

The [Vonage CLI](https://developer.nexmo.com/application/vonage-cli) allows you to carry out many operations on the command line. Examples include creating applications, purchasing numbers, and linking a number to an application.

To install the CLI with NPM run:

```bash
npm install --location=global @vonage/cli
```

Set up the Vonage CLI to use your Vonage API Key and API Secret. You can get these from the [settings page](https://dashboard.nexmo.com/settings) in the Dashboard.

Run the following command in a terminal, while replacing `API_KEY` and `API_SECRET` with your own:

```bash
vonage config:set --apiKey=API_KEY --apiSecret=API_SECRET
```

### How to run Ngrok

<a name="how-to-run-ngrok"></a>

You must make your webhook accessible to Vonage’s APIs over the public Internet. A straightforward way to achieve this during development without standing up your own server is to use [Ngrok](https://ngrok.com/). To learn more, [read our documentation on Ngrok](/tools/ngrok).

Download and install ngrok, then execute the following command to expose your application on port 3000 to the public Internet:

``` shell
./ngrok http 3000
```

If you are a paid subscriber you could type:

``` shell
./ngrok http 3000 -subdomain=your_domain
```

> **NOTE:** In this example Ngrok will divert the Vonage webhooks you specified when you created your Vonage application to `localhost:3000`. Although port 3000 is shown here, you can use any free port that is convenient.

### Create a Voice Application

There are two methods for creating a Voice application:

1. Using the Vonage CLI
2. Using the Dashboard

Each of these methods is described in the following sections.

#### How to create a Voice application using the Vonage CLI

To create your application using the Vonage CLI, enter the following command into the shell replacing `NGROK_HOST_NAME` with the ngrok host that you got when you set up ngrok:

``` shell
vonage apps:create "AspNetTestApp" --voice_answer_url=http://NGROK_HOST_NAME/webhooks/answer --voice_event_url=http://NGROK_HOST_NAME/webhooks/events
```

This command creates a Vonage Application with Voice [capability](/application/overview#capabilities). It  configures the Application with your answer and event webhook URLs and generates a private key file `private.key`, which you should save in your project directory.

The command returns a unique Application ID. Use this Application ID to link your Vonage virtual number to your Application by executing the following:

```shell
vonage apps:link APPLICATION_ID --number=VONAGE_NUMBER
```

This will link that Vonage Virtual Number to your application, which will cause all voice events occurring for that number to be routed to your designated URLs.

#### How to create a Voice application using the Dashboard

You can create Voice applications in the [Dashboard](https://dashboard.nexmo.com/applications).

To create your application using the Dashboard:

1. Under [Applications](https://dashboard.nexmo.com/applications) in the Dashboard, click the **Create a new application** button.

2. Under **Name**, enter the Application name. Choose a name for ease of future reference.

3. Click the button **Generate public and private key**. This will create a public/private key pair and the private key will be downloaded by your browser. Save the private key that was generated.

4. Under **Capabilities** select the **Voice** button.

5. In the **Answer URL** box, enter the URL for your inbound calls webhook, for example, `http://example.com/webhooks/answer`.

6. In the **Event URL** box, enter the URL for your call status webhook, for example, `http://example.com/webhooks/events`.

7. Click the **Generate new application** button.

8. You are now taken to the next step of the Create Application procedure where you should link your Vonage number to the application.

You have now created your application.

> **NOTE:** Before testing your application ensure that your webhooks are configured and your webhook server is running.

## Create the Voice Project File

To start, you will create a Voice `csproj` file. To make testing easier, configure Kestrel with HTTPS disabled.

In your terminal, execute the following command:

```shell
dotnet new mvc --no-https -n VonageVoice
```

## Install Vonage Package

To install the Vonage NuGet package, change directory into the `VonageVoice` repository and run the following command:

```shell
dotnet add package Vonage
```

You can now open the `VonageVoice.csproj` file in Visual Studio.

> **Note**: This tutorial assumes that you are using Visual Studio. Alternatively, you could open the `VonageVoice` folder in Visual Studio Code or another editor, but you might have to create some of the scaffolding that Visual Studio generates for you.

## Add Voice Controller

Right-click on the `Controllers` folder and select add->Controller. Select "Add Empty MVC Controller" and name it `VoiceController`.

Add `using` statements for `Vonage.Voice`, `Vonage.Voice.Nccos`, `Vonage.Voice.Nccos.Endpoints`, `Vonage.Request`, and `Microsoft.Extensions.Configuration` at the top of this file.

### Inject Configuration

Dependency inject an `IConfiguration` object via the constructor:

```csharp
private readonly IConfiguration _config { get; set; }

public VoiceController(IConfiguration config)
{
    _config = config;
}
```

## Add Voicemail Answer Route

Add a route to your `VoiceController` to handle the inbound call. This will be located at `/webhooks/answer` and will return a [NCCO](https://developer.nexmo.com/voice/voice-api/ncco-reference) with a `record` action, which instructs Vonage to start recording the call.

You must also specify the URL of another webhook - `/webhooks/recording` - which Vonage will make a request to when the recording is ready for download. You will code this webhook in the next step.

```csharp
[HttpGet("/webhooks/answer")]
public string Answer()
{
    var talkAction = new TalkAction
    {
        Text = "Hello, you have reached Steve's number," +
        " he cannot come to the phone right now. " +
        "Please leave a message after the tone."
    };
    var recordAction = new RecordAction
    {
        EndOnSilence = "3",
        BeepStart = "true",
        EventUrl = new[] { $"{Request.Scheme}://{Request.Host}/webhooks/recording" },
        EventMethod = "POST"
    };

    var ncco = new Ncco(talkAction, recordAction);
    return ncco.ToString();
}
```

## Add Recording Route

After the call has finished recording, Vonage will send a request to the webhook URL you configured in the `record` action in your NCCO. This request will contain the URL from which you can download the audio file.

In this tutorial, you'll use the `GetRecording` API to download the recording to your local machine:

```csharp
[HttpPost("/webhooks/recording")]
public async Task<IActionResult> Recording()
{
    var appId = _config["APPLICATION_ID"];
    var privateKeyPath = _config["PRIVATE_KEY_PATH"];
    var credentials = Credentials.FromAppIdAndPrivateKeyPath(appId, privateKeyPath);
    var voiceClient = new VoiceClient(credentials);
    var record = await Vonage.Utility.WebhookParser.ParseWebhookAsync<Record>(Request.Body, Request.ContentType);
    var recording = await voiceClient.GetRecordingAsync(record.RecordingUrl);
    await System.IO.File.WriteAllBytesAsync("your_recording.mp3", recording.ResultStream);
    return StatusCode(204);
}
```

## Configure the ASP.NET App

Open your `appsettings.json` file and add the following two fields to it:

* `APPLICATION_ID`: Your Voice API Application's unique identifier.
* `PRIVATE_KEY_PATH`: The location of your Application's `private.key` file.

## Run Your App

In your console navigate to the `csproj` directory and execute the command `dotnet run --urls=http://localhost:3000`. This will start the application up on `http://localhost:3000`. Open that URL in your browser.

## What's Next?

Find out more about the capabilities of the Vonage Voice API:

* [Read the Voice API documentation](/voice/voice-api/overview)
* [View the API Reference](/voice/voice-api/api-reference)
* [Play Text To Speech into a Call](/voice/voice-api/tutorials/play-audio-into-call-dotnet)
* [Play Audio into a Call](/voice/voice-api/code-snippets/record-a-call/dotnet)
* [Take Speech Input](/voice/voice-api/code-snippets/handle-user-input-with-asr/dotnet)
* [Handle User Key Input](/voice/voice-api/code-snippets/handle-user-input-with-dtmf)
* [NCCO Reference](/voice/voice-api/ncco-reference)
