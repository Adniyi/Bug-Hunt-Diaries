---
date: '2025-05-15T01:50:05-07:00'
draft: false
title: 'Things That Broke and What They Taught Me'
tags: ['Personal Growth', 'Lessons Learned', 'Resilience']
---

## Things That Broke and What They Taught Me (Besides Humility)

If you’ve ever stared at an error message and thought, “I have absolutely no idea what’s happening,” congratulations: you are officially a programmer. In this post, I’m sharing a few of my favorite “learning moments” — which is a nicer way of saying “times when everything broke and I had to figure out why.”

These aren’t epic system crashes or million-dollar bugs (yet, thankfully). They’re the small, annoying, sometimes hilarious hiccups that taught me a lot more than things going right ever could.

## 🧩 Broken Thing #1 – Why isn't my site deploying?

**What I Was Doing:**
I was in the process of deploying my site (this site, to be clear). I did the usual things you know: `git add`, `git commit`, `git push`, then checked my hosting platform and saw “deployment failed.” And then... nothing. No deployment. It just sat there, not deployed. I had to read the console to find out what broke.


**What Broke:**
So, it turns out the issue was a typo in my submodules. After a lot of trial and error, I found the problem. I had a submodule that wasn’t properly linked to the main project. I had to go back and re-add the submodule, then re-run the deployment. It was a small mistake, but it took me a while to figure out.

**What i Learned:**
This experience taught me the importance of double-checking my work, especially when it comes to dealing with submodules.


## 🧩 Broken Thing #2 – Why is it still not deploying?

**What I Was Doing:**
I was trying to deploy my site again, but this time, I was getting a different error message. 


**What Broke:**
I thought the first issue was the end of my problems, but NO, it was just the beginning. After fixing the submodules, I encountered another error. This time, the build process for deployment got past the Cloning repository step but failed at the Building step. I had to dig deeper to find the problem. So, I did what any good programmer of this age would do: I copied the error logs without reading them and pasted them into my good old friend, ChatGPT. Daddy GPT told me where the problem was, but since I thought it was a code-related issue, I just waited for it to spit out some code... but to no avail. So, I decided to read the error logs myself.

After reading the logs, I figured out the problem. The issue was that the Hugo version I was using was not compatible with the theme I was using. I had to update the Hugo version to the latest in the environment variables and re-deploy.

**What i Learned:**
This experience taught me the importance of reading error logs and not just relying on tools or other people to solve the problem. Sometimes, the solution is right in front of you, and you just need to take the time to read and understand it.


## 🎓 Wrap-up: What All These Broken Things Taught Me
Breaking things is inevitable — and honestly, kind of necessary. Every time something didn’t work, I learned a little more about how it does work. And each bug I fix is one more piece of experience I carry into the next project.

Also, I now read error messages more slowly. That alone has saved me hours. 😅