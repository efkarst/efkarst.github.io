---
layout: post
title:      "How Sinatra works with the web"
date:       2018-11-29 04:22:56 +0000
permalink:  how_sinatra_works_with_the_web
---


### How the Web Works

At the most basic level, the internet consists of requests to and responses between clients and servers. **Clients** are the end-users devices (like a phone or laptop) accessing web content (typically through a browser). **Servers** are computers that store webpages, sites or apps. When a client requests access to a web page, the server dynamically generates and sends a copy of the web page as a response (along with component files including code, images, music, videos, PDFs, or whatever assets are needed). The user's browser then renders the copy of the web page for the user.

If there were only a few devices in the world this would be a fairly simple theory. However, since there are billions of devices and websites (with more added every day) it's not possible for every browser on every device to know where every website is stored. Thus, a more complex system of **nodes** (connecting points along a route of traffic) were created to handle web requests and responses. Clients, servers and nodes are connected via set of **protocols** (common rules and language for machines to transmit the data that makes the internet work). 

**Hyper Text Transfer Protocol (HTTP)** is the standard protocol used to view web sites through a browser. Browsers send HTTP requests to servers to get a web page, and servers send HTTP responses back to browsers with everything the browser needs to render a web page for the user. **Transmission control protocol (TCP)** and the **Internet Protocol (IP** - each computer has a unique IP identifier) are used to transmit messages and retrieve information across computers. This is done via hardware like **routers** and **Domain Name Servers (DNS)** that help identify the IP address of the server a web page is stored on so the client can send an HTTP request to that server directly. The server sends a response back to the client in the form of small **packets**. Breaking a response into smaller pieces allows the response to travel back over multiple paths in parallel, ultimately speeding up the response time.


### Sinatra Request and Response Cycle
Let's take a more detailed look at how a typical HTTP request and response cycle works for a Sinatra web app. The diagram below details how the internet routes an HTTP request from a client to the appropriate web server, dynamically generates a web page with a Sinatra app on that web server, and sends the HTTP response back to the client.

![HTTP Request and Response Cycle for a Sinatra App](https://gorvog.bn.files.1drv.com/y4me4USMf7sR_Wn0crnLWSvkxAXUjuEtwxsJjimnNki6lYLj1Lt1jsMpri-XIgLYo2VhVJcr-STyEVBXbi14BC2OJsPacW8TwEcWqByKhGQDZJsG88dyAaGQoLc4NLzaYc1y5GwfKlFFmj6bjFiYcLCy2WmHmv5bAkGmRn0sy0pHapfqC0vhokUxS1RVqMQVIaBNTds1I4iGjFFB0vzkTwuXA?width=794&height=736&cropmode=none)

This looks complex, but with information traveling faster than the speed of light across the world-wide-web all of this happens in milliseconds. The user can enjoy the requested web page nearly instantaneously.



### Sources:
* [Mozilla](https://developer.mozilla.org/en-US/docs/Learn/Getting_started_with_the_web/How_the_Web_works)
* [Learn.co](https://learn.co/tracks/full-stack-web-development-v6/rack/rack-and-the-internet/how-the-internet-works)
* [How Stuff Works](https://computer.howstuffworks.com/internet/basics/internet.html)

