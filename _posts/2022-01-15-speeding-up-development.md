---
layout: post
title: Speeding up your development workflow
author: Milan de Ruiter
tags: ["Tools", "xcrun", "Alfred", "Git", "Xcode Templates", "Tips & Tricks"]
---

The faster we write code, the faster our feature can be delivered, right? Well, I don’t agree with this statement as writing code faster can also introduce bugs that take you more time to investigate and solve. 

So, how can you speed up your development process without having the chance you are going to introduce bugs or miss edge case scenarios?

There are actually a few topics to speed up your development workflow, below I will provide a short summary for all the tips and tricks I use to speed up my development workflow. In other separate posts I will do a deep dive on the specific topic.

### Xcode templates
Apple already provides quite some template for you. The default Swift template for example already adds an copyright header and imports Foundation. But, do you know that you can also make your own templates? If you are building features in a specific architecture you can make a template that already has some basic functionality in it. This way you write it once in your template and reuse it in every other features. Less typing, more code!

![xcode template](/assets/speeding-up-development/template-example.png)

### Aliases 
If you have to use a terminal to run scripts ([Fastlane](https://fastlane.tools/), [Git](https://git-scm.com/), etc), you most of the time first need to navigate to your project directory. I have watched people navigating to their project in the terminal and it look literally minutes. After every `cd` they had to do a `ls` and see which directory the need to go. Having spaces in the name made it even harder. My tip for people who have to navigate to the same directory (your project directory) every now and then is to make an alias in your `~/.zshrc`.

### Git client
Use a Git client that you like. You can execute git commands via the terminal, but there are people who like a GUI for their Git commands. My main reason why I prefer to use a git client instead if the terminal is that the diffs are displayed better in my opinion. My favorite git client is [Fork](https://git-fork.com/). It is simple and easy to use. Alternatives are SourceTree or GitKraken. The reason why I like to use Fork is that they have a command panel. By just pressing CMD + P, it opens up an window where I can type my action (e.g. pull, push, create branch, create tag, etc). It doesn’t sounds like this feature adds a lot, but for me it does as I no longer have to touch my mouse/trackpad in most of the cases.

### Xcrun
Learn how to work with the xcrun commands provided by Apple. Xcrun provides you a lot of actions you can do to communicate with your simulator. For example you can take screenshots, record videos, open deeplinks, etc. If you do not like to use the xcrun directly there is also this tool called [RocketSim](https://www.rocketsim.app/) that is created by [Antoine van der Lee](https://www.avanderlee.com/) that helps you with those kind of utilities!

### Code snippets
Code snippets is also a easy way to automate your development process. The benefits are similar to the ones for Xcode templates. You can create code snippets for everything you like. One of my code snippets is to add a todo. When I type todo (most of the type I only type ‘to’) it automatically suggest my todo code snippet. 
![todo snippet](/assets/speeding-up-development/todo-snippet.png)

### Alfred
By far my most used app. [Alfred](https://www.alfredapp.com/) is a replacement for the native spotlight functionality on your Mac. It is basically spotlight on steroids! Alfred can help you to automate workflows, open specific websites with aliases, run commands and it even provides you a easy to use clipboard history.

Next to the tricks/tools I have mentioned above I want to spotlight one final combination of apps/tools that I am using. This combination is [iTerm](https://iterm2.com/) with [oh-my-zsh](https://ohmyz.sh/). This combination helped me to enjoy working with terminals. Where I was scared to work with the terminal on the pst, I now enjoy working with it. It helped me learn to do things with the command line. 

A final note, where a lot of tools can help you to speed up your development and work flow, I would also highlight that you should never depend just on those tools. Sometimes you might need to work on a machine that doesn’t have those tools installed or that the tool is no longer maintained and it stopped working.
