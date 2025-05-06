---
layout: post
title:  "My First commit to SeleniumHQ Was Merged!"
date:   2025-05-06 17:05:51 +0800
categories: selenium
---
## My First Commit to SeleniumHQ Was Merged! Here’s What Happened.

Contributing to open source has always felt a bit ... intimidating. You scroll through issues, see names like "core maintainer" or "WIP:breaking change", and quickly close the tab.

But this time was different.

### How I ended up 

I've been using Selenium on and off in my work, and like many others, I've spent hours browsing through selenium.dev for docs, examples, or quick fixes. One day, I noticed something on the site that made me go:


    "Hmm... I could actually provide an example for this."

So I did what every open source newbie does: 
I forked the repo, cloned it, made the change, and panicked. 

### The commit

My change wasn't huge - just an example in Python and some improvement to the docs. But it felt huge to me.

I double-checked everything locally, make sure I didn't actually delete half the page, and pushed my commit. Then came the moment of truth: opening a Pull Request. I opened the PR against the truck branch. I added a friendly description, crossed my figures, and waited... and waited... 

And then, finally - feedback!

Two wonderful reviewers left comments on my PR. It was critique time.

My PR wasn't perfect. It needed some tweaks, and I had to submit commits eight times to address all suggestions.


First comment: 


      1. the example must be moved into a separate executable file like our other examples
      2. the example must be properly formatted like other examples to pull from the example file
      3. the example must be propagated to all translations
      4. lines must be kept to 100 characters per the style guide https://www.selenium.dev/documentation/about/style/


Second comment: 


    "Since you have provided a full seemingly working example in python - what we can instead do is utilize the codeblock architecture."


Third comment: 


    "Only thing I notice atm that needs to be changed is that you're missing the other languages in the codeblock elements. We use those so that readers can see where examples are missing and for ease of contributions."


Fourth comment: 


    "in the codeblock elements, make sure that each language without an example has a badgecode so we can help facilitate contributions"


I made the changes, pushed the commits, and waited again. There was more feedback, more edits, more commits.

It felt like a never-ending cycle of feedback → update → submit → repeat. But I learned a ton from the process. Feedback is key to improvement, and getting my PR into shape took patience, persistence, and a lot of commits.


### The comeback 

Fast-forward to May 6th, 2025 - eight months later - I got the notification.

  🚀 Your pull request has been merged into truck.

Wait, What! After all this time, it was finally merged! 😲

I refreshed the page. 
Checked the commit history. 
It was real. The merge happened, and my PR was offically part of Selenium's website! 

* **[PR](https://github.com/SeleniumHQ/seleniumhq.github.io/pull/1924)**
* **[Commit](https://github.com/SeleniumHQ/seleniumhq.github.io/commit/1309acfaf9eb9cb11e5e476693ba8561abcb8eb5)** 


### What I learned

Open source moves at its own pace. Sometimes it feels like you're just sitting in a waiting room for months. But the feedback and iteration process is a big part of it. 

The process of receiving comments and improving your work is one of the most rewarding aspects of contributing to open source. You learn by doing, improving and refining. 

Even if it feels like you're just submitting small tweaks. Those changes matter. Keep going.

### Advice to My Past Self (and You)

Don’t get discouraged by feedback. Every comment is an opportunity to improve. Remember, even the most experienced contributors get their PRs reviewed.

If it takes longer than you expect, don’t delete your fork. The merge might come at the most unexpected time.

Be patient and persistent — don’t let the silence between commits deter you. Keep improving, keep iterating, and eventually, your PR will get there.


### Final Thoughts

My first Selenium commit wasn’t technically challenging. But emotionally? It was a journey.

From hope, to waiting, to receiving feedback (and more feedback), to eight commits, and finally to that beautiful “merged” notification, I can honestly say it was one of the most rewarding experiences I’ve had in the open-source world.

So, if you’ve got an old PR that’s just sitting there… don’t give up. The merge may take time, but it’s always worth it in the end. 🚀
