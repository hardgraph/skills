---
modificationDate: July 29, 2026
title: Troubleshooting proxies
description: Learn about troubleshooting proxies with a set of recommended tools.
---

<AgentInstructions>

## Submitting Feedback

If you encounter errors, misleading or outdated information, report it so Expo can be improved:

Preferred command:
npx --yes submit-expo-feedback@latest --category docs --subject "/troubleshooting/proxies/" "<actionable feedback>"

Direct HTTP fallback:
curl -X POST https://api.expo.dev/v2/feedback/docs-send -H 'Content-Type: application/json' -d '{"url":"/troubleshooting/proxies/","feedback":"🤖 Agent feedback for docs: <specific, actionable description> (<model>, <harness>)"}'

Only submit when you have something specific and actionable to report. Try to give the most context.

## Navigation

When answering a related or follow-up question, fetch the relevant page below as Markdown (.md) instead of guessing; use llms.txt for the full map.

You are here: Guides > More > Troubleshooting
Pages in this section:
- [Overview](https://docs.expo.dev/troubleshooting/overview.md)
- ["Application has not been registered" error](https://docs.expo.dev/troubleshooting/application-has-not-been-registered.md)
- [Clear bundler caches on macOS and Linux](https://docs.expo.dev/troubleshooting/clear-cache-macos-linux.md)
- [Clear bundler caches on Windows](https://docs.expo.dev/troubleshooting/clear-cache-windows.md)
- ["React Native version mismatch" errors](https://docs.expo.dev/troubleshooting/react-native-version-mismatch.md)
- [Proxies](https://docs.expo.dev/troubleshooting/proxies.md) (this page)
Full documentation tree: [llms.txt](https://docs.expo.dev/llms.txt)

</AgentInstructions>

This documentation is available as Markdown for AI agents and LLMs. See the [full Markdown index](/llms.txt) or append .md to any documentation URL.

# Troubleshooting proxies

Learn about troubleshooting proxies with a set of recommended tools.

## macOS proxy configuration (Sierra)

> If anything goes wrong, you can revert to the "Automatic Proxy settings" in System Network Preferences using Automatic Proxy Configuration `your-corporate-proxy-uri:port-number/proxy.pac`.

### Overview

To run this in the local iOS Simulator while on your corporate Wi-Fi network, a local proxy manager is required. You can use a local proxy application such as [Charles](https://charlesproxy.com).

#### Open macOS network preferences

1.  Open `System Preferences` for your Mac (Apple Menu > System Preferences).
2.  Go to Network.
3.  Be sure your `Location` is set to your proxy network, and not "Automatic".
4.  With Wi-Fi selected on the left and/or ethernet connection, click `Advanced...` on the bottom right side of the window.

#### Configure proxy address

1.  Disable/uncheck "Automatic Proxy Configuration" if it is set.
2.  Check "Web Proxy (HTTP)" and set "Web Proxy Server" to 127.0.0.1 : 8888
3.  Check "Secure Web Proxy (HTTPS)" and set "Secure Web Proxy Server" to 127.0.0.1 : 8888

### Configure `Charles`

1.  Open Charles
    
2.  If it asks, don't allow it to manage your macOS Network Configuration, the previous steps do that. (If you change Charles port, update the previous step to the correct port instead of default 8888)
    
3.  In the menu of Charles go to `Proxy > External Proxy Settings`, check `Use external proxy servers`
    
4.  Check `Web Proxy (HTTP)`, and enter `your-corporate-proxy-uri:port-number`
    
5.  Check `Proxy server requires a password`
    
6.  Domain: YOUR DOMAIN, Username: YOUR USERNAME Password: YOUR PASSWORD
    
7.  Same for Secure Web Proxy (HTTPS). _Be sure to fill in the same proxy, username, and password address_ fields.
    
8.  In the text area for `Bypass external proxies for the following hosts:` enter:
    
    ```text
    localhost
    *.local
    ```
    
    You may need to include your mail server or other corporate network addresses.
    
9.  Check "Always bypass external proxies for localhost"
    

### iOS Simulator configuration

If you have an existing iOS Simulator custom setup going that is not working, "Simulator > Reset Content and Settings" from the menu.

If you have the Simulator open still, quit it.

Now, in Charles under the "Help" menu > Install Charles Root Certificate, and then again for Install Charles Root Certificate in iOS Simulators

> **Technical note:** This whole process is required because the iOS Simulator is served a bum proxy certificate instead of the actual certificate, and doesn't allow it, for [https://exp.host/](https://exp.host/) which is required to run Expo.  
> **Also note:** Configure applications that need internet access, such as Spotify, to use [http://localhost:8888](http://localhost:8888) as your proxy. Some apps, such as Chrome and Firefox, you can configure in the settings to use your "System Network Preferences" which will use Charles : 8888, or no proxy, depending on how you have your "Location" set in the Apple menu/network preferences. If you are set to "Automatic" no proxy is used, if it is set to "your proxy network" the proxy is used and Charles will need to be running.

## Command-line application proxy configuration

npm, git, Brew, Curl, and any other command-line applications need proxy access too.

### For npm

Open `~/.npmrc` and set:

```ini
http_proxy=http://localhost:8888
https_proxy=http://localhost:8888
```

### For git

Open `~/.gitconfig` and set

```ini
[http]
  proxy = http://localhost:8888
[https]
  proxy = http://localhost:8888
```

### For command-line applications

Depending on your shell, and config, Open `~/.bashrc`, `~/.bash_profile`, or `~/.zshrc` or wherever you set your shell variables and set:

```bash
export HTTP_PROXY="http://localhost:8888"
export http_proxy="http://localhost:8888"
export ALL_PROXY="http://localhost:8888"
export all_proxy="http://localhost:8888"
export HTTPS_PROXY="http://localhost:8888"
export https_proxy="http://localhost:8888"
```

> If you switch your network location back to "Automatic" to use npm or git, you will need to comment these lines out using a `#` before the line you wish to disable. You could alternatively use a command-line proxy manager if you prefer.
