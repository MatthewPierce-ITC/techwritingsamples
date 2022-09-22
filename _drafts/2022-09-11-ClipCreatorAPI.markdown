---
layout: post
title:  "Clip Creator API"
date:   2022-09-11 08:46:00  -0400
categories: User Manuals
---

The Clip Creator Web API uses REST endpoints to provide you with the validation and clip merging tools available in the desktop version of the application.

For basic format validation, a Clip Object is required:

```
{
    begPage: 12
    begLine: 24
    endPage: 14
    endLine: 07
}
```
The return body of this POST operation contains a single boolean value.  True for valid, false for invalid.


To check a Validated clip against a collection of Validated clips, an Insert Clip Object is required.  This contains the Validated clip, and a collection of Validated clips.

```
{
    clip:{
            begPage: 12
            begLine: 24
            endPage: 14
            endLine: 07
        }
    existingClips:[
        clip:{
            begPage: 12
            begLine: 24
            endPage: 14
            endLine: 07
        },
        clip:{
            begPage: 06
            begLine: 12
            endPage: 07
            endLine: 03
        },
        clip:{
            begPage: 19
            begLine: 10
            endPage: 19
            endLine: 21
        }]
}
```

If there are no errors detected (duplicate range, subset range, superset range, or overalpping range), the new collection with the inserted clip is returned.

If there are errors detected, the response body contains the proposed clip, conflicting clip, and a string with the error type.

