---
title: "How to Detect Browser and OS with Userstack API (2026 Guide)"
url: "https://blog.apilayer.com/how-to-detect-browser-and-os-with-userstack-api-2026-guide/"
date: "Tue, 21 Apr 2026 06:11:13 +0000"
author: "Karam"
feed_url: "https://blog.apilayer.com/feed/"
---
<div class="elementor elementor-21836">
				<div class="elementor-element elementor-element-725bd28 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-374cf91 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><span style="font-weight: 400;">Parsing user agent strings yourself is a nightmare. They’re inconsistent, they change constantly, and your regex will break the moment a new browser version ships. That’s exactly the kind of problem an API should solve.</span></p><p><span style="font-weight: 400;">This guide walks you through using the </span><a href="https://apilayer.com/marketplace/userstack-api"><span style="font-weight: 400;">Userstack API</span></a><span style="font-weight: 400;"> to detect browsers, operating systems, and device types from user agent strings. You’ll get working code examples in Node.js and Python, a real API response breakdown, and practical use cases you can implement today.</span></p><h2><b>Key Takeaways</b></h2><ul><li style="font-weight: 400;"><span style="font-weight: 400;">User agent strings are messy and constantly changing, making DIY regex parsers a maintenance burden that breaks with every browser update.</span></li><li style="font-weight: 400;"><span style="font-weight: 400;">Userstack is a REST API that returns structured JSON with browser, OS, device, and crawler data from any user agent string.</span></li><li style="font-weight: 400;"><span style="font-weight: 400;">Integration takes minutes with a simple GET request, and working code examples in Node.js and Python are copy-paste ready.</span></li><li style="font-weight: 400;"><span style="font-weight: 400;">Practical applications include server-side analytics enrichment, conditional content delivery by device type, and device-segmented A/B testing.</span></li><li style="font-weight: 400;"><span style="font-weight: 400;">The free tier lets you test the API immediately, with paid plans starting at $9.99/month for 50,000 lookups and crawler detection.</span></li></ul>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-1a39c06 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-ca2cfac elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<h2><b>Why User Agent Parsing Is Harder Than You Think</b></h2><p><span style="font-weight: 400;">A user agent string looks something like this:</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-009c273 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-fac43a8 elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-twilight copy-to-clipboard ">
			<pre class="highlight-height language-javascript ">
				<code class="language-javascript" readonly="true">
					Mozilla/5.0 (Windows NT 11.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36
				</code>
			</pre>
		</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-714827d e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-21373bf elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p dir="ltr" style="line-height: 1.38; margin-top: 12pt; margin-bottom: 12pt;"><span style="font-size: 11pt; font-family: Poppins,sans-serif; color: #000000; background-color: transparent; font-weight: 400; font-style: normal; font-variant: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">That single string encodes the browser, OS, device type, and rendering engine. Extracting those values accurately means writing regex patterns for every known browser and OS combination. Then maintaining them as new versions ship weekly.</span></p><p dir="ltr" style="line-height: 1.38; margin-top: 12pt; margin-bottom: 12pt;"><span style="font-size: 11pt; font-family: Poppins,sans-serif; color: #000000; background-color: transparent; font-weight: 400; font-style: normal; font-variant: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">The problem has gotten worse. Google has been progressively reducing the information in Chrome’s user agent string through its User-Agent Reduction initiative. Minor version numbers are frozen, OS version details are stripped, and browsers like Firefox and Safari have followed with similar changes. If your parsing logic depends on specific version substrings, it is already broken.</span></p><p dir="ltr" style="line-height: 1.38; margin-top: 12pt; margin-bottom: 12pt;"><span style="font-size: 11pt; font-family: Poppins,sans-serif; color: #000000; background-color: transparent; font-weight: 400; font-style: normal; font-variant: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">Client Hints (the </span><span style="font-size: 11pt; font-family: 'Roboto Mono',monospace; color: #188038; background-color: transparent; font-weight: 400; font-style: normal; font-variant: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">Sec-CH-UA</span><span style="font-size: 11pt; font-family: Poppins,sans-serif; color: #000000; background-color: transparent; font-weight: 400; font-style: normal; font-variant: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;"> header family) are the intended replacement, but Safari doesn’t support them and older browsers ignore them. Server-side applications processing logs from mixed traffic still need to parse traditional UA strings alongside Client Hints. That means two parsing systems instead of one.</span></p><p dir="ltr" style="line-height: 1.38; margin-top: 12pt; margin-bottom: 12pt;"><span style="font-size: 11pt; font-family: Poppins,sans-serif; color: #000000; background-color: transparent; font-weight: 400; font-style: normal; font-variant: normal; text-decoration: none; vertical-align: baseline; white-space: pre-wrap;">This is where a dedicated parsing service earns its keep. You offload the detection problem to an API that keeps its database current.</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-9b1c9c7 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-3bae17d elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><b>What Userstack API Does and How It Works</b></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-7331857 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-abeb1b1 elementor-widget elementor-widget-image">
				<div class="elementor-widget-container">
															<img alt="Userstack API browser and OS detection interface" class="attachment-large size-large wp-image-21838" height="288" src="https://blog.apilayer.com/wp-content/uploads/2026/04/image-1.png" width="512" />															</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-aecbdb0 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-034cf7e elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><a href="https://apilayer.com/marketplace/userstack-api"><span style="font-weight: 400;">Userstack</span></a><span style="font-weight: 400;"> is a REST API that takes a raw user agent string and returns structured JSON (or XML) with parsed data about the browser, operating system, device, and crawler status. It is part of the </span><a href="https://apilayer.com/products/"><span style="font-weight: 400;">APILayer</span></a><span style="font-weight: 400;"> API suite, which hosts dozens of data APIs under a single platform.</span></p><p><span style="font-weight: 400;">The core mechanic is simple. You send a GET request with two parameters: your API access key and the user agent string you want to parse. The API returns a JSON object broken into clearly labeled sections: </span><span style="font-weight: 400;">browser</span><span style="font-weight: 400;">, </span><span style="font-weight: 400;">os</span><span style="font-weight: 400;">, </span><span style="font-weight: 400;">device</span><span style="font-weight: 400;">, and </span><span style="font-weight: 400;">crawler</span><span style="font-weight: 400;">.</span></p><p><span style="font-weight: 400;">Userstack’s detection database is maintained by in-house specialists and updated multiple times per week. This covers new browser versions, device releases, and OS updates without any work on your end. It also identifies crawlers and bots, which is useful if you need to filter non-human traffic from your analytics.</span></p><p><span style="font-weight: 400;">The API supports single lookups and bulk lookups (up to 100 user agent strings per request on paid plans). All requests are encrypted with 256-bit SSL. Let’s set it up.</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-766317c e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-23a19fb elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<h2><b>Getting Started: API Key and First Request</b></h2><h3><b>Step 1: Sign up for an API key.</b></h3><p><span style="font-weight: 400;">Go to the </span><a href="https://apilayer.com/marketplace/userstack-api"><span style="font-weight: 400;">Userstack product page on APILayer</span></a><span style="font-weight: 400;"> and create a free account. After signing up, you’ll find your unique 32-character API access key in your account dashboard.</span></p><h3><b>Step 2: Make your first request with cURL.</b></h3><p><span style="font-weight: 400;">The base endpoint for user agent detection is:</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-ea812d4 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-4de94f9 elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-twilight copy-to-clipboard ">
			<pre class="highlight-height language-javascript ">
				<code class="language-javascript" readonly="true">
					http://api.userstack.com/detect?access_key=YOUR_ACCESS_KEY&amp;ua=USER_AGENT_STRING


				</code>
			</pre>
		</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-0a128a1 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-04285f7 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><span style="font-weight: 400;">Here’s a cURL example using a Chrome on macOS user agent string:</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-d8039ee e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-2192786 elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-twilight copy-to-clipboard ">
			<pre class="highlight-height language-javascript ">
				<code class="language-javascript" readonly="true">
					curl "http://api.userstack.com/detect?access_key=YOUR_ACCESS_KEY&amp;ua=Mozilla%2F5.0%20(Macintosh%3B%20Intel%20Mac%20OS%20X%2010_14_0)%20AppleWebKit%2F537.36%20(KHTML%2C%20like%20Gecko)%20Chrome%2F71.0.3578.98%20Safari%2F537.36"


				</code>
			</pre>
		</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-6ac230c e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-4046fb6 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><span style="font-weight: 400;">Make sure to URL-encode the user agent string. Spaces become </span><span style="font-weight: 400;">%20</span><span style="font-weight: 400;">, semicolons become </span><span style="font-weight: 400;">%3B</span><span style="font-weight: 400;">, and forward slashes become </span><span style="font-weight: 400;">%2F</span><span style="font-weight: 400;">.</span></p><h3><b>Step 3: Read the JSON response.</b></h3><p><span style="font-weight: 400;">Here is the full JSON response returned by the API for the request above:</span></p><p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Ut elit tellus, luctus nec ullamcorper mattis, pulvinar dapibus leo.</p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-118dd13 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-970d639 elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-twilight copy-to-clipboard ">
			<pre class="highlight-height language-javascript ">
				<code class="language-javascript" readonly="true">
					{
  "ua": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_14_0) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/71.0.3578.98 Safari/537.36",
  "type": "browser",
  "brand": "Apple",
  "name": "Mac",
  "url": "https://www.google.com/about/company/",
  "os": {
    "name": "macOS 10.14 Mojave",
    "code": "macos_10_14",
    "url": "https://en.wikipedia.org/wiki/MacOS_Mojave",
    "family": "macOS",
    "family_code": "macos",
    "family_vendor": "Apple Inc.",
    "icon": "https://assets.userstack.com/icons/os/macosx.png",
    "icon_large": "https://assets.userstack.com/icons/os/macosx_big.png"
  },
  "device": {
    "is_mobile_device": false,
    "type": "desktop",
    "brand": "Apple",
    "brand_code": "apple",
    "brand_url": "http://www.apple.com/",
    "name": "Mac"
  },
  "browser": {
    "name": "Chrome",
    "version": "71.0.3578.98",
    "version_major": "71",
    "engine": "WebKit/Blink"
  },
  "crawler": {
    "is_crawler": false,
    "category": null,
    "last_seen": null
  }
}


				</code>
			</pre>
		</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-8a03a09 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-3b6b8ac elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><span style="font-weight: 400;">The response is divided into four clear sections. The </span><span style="font-weight: 400;">os</span><span style="font-weight: 400;"> object gives you the operating system name, version, and family. The </span><span style="font-weight: 400;">browser</span><span style="font-weight: 400;"> object includes the browser name, full version, major version, and rendering engine. The </span><span style="font-weight: 400;">device</span><span style="font-weight: 400;"> object tells you whether it’s mobile, tablet, or desktop, along with the brand and model. The </span><span style="font-weight: 400;">crawler</span><span style="font-weight: 400;"> object flags bots and includes crawler category and last seen timestamp (available on the Basic plan and above).</span></p><p><span style="font-weight: 400;">Now let’s put this into real code.</span></p><h2><b>Working Code Example: Node.js</b></h2><p><span style="font-weight: 400;">This example uses the built-in </span><span style="font-weight: 400;">fetch</span><span style="font-weight: 400;"> API (available in Node.js 18+). No external dependencies required.</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-b8b7a93 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-04e795b elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-twilight copy-to-clipboard ">
			<pre class="highlight-height language-javascript ">
				<code class="language-javascript" readonly="true">
					const API_KEY = "YOUR_ACCESS_KEY";
