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

Take a look around the site on `http://10.10.127.86/` — we will be using this a lot throughout the module. Visit every other page that is linked on the homepage, then check your sitemap — one endpoint should stand out as being very unusual!

Visit this in your browser (or use the "Response" section of the site map entry for that endpoint)

### What is the flag you receive after visiting the unusual endpoint?

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/130cedca-7e1d-41fd-97f0-302ef3818b17)

`THM{NmNlZTliNGE1MWU1ZTQzMzgzNmFiNWVk}`

## The Burp Suite Browser

If the previous tasks seemed overly complex, rest assured, this topic will be a lot simpler.

In addition to modifying our regular web browser to work with the proxy, Burp Suite also includes a built-in Chromium browser that is pre-configured to use the proxy without any of the modifications we just had to do.

To start the Burp Browser, click the `Open Browser` button in the proxy tab. A Chromium window will pop up, and any requests made in this browser will go through the proxy.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9c93553c-9bcf-49a8-afd1-7fbc54cedeeb)

**Note**: There are many settings related to the Burp Browser in the project options and user options settings. Make sure to explore and customise them as needed.

However, if you are running Burp Suite on Linux as the root user (as is the case with the AttackBox), you may encounter an error preventing the Burp Browser from starting due to the inability to create a sandbox environment.

There are two simple solutions to this:

- **Smart option**: Create a new user and run Burp Suite under a low-privilege account to allow the Burp Browser to run without issues.

- **Easy option**: Go to `Settings -> Tools -> Burp's browser` and check the `Allow Burp's browser to run without a sandbox` option. Enabling this option will allow the browser to start without a sandbox. However, please be aware that this option is disabled by default for security reasons. If you choose to enable it, exercise caution, as compromising the browser could grant an attacker access to your entire machine. In the training environment of the AttackBox, this is unlikely to be a significant issue, but use it responsibly.

## Scoping and Targeting

Finally, we come to one of the most important aspects of using the Burp Proxy: **Scoping**.

Capturing and logging all of the traffic can quickly become overwhelming and inconvenient, especially when we only want to focus on specific web applications. This is where scoping comes in.

By setting a scope for the project, we can define what gets proxied and logged in Burp Suite. We can restrict Burp Suite to target only the specific web application(s) we want to test. The easiest way to do this is by switching to the `Target` tab, right-clicking on our target from the list on the left, and selecting `Add To Scope`. Burp will then prompt us to choose whether we want to stop logging anything that is not in scope, and in most cases, we want to select `yes`.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/bc9728ab-cb55-4ebe-ba49-5f88f139abf9)

To check our scope, we can switch to the **Scope settings** sub-tab within the **Target** tab.

The Scope settings window allows us to control our target scope by including or excluding domains/IPs. This section is powerful and worth spending time getting familiar with.

However, even if we disabled logging for out-of-scope traffic, the proxy will still intercept everything. To prevent this, we need to go to the **Proxy settings** sub-tab and select `And` `URL` `Is in target scope` from the "Intercept Client Requests" section.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7a4024fa-6eec-4530-b209-6f0d17f2380a)

Enabling this option ensures that the proxy completely ignores any traffic that is not within the defined scope, resulting in a cleaner traffic view in Burp Suite.

## Proxying HTTPS

**Note**: The AttackBox is already configured to solve the problem posed in this task. If you use the AttackBox and don't wish to read through the information here, you can skip to the next task.

When intercepting HTTP traffic, we may encounter an issue when navigating to sites with TLS enabled. For example, when accessing a site like `https://google.com/`, we may receive an error indicating that the PortSwigger Certificate Authority (CA) is not authorised to secure the connection. This happens because the browser does not trust the certificate presented by Burp Suite.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/041bb3d1-e556-44b7-810d-ca3fc1f2bd65)

To overcome this issue, we can manually add the PortSwigger CA certificate to our browser's list of trusted certificate authorities. Here's how to do it:

- **Download the CA Certificate**: With the Burp Proxy activated, navigate to http://burp/cert. This will download a file called `cacert.der`. Save this file somewhere on your machine.

- **Access Firefox Certificate Settings**: Type `about:preferences` into your Firefox URL bar and press **Enter**. This will take you to the Firefox settings page. Search the page for "certificates" and click on the **View Certificates** button.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/9b48e246-525f-4bfe-8a0b-66928a7c3a6f)

- **Import the CA Certificate**: In the Certificate Manager window, click on the Import button. Select the `cacert.der` file that you downloaded in the previous step.

- **Set Trust for the CA Certificate**: In the subsequent window that appears, check the box that says "Trust this CA to identify websites" and click OK.

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/7619472e-1c68-4708-ac57-26880a70e153)

By completing these steps, we have added the PortSwigger CA certificate to our list of trusted certificate authorities. Now, we should be able to visit any TLS-enabled site without encountering the certificate error.

You can watch the following video for a visual demonstration of the full certificate import process:

By following these instructions, you can ensure that your browser trusts the PortSwigger CA certificate and securely communicates with TLS-enabled websites through the Burp Suite Proxy.

## Example Attack

Having looked at how to set up and configure our proxy, let's go through a simplified real-world example.

We will start by taking a look at the support form at `http://10.10.127.86/ticket/`:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/fda0879c-aee4-4c21-91c6-54947d9d0d74)

In a real-world web app pentest, we would test this for a variety of things, one of which would be Cross-Site Scripting (or XSS). If you have not yet encountered XSS, it can be thought of as injecting a client-side script (usually in Javascript) into a webpage in such a way that it executes. There are various kinds of XSS – the type that we are using here is referred to as "Reflected" XSS, as it only affects the person making the web request.

### Walkthrough

Try typing: `<script>alert("Succ3ssful XSS")</script>`, into the "Contact Email" field. You should find that there is a client-side filter in place which prevents you from adding any special characters that aren't allowed in email addresses 

Fortunately for us, client-side filters are absurdly easy to bypass. There are a variety of ways we could disable the script or just prevent it from loading in the first place.

Let's focus on simply bypassing the filter for now.

First, make sure that your Burp Proxy is active and that intercept is on.

Now, enter some legitimate data into the support form. For example: "pentester@example.thm" as an email address, and "avasdsad" as a query.

Submit the form — the request should be intercepted by the proxy.

With the request captured in the proxy, we can now change the email field to be our very simple payload from above: `<script>alert("Succ3ssful XSS")</script>`. After pasting in the payload, we need to select it, then URL encode it with the `Ctrl + U` shortcut to make it safe to send. This process is shown in the GIF below:

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/29f2669b-5e55-43d4-aab5-ac416614cd0c)

Finally, press the "Forward" button to send the request.

You should find an alert box from the site indicating a successful XSS attack!

![image](https://github.com/zs0b/zs0b.github.io/assets/118095276/a51548e8-9c1e-4d05-8966-0c5dd074a579)











































