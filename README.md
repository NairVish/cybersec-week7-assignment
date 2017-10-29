# Project 7 - WordPress Pentesting

Time spent: **5.5** hours spent in total

> Objective: Find, analyze, recreate, and document **3-5 vulnerabilities** affecting an old version of WordPress

**This README conforms to CodePath's template. See [here](https://github.com/NairVish/cybersec-week7-assignment/blob/master/README_extended.md) for my own extended README.**

## Pentesting Report

1. **CVE 2015-5622: Authenticated Stored Cross-Site Scripting (XSS)**
  - [ ] Summary: 
    - Vulnerability types: XSS
    - Tested in version: 4.1
    - Fixed in version: 4.2.3
  - [ ] GIF Walkthrough: (Video) https://youtu.be/UWHUU_pFlb8
  - [ ] Steps to recreate: https://github.com/NairVish/cybersec-week7-assignment/blob/master/README_extended.md#recreating-the-exploit
  - [ ] Affected source code:
    - [Link 1](https://core.trac.wordpress.org/changeset/33359)
2. **CVE 2015-5714: Authenticated Shortcode Tags Cross-Site Scripting (XSS)**
  - [ ] Summary: 
    - Vulnerability types: XSS
    - Tested in version: 4.0
    - Fixed in version: 4.3.1
  - [ ] GIF Walkthrough: (Video) https://youtu.be/q9XJsXixbcE
  - [ ] Steps to recreate: https://github.com/NairVish/cybersec-week7-assignment/blob/master/README_extended.md#recreating-the-exploit-1
  - [ ] Affected source code:
    - [Link 1](https://github.com/WordPress/WordPress/commit/f72b21af23da6b6d54208e5c1d65ececdaa109c8)
3. **CVE 2017-6817: Authenticated Stored Cross-Site Scripting (XSS) in YouTube URL Embeds**
  - [ ] Summary: 
    - Vulnerability types: XSS
    - Tested in version: 4.0
    - Fixed in version: 4.7.3
  - [ ] GIF Walkthrough: (Video) https://youtu.be/YuqVKOiPfTU
  - [ ] Steps to recreate: https://github.com/NairVish/cybersec-week7-assignment/blob/master/README_extended.md#recreating-the-exploit-2
  - [ ] Affected source code:
    - [Link 1](https://github.com/WordPress/WordPress/commit/419c8d97ce8df7d5004ee0b566bc5e095f0a6ca8)

## Resources

- [WordPress Source Browser](https://core.trac.wordpress.org/browser/)
- [WordPress Developer Reference](https://developer.wordpress.org/reference/)

Videos created with [GIPHY Capture](https://giphy.com/apps/giphycapture).

## Notes

We used WPScan (running on a VM running Kali Linux) to scan a WordPress instance (deployed using Vagrant via WPDistillery) to deploy and scan various versions of WP for vulnerabilities. We then picked a few of these exploits and recreated the steps necessary to execute them ourselves. The main challenge came from the fact that many of the vulnerabilities listed by WPScan did not contain explicit information on how to carry out the exploit (rather they were just general announcements on the existence of each vulnerability).

## License

    Copyright 2017 Vishnu Nair

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

        http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