const USER_AGENT =
  "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/125.0.0.0 Safari/537.36";

async function detectUserAgent(ua) {
  const endpoint = `http://api.userstack.com/detect?access_key=${API_KEY}&amp;ua=${encodeURIComponent(ua)}`;

  try {
    const response = await fetch(endpoint);
    const data = await response.json();

    if (data.error) {
      console.error(`API Error: ${data.error.type} - ${data.error.info}`);
      return;
    }

    console.log(`Browser: ${data.browser.name} ${data.browser.version}`);
    console.log(`OS: ${data.os.name}`);
    console.log(`Device Type: ${data.device.type}`);
    console.log(`Is Mobile: ${data.device.is_mobile_device}`);
    console.log(`Is Crawler: ${data.crawler.is_crawler}`);
  } catch (error) {
    console.error("Request failed:", error.message);
  }
}

detectUserAgent(USER_AGENT);


				</code>
			</pre>
		</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-0ad2b29 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-68c2f22 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><span style="font-weight: 400;">Run this with </span><span style="font-weight: 400;">node detect.js</span><span style="font-weight: 400;"> after replacing </span><span style="font-weight: 400;">YOUR_ACCESS_KEY</span><span style="font-weight: 400;"> with your actual key.</span></p><p><span style="font-weight: 400;">Notice the error handling. The Userstack API returns a JSON error object (not an HTTP error status) when something goes wrong, so check for </span><span style="font-weight: 400;">data.error</span><span style="font-weight: 400;"> explicitly. Common error codes include </span><span style="font-weight: 400;">101</span><span style="font-weight: 400;"> for invalid API keys and </span><span style="font-weight: 400;">104</span><span style="font-weight: 400;"> for exceeding your monthly request limit. The full error code list is in the </span><a href="https://userstack.com/documentation"><span style="font-weight: 400;">Userstack documentation</span></a><span style="font-weight: 400;">.</span></p><h2><b>Working Code Example: Python</b></h2><p><span style="font-weight: 400;">This example uses the </span><span style="font-weight: 400;">requests</span><span style="font-weight: 400;"> library, which you can install with </span><span style="font-weight: 400;">pip install requests</span><span style="font-weight: 400;">.</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-17942a3 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-95e4176 elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-twilight copy-to-clipboard ">
			<pre class="highlight-height language-javascript ">
				<code class="language-javascript" readonly="true">
					import requests
