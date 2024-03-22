---
layout: post
title: Burp Suite The Basics Part 2 
date: 2024-03-22 20:00 +0700
categories: [TryHackMe, Complete Beginner]
tags: [Blue Team, Complete Beginner]     # TAG names should always be lowercase
img_path: '/assets/THM'
image: 
  path: logo.png
--- 

## Introduction to the Burp Proxy

The Burp Proxy is a fundamental and crucial tool within Burp Suite. It enables the capture of requests and responses between the user and the target web server. This intercepted traffic can be manipulated, sent to other tools for further processing, or explicitly allowed to continue to its destination.

Key Points to Understand About the Burp Proxy

- **Intercepting Requests**: When requests are made through the Burp Proxy, they are intercepted and held back from reaching the target server. The requests appear in the Proxy tab, allowing for further actions such as forwarding, dropping, editing, or sending them to other Burp modules. To disable the intercept and allow requests to pass through the proxy without interruption, click the `Intercept is on` button.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/528ed4c1-4ac5-41d7-b7fb-ef6f55406c7f)

- **Taking Control**: The ability to intercept requests empowers testers to gain complete control over web traffic, making it invaluable for testing web applications.

- **Capture and Logging**: Burp Suite captures and logs requests made through the proxy by default, even when the interception is turned off. This logging functionality can be helpful for later analysis and review of prior requests.

- **WebSocket Support**: Burp Suite also captures and logs WebSocket communication, providing additional assistance when analysing web applications.

- **Logs and History**: The captured requests can be viewed in the **HTTP history** and **WebSockets history** sub-tabs, allowing for retrospective analysis and sending the requests to other Burp modules as needed.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7c051766-d256-45a8-9758-69f893bd567f)

Proxy-specific options can be accessed by clicking the **Proxy settings** button. These options provide extensive control over the Proxy’s behaviour and functionality. Familiarise yourself with these options to optimize your Burp Proxy usage.

Some Notable Features in the Proxy Settings

- **Response Interception**: By default, the proxy does not intercept server responses unless explicitly requested on a per-request basis. The "Intercept responses based on the following rules" checkbox, along with the defined rules, allows for a more flexible response interception.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/e6a78676-4526-4273-93ad-3420e25b40d4)

- **Match and Replace**: The "Match and Replace" section in the **Proxy settings** enables the use of regular expressions (regex) to modify incoming and outgoing requests. This feature allows for dynamic changes, such as modifying the user agent or manipulating cookies.

Take the time to explore and experiment with the Proxy options, as this will enhance your understanding and proficiency with the tool.

## Connecting through the Proxy (FoxyProxy)

Start the machine by clicking the **Start Machine** button at the upper right corner of this task.

To use the Burp Suite Proxy, we need to configure our local web browser to redirect traffic through Burp Suite. In this task, we will focus on configuring the proxy using the FoxyProxy extension in Firefox.

Please note that the instructions provided are specific to Firefox. If you are using a different browser, you may need to find alternative methods or use the TryHackMe AttackBox.

Here are the steps to configure the Burp Suite Proxy with FoxyProxy:

- **Install FoxyProxy**: Download and install the [FoxyProxy Basic extension](https://addons.mozilla.org/en-US/firefox/addon/foxyproxy-basic/).

**Note: FoxyProxy is already installed on the AttackBox.**

- **Access FoxyProxy Options**: Once installed, a button will appear at the top right of the Firefox browser. Click on the FoxyProxy button to access the FoxyProxy options pop-up.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/4127d82c-a01d-4169-a5c1-fd33920b8634)

- **Create Burp Proxy Configuration**: In the FoxyProxy options pop-up, click the **Options** button. This will open a new browser tab with the FoxyProxy configurations. Click the **Add** button to create a new proxy configuration.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/cc140ad7-ba32-411c-b5ae-c6df29fcbb21)

- **Add Proxy Details**: On the "Add Proxy" page, fill in the following values:

  - Title: `Burp` (or any preferred name)

  - Proxy IP: `127.0.0.1`

  - Port: `8080`

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/0e540ccc-7ab3-473c-b244-3cbbdc049017)

- **Save Configuration**: Click **Save** to save the Burp Proxy configuration.

- **Activate Proxy Configuration**: Click on the FoxyProxy icon at the top-right of the Firefox browser and select the `Burp` configuration. This will redirect your browser traffic through `127.0.0.1:8080`. Note that Burp Suite must be running for your browser to make requests when this configuration is activated.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bad3b22c-ff10-4563-bd16-410dd1131152)

- **Enable Proxy Intercept in Burp Suite**: Switch to Burp Suite and ensure that Intercept is turned on in the **Proxy** tab.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/abc23324-5478-41bf-b471-0adad2d9cbea)

- **Test the Proxy**: Open Firefox and try accessing a website, such as the homepage for `http://MACHINE_IP/`. Your browser will hang, and the proxy will populate with the HTTP request. Congratulations, you have successfully intercepted your first request!

### Remember the following:

- When the proxy configuration is active, and the intercept is switched on in Burp Suite, your browser will hang whenever you make a request.

- Be cautious not to leave the intercept switched on unintentionally, as it can prevent your browser from making any requests.

- Right-clicking on a request in Burp Suite allows you to perform various actions, such as forwarding, dropping, sending to other tools, or selecting options from the right-click menu.

Take note of these details as you begin using the Burp Suite Proxy.

**Note**: Consider closing the other tabs in the AttackBox browser before enabling interception, as you will receive some WebSocket requests instead of request from the target VM.

## Site Map and Issue Definitions

The **Target** tab in Burp Suite provides more than just control over the scope of our testing. It consists of three sub-tabs:

- **Site map**: This sub-tab allows us to map out the web applications we are targeting in a tree structure. Every page that we visit while the proxy is active will be displayed on the site map. This feature enables us to automatically generate a site map by simply browsing the web application. In Burp Suite Professional, we can also use the site map to perform automated crawling of the target, exploring links between pages and mapping out as much of the site as possible. Even with Burp Suite Community, we can still utilize the site map to accumulate data during our initial enumeration steps. It is particularly useful for mapping out APIs, as any API endpoints accessed by the web application will be captured in the site map.

- **Issue definitions**: Although Burp Community does not include the full vulnerability scanning functionality available in Burp Suite Professional, we still have access to a list of all the vulnerabilities that the scanner looks for. The **Issue definitions** section provides an extensive list of web vulnerabilities, complete with descriptions and references. This resource can be valuable for referencing vulnerabilities in reports or assisting in describing a particular vulnerability that may have been identified during manual testing.

- **Scope settings**: This setting allows us to control the target scope in Burp Suite. It enables us to include or exclude specific domains/IPs to define the scope of our testing. By managing the scope, we can focus on the web applications we are specifically targeting and avoid capturing unnecessary traffic.

Overall, the **Target** tab offers features beyond scoping, allowing us to map out web applications, fine-tune our target scope, and access a comprehensive list of web vulnerabilities for reference purposes.

Challenge

Take a look around the site on `http://10.10.67.41/` — we will be using this a lot throughout the module. Visit every other page that is linked on the homepage, then check your sitemap — one endpoint should stand out as being very unusual!

Visit this in your browser (or use the "Response" section of the site map entry for that endpoint)




































































