---
description: My notes/ walkthrough of the ProxyAsAService HackTheBox challenge.
draft: false
tags:
  - "#hackthebox"
  - "#ssrf"
enableToc: true
unlisted: false
title: ProxyAsAService Challenge
date: 2024-03-31
---

An [[rating:: easy]], [[challenge_category:: web]] challenge. This is a fun little challenge introducing the concept of Server-Side Request Forgery.

## Initial Observations
### Technology:
- [Flask](https://flask.palletsprojects.com/en/3.0.x/) Web Framework
### Misc
- Flag gets set as an environment variable in the Dockerfile.
- Secret Key created at runtime in `config.py`
- Two [Blueprints](https://flask.palletsprojects.com/en/3.0.x/blueprints/) (Flask Concept for modular applications):
	- `/` <- public
	- `/debug` 
- Two routes:
	- `/` <- public
	- `/environment` <- only accessible if the `util.is_from_localhost` check is passed
- The public route accepts an `url` as a request arg. It fetches `f'http://reddit.com/{url}'`  and returns, content, status code and headers.
- The `/environment` route returns all env vars, including the flag.

>[!info] First Hypothesis
>We need to somehow trick the proxy route to query the environment route, since it can only be accessed from localhost

## Dive In
Lets continue inspecting the `util.py` functions, since the key to the challenge seems to be here.

### The `is_from_localhost` function
The function checks if the `remote_addr` of the Flask [Request Object](https://tedboy.github.io/flask/generated/generated/flask.Request.html) is equal to `127.0.0.1`. Without knowing how exactly Flask populates that field, I assume that it can't be easily tricked. Nevertheless I had to try:
```bash
curl http://<target_ip>/environment -H "X-Forwarded-For: 127.0.0.1"
```
But as expected it returned a `403: Not Allowed`.

### The `proxy_req` function
This function gets the http method, a byte representation of the request body (`request.get_data()`) and a selection of the headers (`['x-csrf-token', 'cookie', 'referer']`) from the request context, and the formatted reddit URL, does a request using python `requests`, then checks the requested URL as well as the `response.url` with the `is_safe_url` function, and then returns response and headers. 

>[!summary] Mental Notes
>Seeing that the is_safe_url check is made after the request is performed made me suspicious. Furthermore I might have to research what the `response.url` is and in which cases it differs from the requested URL.

### The `is_safe_url` function
Checks if any of ['localhost', '127.', '192.168.', '10.', '172.'] is in `url` . What jumps out to me is that `0.0.0.0` is missing in this list. 

>[!info] Attack Path
>From looking at the source code, we know that we can get the flag, by 
>- making a request to `/debug/environment`
>- from localhost
>- by controlling the `url` part in `f'http://reddit.com/{url}'`
>- bypassing the `is_safe_url` method by addressing localhost via `0.0.0.0`
>
 I knew that this type of attack is called Server-Side Request Forgery or SSRF. Since this is my first SSRF, I had [a quick chat](https://chat.openai.com/share/5ad97158-c568-448e-880b-74fa27b4f35c) about it and ChatGPT remembered me of the option to use an `@` in URLs! This might be it!
 
## Exploit
With all this information at hand, the exploitation is rather simple. This is our crafted URL:
```bash
curl http://<target_ip>/?url=%400.0.0.0%3A1337%2Fdebug%2Fenvironment
```

Et voilà: There we have the flag (along with all other environment variables 🤯)

