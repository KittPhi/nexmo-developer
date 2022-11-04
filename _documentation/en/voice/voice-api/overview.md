---
title: Overview
meta_title: Text-To-Speech, IVR, Call Recording and more with Vonage's Voice API
navigation_weight: 1
description: The Vonage Voice API is the easiest way to build high-quality voice applications in the Cloud.
---

# Voice API Overview

The Vonage Voice API is the easiest way to build high-quality voice applications in the Cloud. With the Voice API you can:

* Build apps that scale with the web technologies you are already using
* Control the flow of inbound and outbound calls in JSON with Nexmo Call Control Objects (NCCO). (Nexmo is now Vonage).
* Record and store inbound or outbound calls
* Create conference calls
* Send text-to-speech messages in 40 languages with different genders and accents

## Contents

In this document you can learn about:

* [Vonage Voice API Concepts](#concepts) to introduce terminology
* [How to Get Started with the Voice API](#getting-started) including examples in your language
* [Guides](#guides) learn about working with the Voice API
* [Code Snippets](#code-snippets) code snippets to help with specific tasks
* [Use Cases](#use-cases) detailed use cases with code examples
* [Reference](#reference) API documentation and other supporting content

## Concepts

* **Authentication with JWTs** - interaction with the Voice API are authenticated using JWTs (JSON Web Tokens). The [libraries](/tools) handle JWT generation using a unique Vonage Voice Application ID and a Private Key. For more information see [authenticating your applications](/concepts/guides/authentication).
* **Vonage Voice Applications** - Vonage Voice Applications represent a one-to-one mapping with the application that you are building. They contain configuration such as virtual numbers and webhook callback URLs. You can create Vonage Voice Applications using the [Vonage Dashboard](https://dashboard.nexmo.com/sign-in), the [Vonage CLI](/tools), or via the [Application API](/concepts/guides/applications).
* **[Webhooks](/concepts/guides/webhooks)** - HTTP requests are made to your application web server so that you can act upon them. For example, an incoming call will send a webhook.
* **[Call Flow](/voice/voice-api/concepts/call-flow)** - The various stages of a call and how they interact.
* **[Customizing Spoken Text](/voice/voice-api/concepts/customizing-tts)** - Use Speech Synthesis Markup Language (SSML) to control how text-to-speech is read out.
* **[Endpoints](/voice/voice-api/concepts/endpoints)** - When connecting a call, you can connect to another phone number, a `sip` endpoint or a `websocket`. These are known as endpoints.
* **[NCCOs](/voice/voice-api/concepts/ncco)** - Nexmo Call Control Objects are a set of actions that instruct Vonage how to control the call to your Vonage application. For example, you can `connect` a call, send synthesized speech using `talk`, `stream` audio, or `record` a call. They are represented in JSON form as an Array of objects. For more information see the [NCCO Reference](/voice/voice-api/ncco-reference).
* **[Numbers](/voice/voice-api/concepts/numbers)** - The key concepts of using phone numbers in the Vonage Voice API.
* **[Payments Over the Phone](/voice/voice-api/concepts/payments)** - Take payments during phone calls in a PCI compliant secure way.
* **[Recording](/voice/voice-api/concepts/recording)** - Recording audio input from a caller or recording the conversation between two callers.
* **[Text to Speech](/voice/voice-api/concepts/text-to-speech)** - Using our Text-To-Speech engine, you can play machine-generated speech to your callers.
* **[Troubleshooting](/voice/voice-api/concepts/troubleshooting)** - A few things to look for if you are experiencing problems.
* **[WebSockets](/voice/voice-api/concepts/websockets)** - You can connect the audio of a call to a WebSocket to work with it in real time.

## Getting Started

### Voice Playground

In the [Developer Dashboard](https://dashboard.nexmo.com), you can try out the Voice API interactively in the Voice Playground. Once you are [signed up for a Vonage API account](https://dashboard.nexmo.com/signup), you can go to [Voice Playground](https://dashboard.nexmo.com/voice/playground) in the Dashboard (Voice ‣ Voice Playground).

More details are available in this blog post: [Meet Voice Playground, Your Testing Sandbox for Vonage Voice Apps](https://www.nexmo.com/legacy-blog/2017/12/12/voice-playground-testing-sandbox-nexmo-voice-apps/)

### Using the API

The primary way that you'll interact with the Vonage API voice platform is via the [public API](/voice/voice-api/api-reference). To place an outbound call, you make a `POST` request to `https://api.nexmo.com/v1/calls`.

To make this easier, Vonage provides Server SDKs in various languages that take care of authentication and creating the correct request body for you.

To get started, choose your language below and replace the following variables in the example code:

Key | Description
-- | --
`VONAGE_NUMBER` | Your Vonage number that the call will be made from. For example `447700900000`.
`TO_NUMBER` | The number you would like to call to in E.164 format. For example `447700900001`.

```code_snippets
source: '_examples/voice/make-an-outbound-call'
application:
  type: voice
  name: 'Outbound Call code snippet'
  answer_url: https://raw.githubusercontent.com/nexmo-community/ncco-examples/gh-pages/text-to-speech.json
```

## Guides

* **[Masked Calling](/voice/voice-api/guides/masked-calling)** - Enable users to call each other, keeping their real numbers private.
* **[Conference Calling](/voice/voice-api/guides/conference-calling)** - When a phone call is made or received by Vonage it is added to a conversation. A single conversation contains one or more phone calls (sometimes referred to as legs).
* **[Using the Client SDK for App to App Calling](/voice/voice-api/guides/app-to-app-calling)** - Make a phone call from app to another app.
* **[Voice Notifications](/voice/voice-api/guides/voice-notifications)** - Learn how to contact a list of people by phone, convey a message, and see who confirmed that they had received the message.
* **[Interactive Voice Response (IVR)](/voice/voice-api/guides/interactive-voice-response)** - Build an automated phone system for users to input information with the keypad and hear a spoken response.
* **[Integrate Phone Calls to a Dialogflow Chatbot Via Websockets](/voice/voice-api/guides/voice-bot)** - This guide will help you to start with an example Dialogflow bot and interact with it from phone calls using provided sample reference codes using Vonage Voice API.
* **[Connecting Voice Calls to an Amazon Lex Bot](/voice/voice-api/guides/connecting-voice-calls-to-an-amazon-lex-bot)** - This guide will help you to start with an example Dialogflow bot and interact with it from phone calls using provided sample reference codes using Vonage Voice API.
* **[Click to Call](/voice/voice-api/guides/click-to-call)** - Learn how to enable your customers to call you directly from your website.
* **[Call Tracking](/voice/voice-api/guides/call-tracking)** - Keep track of which campaigns are working well by using different numbers for each one and tracking the incoming calls. This tutorial shows you how to handle incoming calls, connect them to another number, and track the phone numbers that called each of your Vonage numbers.
* **[Programmable SIP](/voice/voice-api/guides/programmable-sip)** - Use Vonage SIP to forward inbound and send outbound Voice calls that use the Session Initiation Protocol.
* **[Voice Mail using .NET](/voice/voice-api/guides/voice-mail)** - Learn how to record audio from inbound calls using .NET and the Vonage Voice API.
* **[Call Recording Transcription](/voice/voice-api/guides/transcribe-amazon-api)** - This tutorial shows you how to use the Amazon Transcribe API to transcribe a phone conversation recorded with the Vonage Voice API.
* **[Agent Workplace](/voice/voice-api/guides/agent-workplace)** - How to allow your web app to receive an inbound phone call.
* **[Voice Journey](/voice/voice-api/guides/voice-journey)** - This tutorial shows you how to add programmable assistance to your contact center.
* **[Speech Recognition](/voice/voice-api/guides/asr)** - Capture user input by converting user speech to text form during a call.
* **[DTMF](/voice/voice-api/guides/dtmf)** - Capture user input by detecting DTMF tones (button presses) during a call.
* **[Contact Center Intelligence](/voice/voice-api/guides/cci)** - Learn how to enhance your contact center solution with Voice API.
* **[Fraud Prevention](/voice/voice-api/guides/fraud-prevention)** - Protect your Voice API applications from fraud.

## Code Snippets

```code_snippet_list
product: voice/voice-api
```

## Use Cases

```use_cases
product: voice/voice-api
```

## Reference

* [Voice API Reference](/api/voice)
* [NCCO Reference](/voice/voice-api/ncco-reference)
* [Webhook Reference](/voice/voice-api/webhook-reference)
