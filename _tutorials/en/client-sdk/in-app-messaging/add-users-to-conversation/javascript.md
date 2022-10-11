---
title: Add users to the conversation
description: Add your two new users as Conversation members
---

# Add Users to the Conversation

You must now add your [Users](/conversation/concepts/user) as [Members](/conversation/concepts/member) of the [Conversation](/conversation/concepts/conversation) using Vonage CLI. 
To add `Alice` to the conversation replace `CONVERSATION_ID` in the command below with your conversation ID generated previously (`CON-...`) and `ALICE_USR_ID` with the ID generated (`USR-`) in the previous step and run the command:

> To use Vonage CLI Conversation commands you need the Conversations plugin to be installed. If you have not done so already, it can be installed by running the following command: `vonage plugins:install @vonage/cli-plugin-conversations`

```sh
vonage apps:conversations:members:add CONVERSATION_ID ALICE_USER_ID
```

The output is ID of the Member:

```
Member added: MEM-aaaaaaa-bbbb-cccc-dddd-0123456789ab
```

Now you need to add the second user, `Bob` to the Conversation. Similarly, replace `CONVERSATION_ID`, `BOB_USER_ID`, and run the command:

```sh
vonage apps:conversations:members:add CONVERSATION_ID BOB_USER_ID
Member added: MEM-eeeeeee-...
```
