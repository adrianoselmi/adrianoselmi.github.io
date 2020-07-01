---
layout: post
title:  "Power Automate: Lookup column throws error 'data type is not supported'"
featured_image: /images/sidebar/IMG_3431.jpg
date:   2020-06-29 09:30
category: Power Automate
tag: Trigger Error
subtitle: There's a problem with the flow's trigger.
excerpt: "There's a problem with the flow's trigger. The required field Lookup Column data type is not supported"
---

## Issue:

A flow was created to run whenever a new item was created or edited. The flow did not run at all and instead advised that there is an issue with the flow's trigger.

Trigger image
![There's a problem with the flow's trigger](/images/20200629/trigger_error.png "There's a problem with the flow's trigger")

Clicking on 'Fix the trigger' displays the flow with an error on the trigger.

![Flow history](/images/20200629/bad_request.png "Flow history")

The following error message is displayed:

```json
{
  "status": 400,
  "message": "The required field \"Related risk(s)\" data type is not supported\r\nclientRequestId: 7e29a495-ddab-4c6c-8af7-0b73ca51381a\r\nserviceRequestId: 7e29a495-ddab-4c6c-8af7-0b73ca51381a"
}
```

## Resolution

The lookup column value was using the Title (linked to item) field. Changing this to normal Title column resolved the issue. 

![Lookup column](/images/20200629/lookup_column.png "Lookup column")

The error message remained in place for a while but workflows started running correctly.

![Workflow succeeded](/images/20200629/succeeded.png "workflow succeeded")