from urllib.parse import quote

API_KEY = "YOUR_ACCESS_KEY"
USER_AGENT = (
    "Mozilla/5.0 (iPhone; CPU iPhone OS 17_5 like Mac OS X) "
    "AppleWebKit/605.1.15 (KHTML, like Gecko) Version/17.5 "
    "Mobile/15E148 Safari/604.1"
)

def detect_user_agent(ua):
    endpoint = "http://api.userstack.com/detect"
    params = {
        "access_key": API_KEY,
        "ua": ua
    }

    try:
        response = requests.get(endpoint, params=params)
        data = response.json()

        if "error" in data:
            print(f"API Error: {data['error']['type']} - {data['error']['info']}")
            return

        print(f"Browser: {data['browser']['name']} {data['browser']['version']}")
        print(f"OS: {data['os']['name']}")
        print(f"Device Type: {data['device']['type']}")
        print(f"Is Mobile: {data['device']['is_mobile_device']}")
        print(f"Is Crawler: {data['crawler']['is_crawler']}")

    except requests.exceptions.RequestException as e:
        print(f"Request failed: {e}")

detect_user_agent(USER_AGENT)


				</code>
			</pre>
		</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-11b9a28 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-aa31eab elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><span style="font-weight: 400;">Run this with </span><span style="font-weight: 400;">python detect.py</span><span style="font-weight: 400;">. This example uses an iPhone user agent string, so you should see the device type returned as mobile and </span><span style="font-weight: 400;">is_mobile_device</span><span style="font-weight: 400;"> as </span><span style="font-weight: 400;">true</span><span style="font-weight: 400;">. The </span><span style="font-weight: 400;">requests</span><span style="font-weight: 400;"> library handles URL encoding automatically when you pass parameters through the </span><span style="font-weight: 400;">params</span><span style="font-weight: 400;"> dictionary.</span></p><p><span style="font-weight: 400;">Both examples follow the same pattern: build the request, call the API, check for errors, extract the fields you need.</span></p><h2><b>Real-World Use Cases</b></h2><h3><b>Enriching Server-Side Analytics</b></h3><p><span style="font-weight: 400;">Client-side analytics tools get blocked by ad blockers on an estimated 25% to 40% of desktop traffic. Server logs capture every request, but raw logs give you user agent strings, not structured data.</span></p><p><span style="font-weight: 400;">Pipe your access log UA strings through the Userstack API and you get clean breakdowns by browser, OS, and device type. For high-volume logs, use the bulk lookup endpoint to process up to 100 user agents per request.</span></p><h3><b>Conditional Content Delivery</b></h3><p><span style="font-weight: 400;">Knowing the device type at the server level lets you make decisions before the page renders. Serve compressed images to mobile users. Redirect tablet users to a touch-optimized layout. Skip loading heavy JavaScript bundles for older browsers.</span></p><p><span style="font-weight: 400;">Here’s how this looks in an Express.js middleware:</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-8f377a1 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-3e98f94 elementor-widget elementor-widget-code-highlight">
				<div class="elementor-widget-container">
							<div class="prismjs-twilight copy-to-clipboard ">
			<pre class="highlight-height language-javascript ">
				<code class="language-javascript" readonly="true">
					app.use(async (req, res, next) => {
  const ua = req.headers["user-agent"];
  const endpoint = `http://api.userstack.com/detect?access_key=${API_KEY}&amp;ua=${encodeURIComponent(ua)}`;
  const response = await fetch(endpoint);
  const data = await response.json();

  req.deviceType = data.device?.type || "desktop";
  next();
});


				</code>
			</pre>
		</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-4092ef3 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-a5d2561 elementor-widget elementor-widget-html">
				<div class="elementor-widget-container">
					

