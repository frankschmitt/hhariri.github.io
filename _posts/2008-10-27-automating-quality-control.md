---
layout: post
title: Automating "quality" control?
categories: []
tags:
- Process
status: publish
type: post
published: true
meta:
  _elasticsearch_indexed_on: '2008-10-27 14:34:42'
comments: true
---
<p>I read a <a href="http://elegantcode.com/2008/10/21/visual-studio-plug-in-idea/">post today</a> about how there should be a plug-in for Visual Studio to disable copy/paste, in order to prevent people from copying and pasting the same code over and over again, instead of applying basic design principles (DRY). Now I'm sure the author didn't mean this in a serious way (it's hell to be without copy/paste, and if you don't believe me, use the iPhone for a couple of days), but it reminds me of recent discussions about RhinoMock vs TypeMock, or put another way, TypeMock vs every other mocking framework out there. </p>  <p>The main discussion was that TypeMock could lead to malpractice. By using TypeMock, people could potentially end up with tightly coupled code. Since there isn't a need to inject dependencies, you could create dependent objects in your classes. I posted my thoughts on this a long time ago (too far back for Live Writer to show it up in recent blog posts), so I'm not going to get into too much detail, but suffice to say, I didn't agree then and I still don't agree now. </p>  <p>But what has that got to do with the copy/paste post you might be asking yourself. Well I think in both cases, we are showing signs of wanting the tool/framework to do the work for us, to police us, so to speak. Which begs the question, why? </p>  <p>I work on a team, as a tech lead. The part about work is questionable, but nonetheless I am on a team. Part of my job is to oversee my team and make sure that things are being done correctly. In my book, correct means applying good design principles. It is my job to make sure that my team applies these principles. If a team member is new or is not familiar with certain concepts, it's my job to teach them to him/her. With new team members, there is normally a period of code-review, where during check-ins code is reviewed by senior developers (either myself or someone else on the team) to make sure that they comply not only with design principles but standards of coding and practices. However, as time elapses, code reviews become less frequent (although there should always be peer review on a team from time to time). So initially it might be a time-consuming job, but in the long run it isn't. In many cases it's also very productive since there's always something new you can learn. </p>  <p>If people are taught things in a way, that not only do they understand, but see what happens when they do it the wrong way, they will act correctly. </p>  <p>Developers are not stupid. </p>
