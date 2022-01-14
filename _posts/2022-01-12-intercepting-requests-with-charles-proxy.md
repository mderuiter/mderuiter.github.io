---
layout: post
title: Intercepting requests with Charles Proxy
author: Milan de Ruiter
tags: Charles Debugging Tool
---

> Charles is an HTTP proxy / HTTP monitor / Reverse Proxy that enables a developer to view all of the HTTP and SSL / HTTPS traffic between their machine and the Internet. This includes requests, responses and the HTTP headers (which contain the cookies and caching information). 

For iOS developers or QA engineers Charles can be used to view and modify all the traffic that the coming from the app, but also send to the app. It is in one of the main utility applications when debugging and fixing our network requests. Charles is working on iOS Simulators and on physical devices. I like to use the iOS Simulator, so I do not have to switch to another device. In this post we will talk about how to intercept requests on the iOS Simulator.

### How to setup Charles Proxy.
The first thing you need to do is of course, to download Charles Proxy. Charles Proxy can be downloaded from [their website](https://www.charlesproxy.com/download). 

### Install Root Certificate
After you have installed Charles and opened the app, you will have to install the Root Certificate to the simulator. To do so, in Charles menu bar, go to: Help > SSL Proxying > Install Charles Root Certificate in iOS Simulators. Charles will prompt you that the certificates are installed. It also tells you that if the traffic is not coming in, you have to reset your simulator. 
![Charles macOS proxy](/assets/intercepting-requests-with-charles-proxy/charles-install-certificates.png)

> Tip: Reset your simulator without even checking is traffic is coming in.

### Enable macOS Proxy
To listen to all traffic that goes through your machine, you have to make sure that the macOS Proxy is enabled. In Charles menu bar, go to: Proxy > maxOS Proxy. 
![Charles macOS proxy](/assets/intercepting-requests-with-charles-proxy/charles-macos-proxy.png)

### Start recording
Once the Root Certificate is installed and macOS Proxy is enabled, you can start recording traffic. The record button, not to be confused with the 'enable breakpoints' button is the second button.
![Charles record button](/assets/intercepting-requests-with-charles-proxy/charles-record-button.png)

After pressing the record button, you will see a lot of requests coming in. Most of them will not be related to your iOS Simulator. This is because we are not filtering the requests for the simulator.

### SSL Proxy Setting
By default the SSL Proxy Setting is enabled for everything. Which means that all applications that use certificate pinning will be stop working. To limit the impact on the other apps running on your machine you can include and exclude locations. In Charles menu, go to: Proxy > SSL Proxy Settings and add the locations that should be proxied. As I am working for ABN AMRO, I will set the location to the website of ABN AMRO.
![Charles SSL proxy settings](/assets/intercepting-requests-with-charles-proxy/charles-ssl-proxy-settings.png)

### Tips and Tricks
Since a lot of apps will be making network request during your debug session, you can use the filter to filter your requests. 

Another tip is to use the breakpoints feature in Charles. This enables you to modify the request and the response of any requests. To configure breakpoints you go to Charles menu bar, Proxy > Breakpoint settings. In the Breakpoint Setting window you can configure breakpoints for specific requests or responses. Once your breakpoint is hit, Charles will show an window where you can change the header and the body of the requests!
![Charles breakpoint settings](/assets/intercepting-requests-with-charles-proxy/charles-breakpoint-settings.png)