---
title: "Weekly Summary - What I learned"
date: 2020-01-18 00:12:30 +0900
categories: updated
comments: true
---

After a long break(In Japan first week of january companys tends to take a new-year holiday), sat back on my workspace, started digging on new projects related with automations. Here above I want to share what I found interesting:

## [pure][pure] prompt
![pure prompt in WSL Ubuntu](/assets/img/pure-prompt.png)
Short description: The most simple prompt theme for zsh. I've been using [agnoster][agnoster] theme for months, but I found it quite heavy especially with heavy git repositories(actually it's not just a problem of the theme, but whole zsh with git repository display...), and wanted to fix this problem and more, use the most simple prompt without any fancy font with icons included. (As I have multiple develop environments)
Installation was very simple with npm (npm i -g prompt-pure), and just added few shell script on .zshrc to run <code>prompt pure</code> every time launching zsh. I just used for few days but I'm satisfied with its simplicity.

## [phpPresentation][phpPresentation]
This unpolished gem was one of few tools that allow PHP to build Microsoft Office XML document from zero. Of course still lots of objects are not made and had to route through other modules to accomplish what I intended, but what I appreciate from this module is how 'well' the whole structure was built, and very easy to follow every class and understand the manipulation of code.
To be honest, I found few limitations that are critical to accomplish my mission, I might migrate whole system to PowerShell. OpenXML document manipulation takes to much dedication, and instead if you're able to automate PowerPoint itself(with COM interop objects) you can do further things such as running VB Script or Macro to allow real time data update. But PhpPresentation gave me the insight to go further with Document automation and I really appreciate that.

## Next week's challenge
We're trying to implement new tools such as AWX(Open source Ansible Tower), Guacamole, gitlab and other tools in order to audit running KUSANAGI services over various Clouds. Other colleagues already started to build scripts on each applications, and my job is to bind them as a single service. My main concern is the scalability both in usages and specifications, therefore I will try to refactor bash sccripts into Python, and regulate dependency to make each modules works in any envrionments.


[pure]: https://github.com/sindresorhus/pure/
[agnoster]: https://github.com/agnoster/agnoster-zsh-theme
[phpPresentation]: https://github.com/PHPOffice/PHPPresentation
