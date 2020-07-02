---
date: "2020-07-02"
title: "C# 9 Deep Dive: Records"
excerpt: In a C# 9 deep dive, we will first walk through init-only features.
tags: [csharp]
header:
    overlay_image: /assets/images/records.png
    overlay_filter: 0.8
---

In the previous post, we discussed the init-only features of C# 9, which allowed you to make individual properties immutable. That works great on a case-by-case basis, but the real power in this functionality is when you can do this for objects. This is where records shine, and will be the focus of this post.

This is the second post in a five-post series on C# 9 features:

- Post 1 - [Init-only features](https://daveabrock.com/2020/06/29/c-sharp-9-deep-dive-inits)
- Post 2 (*this post*) - Records
- Post 3 (*future post*) - Pattern matching
- Post 4 (*future post*) - Target typing and covariant returns
- Post 5 (*future post*) - Putting it all together: an all-in-one application

**Heads up!** C# 9 is still in preview mode, so much of this content might change. I will do my best to update it as I come across it, but that is not guaranteed. Have fun, but your experience may vary.
{: .notice--danger}

## What's next
