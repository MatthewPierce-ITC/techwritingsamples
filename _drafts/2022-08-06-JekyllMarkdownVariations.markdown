---
layout: post
title:  "Markdown Variations for Static Assets When Using Jekyll"
date:   2022-08-06 08:09:59 -   https://matthewpierce-ITC.github.it/clipcreatordocs
categories: How To
---

### Markdown Variations for Static Assets When Using Jekyll

Markdown proivdes a simplified way to author...

[Image of standard "Jekyll New" directory structure]

For organizational purposes a ***pages*** directory can be added to house the indiex, about, and any supplemental .markdown files.

Images located in assets/images can be added to the index.markdown page easily with the following syntax:

```
![Sample Image](../assets/filename.png)
```

When the site is published to GitHub pages, the HTML is rendered and the image is visible on the page in the browser:

![Rendered sample page with image](assets/images/rendered page)

Jekyll blog posts resited in the _posts directory and have a specific format for their filenames.  This signals [get verbage from tutorial] that these are posts so that they are processed accordingly.  Adding images to these markdown files with the same syntax will result in a broken link error:

![Screen Grab - Broken Link](assets/images/brokenPost.png)

This is because [explain why].  The solution for this problem is to incorporate a bit of Liquid into the markdown for the posts:

```
![Alt Text]({{"/assets/images/filename.png" | relative_url }} )
```

Now when the files are pushed back to GitHub for publishing, the proper HTML is generated to display the linked image for the post.

![Grab Screen - Properly Linked and Rendered Post]("assets/images/filename.png")