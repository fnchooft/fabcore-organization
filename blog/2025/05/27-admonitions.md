# Admonitions

---
description: "Admonitions are formatted, often-colored, and icon-tagged text blocks in documentation used to highlight specific types of information—notes, tips, warnings, or dangers—to improve readability and highlight critical content. They are commonly used in Markdown, Docusaurus, and AsciiDoc, appearing as blockquotes that separate important or cautious warnings from main content."
tags:
 - admonition
 - formatting
---

You need to use hints... not very pretty... - but it works.

The following code:

```bash
{% hint style="info" %}
**Info hints** are great for showing general information, or providing tips and tricks.
{% endhint %}

{% hint style="success" %}
**Success hints** are good for showing positive actions or achievements.
{% endhint %}

{% hint style="warning" %}
**Warning hints** are good for showing important information or non-critical warnings.
{% endhint %}

{% hint style="danger" %}
**Danger hints** are good for highlighting destructive actions or raising attention to critical information.
{% endhint %}
```

Generates:

{% hint style="info" %}
**Info hints** are great for showing general information, or providing tips and tricks.
{% endhint %}

{% hint style="success" %}
**Success hints** are good for showing positive actions or achievements.
{% endhint %}

{% hint style="warning" %}
**Warning hints** are good for showing important information or non-critical warnings.
{% endhint %}

{% hint style="danger" %}
**Danger hints** are good for highlighting destructive actions or raising attention to critical information.
{% endhint %}