<div class="userstack-offer-section">
<h2>Get Your Free Userstack API Key</h2>
<div class="userstack-offer-logo">Userstack</div>

<div class="userstack-offer-middle">

<p>
            Understand your users better with accurate user agent parsing and device detection.
            Built for developers who need reliable browser, OS, and device insights at scale.
</p>

    <a class="userstack-offer-btn" href="https://userstack.com/signup/free?utm_source=blog.apilayer.com&amp;utm_medium=referral&amp;utm_campaign=Blog%20Signups&amp;utm_term=Get%20Your%20Free%20API%20Key!" target="_blank">
        Get Free API Access

</a>
    <div class="userstack-offer-small">
        No credit card required<br />
        Free monthly requests included

</div>
</div>
</div>
				</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-3127b6a e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-89629ea elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<p><span style="font-weight: 400;">Downstream routes can then check </span><span style="font-weight: 400;">req.deviceType</span><span style="font-weight: 400;"> to serve the right content. For production, add caching (Redis or in-memory) to avoid calling the API on every request for the same UA string. Most sites see only a few hundred unique user agent strings, so a simple cache eliminates redundant calls.</span></p><h3><b>A/B Testing Segmented by Device</b></h3><p><span style="font-weight: 400;">When you run A/B tests, aggregate results can hide device-specific differences. A checkout flow change might increase conversions on desktop by 12% but decrease them on mobile by 8%. Without device segmentation, you’d see a modest overall lift and ship a change that hurts mobile users.</span></p><p><span style="font-weight: 400;">Use the </span><span style="font-weight: 400;">device.type</span><span style="font-weight: 400;"> field from Userstack to tag each user entering a test. Store the device type alongside the experiment assignment in your analytics database. At analysis time, filter results by device to catch these splits. You can also use </span><span style="font-weight: 400;">browser.name</span><span style="font-weight: 400;"> and </span><span style="font-weight: 400;">os.name</span><span style="font-weight: 400;"> to target experiments at specific browser or OS combinations.</span></p><h2><b>API Tiers, Rate Limits, and Pricing</b></h2><p><span style="font-weight: 400;">Userstack offers five plans. Here is what each includes, based on the current </span><a href="https://userstack.com/product"><span style="font-weight: 400;">pricing page</span></a><span style="font-weight: 400;">:</span></p><table><tbody><tr><td><p><b>Plan</b></p></td><td><p><b>Monthly Lookups</b></p></td><td><p><b>Price/Month</b></p></td><td><p><b>Key Features</b></p></td></tr><tr><td><p><span style="font-weight: 400;">Free</span></p></td><td><p><span style="font-weight: 400;">100</span></p></td><td><p><span style="font-weight: 400;">$0</span></p></td><td><p><span style="font-weight: 400;">Device, browser, OS detection. HTTPS.</span></p></td></tr><tr><td><p><span style="font-weight: 400;">Basic</span></p></td><td><p><span style="font-weight: 400;">50,000</span></p></td><td><p><span style="font-weight: 400;">$9.99</span></p></td><td><p><span style="font-weight: 400;">Everything in Free, plus crawler detection.</span></p></td></tr><tr><td><p><span style="font-weight: 400;">Business</span></p></td><td><p><span style="font-weight: 400;">500,000</span></p></td><td><p><span style="font-weight: 400;">$49.99</span></p></td><td><p><span style="font-weight: 400;">Adds bulk lookups (up to 100 UAs per request).</span></p></td></tr><tr><td><p><span style="font-weight: 400;">Business Pro</span></p></td><td><p><span style="font-weight: 400;">2,000,000</span></p></td><td><p><span style="font-weight: 400;">$99.99</span></p></td><td><p><span style="font-weight: 400;">Full feature set, highest standard volume.</span></p></td></tr><tr><td><p><span style="font-weight: 400;">Enterprise</span></p></td><td><p><span style="font-weight: 400;">Custom</span></p></td><td><p><span style="font-weight: 400;">Custom</span></p></td><td><p><span style="font-weight: 400;">Custom solutions, volume pricing.</span></p></td></tr></tbody></table><p><span style="font-weight: 400;">One thing to keep in mind: the free plan does not include crawler detection. If filtering bot traffic is part of your use case, you need the Basic plan or higher.</span></p><h2><b>Caching and Rate Limit Best Practices</b></h2><p><span style="font-weight: 400;">Regardless of your plan, avoid calling the API for the same user agent string repeatedly. User agent strings cluster heavily. A typical website might see thousands of requests per day, but only a few hundred unique UA strings.</span></p><p><span style="font-weight: 400;">Cache the parsed result keyed by the raw UA string. A JavaScript </span><span style="font-weight: 400;">Map</span><span style="font-weight: 400;"> or Python </span><span style="font-weight: 400;">dict</span><span style="font-weight: 400;"> works for single-server setups. For distributed systems, use Redis or Memcached with a TTL of 24 to 48 hours. This strategy can reduce actual API calls by 90% or more, making even the free tier viable for low-traffic testing. For guidance on production API patterns, see </span><a href="https://blog.apilayer.com/building-ai-api-interfaces-in-2025-from-rest-to-ai-optimized-design/"><span style="font-weight: 400;">Building AI API Interfaces in 2025</span></a><span style="font-weight: 400;"> on the APILayer blog.</span></p><h2><b>Start Detecting Browsers and Devices Now</b></h2><p><span style="font-weight: 400;">User agent parsing is one of those problems that looks simple until you maintain it in production. Userstack handles the detection logic, keeps its database current, and returns clean JSON you can plug into analytics pipelines, content delivery logic, or A/B testing workflows with minimal code.</span></p><p><span style="font-weight: 400;">Userstack’s free tier gives you 10,000 API requests per month, enough to test it in your project and see real results. Grab your API key here and start detecting browsers and devices in minutes: </span><a href="https://apilayer.com/marketplace/userstack-api"><span style="font-weight: 400;">https://apilayer.com/marketplace/userstack-api</span></a></p><h2><b>Frequently Asked Questions</b></h2><h3><b>1. What is the Userstack API and how does it parse user agent strings?</b></h3><p><span style="font-weight: 400;">Userstack is a REST API that accepts a raw user agent string and returns structured JSON containing the detected browser name and version, operating system, device type, and crawler status. It maintains its own detection database, updated multiple times per week, so you don’t need to write or maintain parsing logic yourself.</span></p><h3><b>2. Can I use the Userstack API to detect mobile vs. desktop users?</b></h3><p><span style="font-weight: 400;">Yes. The API response includes a </span><span style="font-weight: 400;">device</span><span style="font-weight: 400;"> object with a </span><span style="font-weight: 400;">type</span><span style="font-weight: 400;"> field that returns values like </span><span style="font-weight: 400;">desktop</span><span style="font-weight: 400;">, </span><span style="font-weight: 400;">smartphone</span><span style="font-weight: 400;">, or </span><span style="font-weight: 400;">tablet</span><span style="font-weight: 400;">. It also includes an </span><span style="font-weight: 400;">is_mobile_device</span><span style="font-weight: 400;"> boolean, making it straightforward to branch your application logic based on device category.</span></p><h3><b>3. How do I detect the browser and OS from a user agent string using an API?</b></h3><p><span style="font-weight: 400;">Send a GET request to </span><span style="font-weight: 400;">http://api.userstack.com/detect</span><span style="font-weight: 400;"> with your API key and the URL-encoded user agent string as parameters. The response JSON includes a </span><span style="font-weight: 400;">browser</span><span style="font-weight: 400;"> object (with </span><span style="font-weight: 400;">name</span><span style="font-weight: 400;">, </span><span style="font-weight: 400;">version</span><span style="font-weight: 400;">, and </span><span style="font-weight: 400;">engine</span><span style="font-weight: 400;">) and an </span><span style="font-weight: 400;">os</span><span style="font-weight: 400;"> object (with </span><span style="font-weight: 400;">name</span><span style="font-weight: 400;">, </span><span style="font-weight: 400;">family</span><span style="font-weight: 400;">, and </span><span style="font-weight: 400;">family_vendor</span><span style="font-weight: 400;">) that give you all the parsed details.</span></p><h3><b>4. Does the Userstack API detect bots and web crawlers?</b></h3><p><span style="font-weight: 400;">Yes, on the Basic plan ($9.99/month) and above. The API response includes a </span><span style="font-weight: 400;">crawler</span><span style="font-weight: 400;"> object with an </span><span style="font-weight: 400;">is_crawler</span><span style="font-weight: 400;"> boolean, a </span><span style="font-weight: 400;">category</span><span style="font-weight: 400;"> field (identifying the type of crawler), and a </span><span style="font-weight: 400;">last_seen</span><span style="font-weight: 400;"> timestamp. This is useful for filtering automated traffic from your analytics or blocking unwanted bots.</span></p><h3><b>5. What programming languages work with the Userstack API?</b></h3><p><span style="font-weight: 400;">Any language that can send HTTP GET requests works with Userstack. The </span><a href="https://userstack.com/documentation"><span style="font-weight: 400;">official documentation</span></a><span style="font-weight: 400;"> includes code examples for PHP, Python, Node.js, jQuery, Go, and Ruby. Since the API returns standard JSON, integration is straightforward in any modern programming language or framework.</span></p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-097567f e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
					</div>
				</div>
		<div class="elementor-element elementor-element-d6659a8 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-2c204da elementor-widget elementor-widget-html">
				<div class="elementor-widget-container">
									</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-9ce8e20 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-2a60b69 elementor-shape-rounded elementor-grid-0 e-grid-align-center elementor-widget elementor-widget-social-icons">
				<div class="elementor-widget-container">
							<div class="elementor-social-icons-wrapper elementor-grid">
							<span class="elementor-grid-item">
					<a class="elementor-icon elementor-social-icon elementor-social-icon-facebook elementor-repeater-item-0a88b8e" href="https://www.facebook.com/APILayer/" target="_blank">
						<span class="elementor-screen-only">Facebook</span>
						<svg class="e-font-icon-svg e-fab-facebook" viewBox="0 0 512 512" xmlns="http://www.w3.org/2000/svg"><path d="M504 256C504 119 393 8 256 8S8 119 8 256c0 123.78 90.69 226.38 209.25 245V327.69h-63V256h63v-54.64c0-62.15 37-96.48 93.67-96.48 27.14 0 55.52 4.84 55.52 4.84v61h-31.28c-30.8 0-40.41 19.12-40.41 38.73V256h68.78l-11 71.69h-57.78V501C413.31 482.38 504 379.78 504 256z"></path></svg>					</a>
				</span>
							<span class="elementor-grid-item">
					<a class="elementor-icon elementor-social-icon elementor-social-icon-twitter elementor-repeater-item-77ed96b" href="https://x.com/apilayer/" target="_blank">
						<span class="elementor-screen-only">Twitter</span>
						<svg class="e-font-icon-svg e-fab-twitter" viewBox="0 0 512 512" xmlns="http://www.w3.org/2000/svg"><path d="M459.37 151.716c.325 4.548.325 9.097.325 13.645 0 138.72-105.583 298.558-298.558 298.558-59.452 0-114.68-17.219-161.137-47.106 8.447.974 16.568 1.299 25.34 1.299 49.055 0 94.213-16.568 130.274-44.832-46.132-.975-84.792-31.188-98.112-72.772 6.498.974 12.995 1.624 19.818 1.624 9.421 0 18.843-1.3 27.614-3.573-48.081-9.747-84.143-51.98-84.143-102.985v-1.299c13.969 7.797 30.214 12.67 47.431 13.319-28.264-18.843-46.781-51.005-46.781-87.391 0-19.492 5.197-37.36 14.294-52.954 51.655 63.675 129.3 105.258 216.365 109.807-1.624-7.797-2.599-15.918-2.599-24.04 0-57.828 46.782-104.934 104.934-104.934 30.213 0 57.502 12.67 76.67 33.137 23.715-4.548 46.456-13.32 66.599-25.34-7.798 24.366-24.366 44.833-46.132 57.827 21.117-2.273 41.584-8.122 60.426-16.243-14.292 20.791-32.161 39.308-52.628 54.253z"></path></svg>					</a>
				</span>
							<span class="elementor-grid-item">
					<a class="elementor-icon elementor-social-icon elementor-social-icon-youtube elementor-repeater-item-9f45179" href="https://www.youtube.com/channel/UCwDncja3klyMhkKXWV1zoNQ" target="_blank">
						<span class="elementor-screen-only">Youtube</span>
						<svg class="e-font-icon-svg e-fab-youtube" viewBox="0 0 576 512" xmlns="http://www.w3.org/2000/svg"><path d="M549.655 124.083c-6.281-23.65-24.787-42.276-48.284-48.597C458.781 64 288 64 288 64S117.22 64 74.629 75.486c-23.497 6.322-42.003 24.947-48.284 48.597-11.412 42.867-11.412 132.305-11.412 132.305s0 89.438 11.412 132.305c6.281 23.65 24.787 41.5 48.284 47.821C117.22 448 288 448 288 448s170.78 0 213.371-11.486c23.497-6.321 42.003-24.171 48.284-47.821 11.412-42.867 11.412-132.305 11.412-132.305s0-89.438-11.412-132.305zm-317.51 213.508V175.185l142.739 81.205-142.739 81.201z"></path></svg>					</a>
				</span>
							<span class="elementor-grid-item">
					<a class="elementor-icon elementor-social-icon elementor-social-icon-instagram elementor-repeater-item-6bbb06a" href="https://www.instagram.com/apilayer" target="_blank">
						<span class="elementor-screen-only">Instagram</span>
						<svg class="e-font-icon-svg e-fab-instagram" viewBox="0 0 448 512" xmlns="http://www.w3.org/2000/svg"><path d="M224.1 141c-63.6 0-114.9 51.3-114.9 114.9s51.3 114.9 114.9 114.9S339 319.5 339 255.9 287.7 141 224.1 141zm0 189.6c-41.1 0-74.7-33.5-74.7-74.7s33.5-74.7 74.7-74.7 74.7 33.5 74.7 74.7-33.6 74.7-74.7 74.7zm146.4-194.3c0 14.9-12 26.8-26.8 26.8-14.9 0-26.8-12-26.8-26.8s12-26.8 26.8-26.8 26.8 12 26.8 26.8zm76.1 27.2c-1.7-35.9-9.9-67.7-36.2-93.9-26.2-26.2-58-34.4-93.9-36.2-37-2.1-147.9-2.1-184.9 0-35.8 1.7-67.6 9.9-93.9 36.1s-34.4 58-36.2 93.9c-2.1 37-2.1 147.9 0 184.9 1.7 35.9 9.9 67.7 36.2 93.9s58 34.4 93.9 36.2c37 2.1 147.9 2.1 184.9 0 35.9-1.7 67.7-9.9 93.9-36.2 26.2-26.2 34.4-58 36.2-93.9 2.1-37 2.1-147.8 0-184.8zM398.8 388c-7.8 19.6-22.9 34.7-42.6 42.6-29.5 11.7-99.5 9-132.1 9s-102.7 2.6-132.1-9c-19.6-7.8-34.7-22.9-42.6-42.6-11.7-29.5-9-99.5-9-132.1s-2.6-102.7 9-132.1c7.8-19.6 22.9-34.7 42.6-42.6 29.5-11.7 99.5-9 132.1-9s102.7-2.6 132.1 9c19.6 7.8 34.7 22.9 42.6 42.6 11.7 29.5 9 99.5 9 132.1s2.7 102.7-9 132.1z"></path></svg>					</a>
				</span>
							<span class="elementor-grid-item">
					<a class="elementor-icon elementor-social-icon elementor-social-icon-linkedin elementor-repeater-item-76fbdb3" href="https://www.linkedin.com/company/apilayer/" target="_blank">
						<span class="elementor-screen-only">Linkedin</span>
						<svg class="e-font-icon-svg e-fab-linkedin" viewBox="0 0 448 512" xmlns="http://www.w3.org/2000/svg"><path d="M416 32H31.9C14.3 32 0 46.5 0 64.3v383.4C0 465.5 14.3 480 31.9 480H416c17.6 0 32-14.5 32-32.3V64.3c0-17.8-14.4-32.3-32-32.3zM135.4 416H69V202.2h66.5V416zm-33.2-243c-21.3 0-38.5-17.3-38.5-38.5S80.9 96 102.2 96c21.2 0 38.5 17.3 38.5 38.5 0 21.3-17.2 38.5-38.5 38.5zm282.1 243h-66.4V312c0-24.8-.5-56.7-34.5-56.7-34.6 0-39.9 27-39.9 54.9V416h-66.4V202.2h63.7v29.2h.9c8.9-16.8 30.6-34.5 62.9-34.5 67.2 0 79.7 44.3 79.7 101.9V416z"></path></svg>					</a>
				</span>
							<span class="elementor-grid-item">
					<a class="elementor-icon elementor-social-icon elementor-social-icon-github elementor-repeater-item-1c46185" href="https://github.com/apilayer/" target="_blank">
						<span class="elementor-screen-only">Github</span>
						<svg class="e-font-icon-svg e-fab-github" viewBox="0 0 496 512" xmlns="http://www.w3.org/2000/svg"><path d="M165.9 397.4c0 2-2.3 3.6-5.2 3.6-3.3.3-5.6-1.3-5.6-3.6 0-2 2.3-3.6 5.2-3.6 3-.3 5.6 1.3 5.6 3.6zm-31.1-4.5c-.7 2 1.3 4.3 4.3 4.9 2.6 1 5.6 0 6.2-2s-1.3-4.3-4.3-5.2c-2.6-.7-5.5.3-6.2 2.3zm44.2-1.7c-2.9.7-4.9 2.6-4.6 4.9.3 2 2.9 3.3 5.9 2.6 2.9-.7 4.9-2.6 4.6-4.6-.3-1.9-3-3.2-5.9-2.9zM244.8 8C106.1 8 0 113.3 0 252c0 110.9 69.8 205.8 169.5 239.2 12.8 2.3 17.3-5.6 17.3-12.1 0-6.2-.3-40.4-.3-61.4 0 0-70 15-84.7-29.8 0 0-11.4-29.1-27.8-36.6 0 0-22.9-15.7 1.6-15.4 0 0 24.9 2 38.6 25.8 21.9 38.6 58.6 27.5 72.9 20.9 2.3-16 8.8-27.1 16-33.7-55.9-6.2-112.3-14.3-112.3-110.5 0-27.5 7.6-41.3 23.6-58.9-2.6-6.5-11.1-33.3 2.6-67.9 20.9-6.5 69 27 69 27 20-5.6 41.5-8.5 62.8-8.5s42.8 2.9 62.8 8.5c0 0 48.1-33.6 69-27 13.7 34.7 5.2 61.4 2.6 67.9 16 17.7 25.8 31.5 25.8 58.9 0 96.5-58.9 104.2-114.8 110.5 9.2 7.9 17 22.9 17 46.4 0 33.7-.3 75.4-.3 83.6 0 6.5 4.6 14.4 17.3 12.1C428.2 457.8 496 362.9 496 252 496 113.3 383.5 8 244.8 8zM97.2 352.9c-1.3 1-1 3.3.7 5.2 1.6 1.6 3.9 2.3 5.2 1 1.3-1 1-3.3-.7-5.2-1.6-1.6-3.9-2.3-5.2-1zm-10.8-8.1c-.7 1.3.3 2.9 2.3 3.9 1.6 1 3.6.7 4.3-.7.7-1.3-.3-2.9-2.3-3.9-2-.6-3.6-.3-4.3.7zm32.4 35.6c-1.6 1.3-1 4.3 1.3 6.2 2.3 2.3 5.2 2.6 6.5 1 1.3-1.3.7-4.3-1.3-6.2-2.2-2.3-5.2-2.6-6.5-1zm-11.4-14.7c-1.6 1-1.6 3.6 0 5.9 1.6 2.3 4.3 3.3 5.6 2.3 1.6-1.3 1.6-3.9 0-6.2-1.4-2.3-4-3.3-5.6-2z"></path></svg>					</a>
				</span>
							<span class="elementor-grid-item">
					<a class="elementor-icon elementor-social-icon elementor-social-icon-discord elementor-repeater-item-a70b088" href="https://discord.com/invite/hgjA78638n" target="_blank">
						<span class="elementor-screen-only">Discord</span>
						<svg class="e-font-icon-svg e-fab-discord" viewBox="0 0 448 512" xmlns="http://www.w3.org/2000/svg"><path d="M297.216 243.2c0 15.616-11.52 28.416-26.112 28.416-14.336 0-26.112-12.8-26.112-28.416s11.52-28.416 26.112-28.416c14.592 0 26.112 12.8 26.112 28.416zm-119.552-28.416c-14.592 0-26.112 12.8-26.112 28.416s11.776 28.416 26.112 28.416c14.592 0 26.112-12.8 26.112-28.416.256-15.616-11.52-28.416-26.112-28.416zM448 52.736V512c-64.494-56.994-43.868-38.128-118.784-107.776l13.568 47.36H52.48C23.552 451.584 0 428.032 0 398.848V52.736C0 23.552 23.552 0 52.48 0h343.04C424.448 0 448 23.552 448 52.736zm-72.96 242.688c0-82.432-36.864-149.248-36.864-149.248-36.864-27.648-71.936-26.88-71.936-26.88l-3.584 4.096c43.52 13.312 63.744 32.512 63.744 32.512-60.811-33.329-132.244-33.335-191.232-7.424-9.472 4.352-15.104 7.424-15.104 7.424s21.248-20.224 67.328-33.536l-2.56-3.072s-35.072-.768-71.936 26.88c0 0-36.864 66.816-36.864 149.248 0 0 21.504 37.12 78.08 38.912 0 0 9.472-11.52 17.152-21.248-32.512-9.728-44.8-30.208-44.8-30.208 3.766 2.636 9.976 6.053 10.496 6.4 43.21 24.198 104.588 32.126 159.744 8.96 8.96-3.328 18.944-8.192 29.44-15.104 0 0-12.8 20.992-46.336 30.464 7.68 9.728 16.896 20.736 16.896 20.736 56.576-1.792 78.336-38.912 78.336-38.912z"></path></svg>					</a>
				</span>
					</div>
						</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-5c93290 e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-88aaac8 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<h2 style="font-family: Roboto; margin-top: 30px; font-size: 18px; background-color: #eef1f4;"><strong style="font-weight: bold;">Recommended Resources: </strong></h2><p> </p>								</div>
				</div>
					</div>
				</div>
		<div class="elementor-element elementor-element-bd6cefa e-flex e-con-boxed e-con e-parent">
					<div class="e-con-inner">
				<div class="elementor-element elementor-element-343d789 elementor-widget elementor-widget-text-editor">
				<div class="elementor-widget-container">
									<ul><li><strong><a href="https://blog.apilayer.com/ip-address-metadata-explained-what-you-can-learn-from-an-ip-isp-asn-location-and-more/">IP Address Metadata Explained What You Can Learn From an IP, ISP, ASN, Location, and More</a></strong></li><li><a href="https://blog.apilayer.com/static-ip-address-explained-what-it-is-pros-cons-costs-and-when-you-need-one/"><strong>Static IP Address Explained: What It Is, Pros & Cons, Costs, and When You Need One</strong></a></li></ul>								</div>
				</div>
					</div>
				</div>
				</div>
		<p>The post <a href="https://blog.apilayer.com/how-to-detect-browser-and-os-with-userstack-api-2026-guide/">How to Detect Browser and OS with Userstack API (2026 Guide)</a> appeared first on <a href="https://blog.apilayer.com">APILayer Blog - All About APIs: AI, ML, Finance, &amp; More APIs</a>.</p>
