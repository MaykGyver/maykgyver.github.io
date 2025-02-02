---
layout: post
title: Get metrics about your Github Pages page with Microsoft Clarity
date: "2025-02-02"
---

This post was inspired by [a discussion on Github](https://github.com/orgs/community/discussions/31474#discussioncomment-11955747). A [thread on Stack Overflow](https://stackoverflow.com/questions/54034946/how-to-add-more-to-a-head-on-jekyll) pointed to a solution which verified at [minima's repository](https://github.com/jekyll/minima/blob/master/_includes/custom-head.html).

1. Go to [Microsoft Clarity](https://clarity.microsoft.com/) and sign up for your page.
2. Clarity then asks for the "Installation Method". At the time of this writing, Github Pages wasn't available under "Install on a third-party platform". So we will go for "Install manually".
3. Copy the presented code block.
4. Head over to your page's repository and create a new file *_includes/custom-head.html*. Paste the snippet from step 3.
5. Commit the change. Wait for some hours and get back to MS Clarity. You should see first results now.

Some minor hassle was between steps 4 and 5. The toolchain (my local jekyll for preview as well as the productive Github Pages) did not properly handle *_includes/custom-head.html*. This looks to be fixed when updating from minima 2.5.x to 3.0.x. I hope that when you read this, minima 3+ is delivered by default. But until then, I use a workaround. I gained early access on the 3.0 release via direct retrieval from its github repository. I updated my Gemfile:
```diff
- gem "minima", "~> 2.5"
+ gem "minima", git: "https://github.com/jekyll/minima"
```
Since we deviate now from Github Page's vanilla environment, we need to [configure Github Actions for Jekyll](https://jekyllrb.com/docs/continuous-integration/github-actions/). When the *.github/workflows/jekyll.yml* template was proposed, I found no modifications necessary and committed the template as proposed.

Then I wanted to [verify the installation](https://learn.microsoft.com/en-us/clarity/setup-and-installation/clarity-setup#verify-your-installation) again. And nothing happened. Then I realized, that I use Vivaldi browser which has a reasonable tracking blocker enabled by default. I disabled it for my page and MS Clarity started to show data. Getting data when tracking is allowed and getting no data when the user wants no tracking – that sounds reasonable to me. Mission accomplished.
