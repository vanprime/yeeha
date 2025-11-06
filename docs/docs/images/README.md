# jira-extension

A small hacky extension to circumvent using paid plugins like scriptrunner or other external services, since all data is already there. We just have to use it. 

# motivation

On premise and cloud version of jira (imo) lack functionality to inspect the data we're generating whilst using the platform. This is an approach to make use of that data as close to jira as possible. Instead of exporting the data to spreadsheets ot other platforms, we built a custom in-browser solution.

# usage

1. Access Extension Management Page: Type `chrome://extensions/` in the address bar and press Enter.<br>
   <img src="./docs/images/chrome-extension-manager.png" width="300">

2. Enable Developer Mode: In the top right corner of the Extensions page, you’ll see a switch for Developer mode. Turn it on. This allows you to load unpacked extensions.<br>
   <img src="./docs/images/chrome-developer-mode.png" width="300">

3. Load the Unpacked Extension: Click on the load unpacked button which will appear in the corner after you enable Developer Mode. A file dialog will open. Navigate to your download of the [release](https://github.com/vanprime/jira-extension/releases).<br>
   <img src="./docs/images/chrome-load-unpacked.png" width="300">

4. pin the extension to the toolbar<br>
   <img src="./docs/images/chrome-pin-extension.png" width="300">

5. click the extension icon to toggle the side panel and start using the different features.


# auth

Auth is handled by your session. The jira API relies on browser-based authentication (like cookies or session tokens) for requests originating from the same domain. When the extension's script makes a fetch request to the jira API from a page on the same domain, it automatically includes these credentials, thanks to the browser's same-origin policy.