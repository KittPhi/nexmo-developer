---
title: Send an Image Message
meta_title: Send an image message on WhatsApp using the Messages API
---

# Send an Image Message

In this code snippet you learn how to send a WhatsApp image message using the Messages API. For WhatsApp the maximum outbound media size is 64MB.

> **IMPORTANT:** If a customer has not messaged you first, then the first time you send a message to a user, WhatsApp requires that the message contains a template. This is explained in more detail in the [Understanding WhatsApp topic](/messages/concepts/whatsapp).

## Example

Find the description for all variables used in each code snippet below:

```snippet_variables
- VONAGE_APPLICATION_ID
- VONAGE_APPLICATION_PRIVATE_KEY_PATH
- VONAGE_PRIVATE_KEY_PATH
- BASE_URL.MESSAGES
- MESSAGES_API_URL
- WHATSAPP_NUMBER
- VONAGE_WHATSAPP_NUMBER
- VONAGE_NUMBER.WHATSAPP
- TO_NUMBER.MESSAGES
- IMAGE_URL
- IMAGE_CAPTION
```

> **NOTE:** Don't use a leading `+` or `00` when entering a phone number, start with the country code, for example, 447700900000.

```code_snippets
source: '_examples/messages/whatsapp/send-image'
application:
  type: messages
  name: 'Send a WhatsApp image'
```

## Try it out

When you run the code a WhatsApp image message is sent to the destination number.
