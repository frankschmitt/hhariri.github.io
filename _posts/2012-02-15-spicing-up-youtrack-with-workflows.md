---
layout: post
title: Spicing up YouTrack with Workflows
categories:
- JetBrains
tags:
- Workflow
status: publish
type: post
published: true
meta:
  reddit: a:2:{s:5:"count";i:0;s:4:"time";i:1385704298;}
  _elasticsearch_indexed_on: '2012-02-15 06:55:45'
---
<p align="justify">I had to set up a new project yesterday on YouTrack and needed to customize some of the default
status fields. One of the ones I added was a ‘Paused’ status, for those times when a task gets held up for whatever the reason.
Adding a custom value is pretty simple to do: under <em>Administration</em>, select the project, click on the the <em>Fields </em>tab,
and click on the <em>Value Bundle</em> link of the field you want to update, in my case the <em>State </em>field</p> <p align="left">
<p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;" title="image" border="0" alt="image" src="{{ site.images }}/wf-1.png" width="496" height="176"></a></p> <p align="left"> <p align="left">Once there, select <em>Add State </em>and type in the new value. You can give it a description and an optional color. I chose an orange tone to indicate its paused [<em>Resolved </em>checkbox indicates if the state represents the issue as resolved].</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image1.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image"
 src="{{ site.images }}/wf-2.png" width="916" height="248"></a></p> <p align="left"> <p align="justify">That was pretty simple. However, there is a minor issue. I want to know why a task is paused, much like I’d like to know why an issue is closed as ‘Will not fixed’. I’d like the person pausing it to provide a comment as to why the task has been put on hold. How can I accomplish this in YouTrack? How can I force someone to submit a comment when a state is set to Paused.</p> <p align="left"> <h3 align="left">Workflow Editor</h3> <p align="justify">This is where YouTrack’s workflow editor comes into play. I looked at this tool nearly 2 years ago and I must say, there have been some major improvements. However what really shocked me was how simple it was to get up to speed. In less than 15 minutes I had downloaded it, created a custom rule and had it working on my project.</p> <p align="left"> <p align="justify">The workflow editor is based on <a href="http://www.jetbrains.com/mps">MPS</a> or Meta Programming System. If you’re not familiar with this, it is an extremely powerful Open Source tool from us that allows you to create and extend languages. But it’s not limited to only creating a DSL. It also provides you with the means to create editors, complete with Intellisense, Code Analysis and Debuggers. In fact, YouTrack itself is based on a language called WEBR which is developed on MPS.</p> <p align="justify">The good news of course is that you do not need to know anything about MPS to work with Workflows in YouTrack. There’s already a packaged-up, self-contained editor that provides you with everything you need.</p> <p align="left">Let’s see how we can use this to add some spice to YouTrack.</p> <p align="left"> <h3 align="left">Scenario: Requiring a comment when changing a state</h3> <p align="justify">The problem we are going to tackle is the one mentioned previously: we want the user to fill out a comment when changing the state of an issue. Since there’s no specific workflow built for this, we need to create one. As such, we first download the Workflow editor. You can do this from YouTrack itself, under the <em>Workflow </em>tab</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image2.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image"
 src="{{ site.images }}/wf-3.png" width="542" height="102"></a></p> <p align="left"> <p align="justify">Once download (it works on any platform as it’s based on Java), open it up. The first time you run it, it will display a hint box that the connection settings to the server need to be defined. You DO NOT need to have root permissions to the server to define workflow. You DO NEED however Administrative privileges on the Projects you want to use your workflows with.</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image3.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image"
  src="{{ site.images }}/wf-4.png" width="864" height="580"></a></p> <p align="left"> <p align="justify">1. Define these settings by clicking on the Connection Settings link. You can always change them later by clicking on the <em>Workflow | Connection Strings </em>menu entry.</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image4.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb4.png" width="550" height="300"></a></p> <p align="left"> <p align="justify">Once you verify the connection, you are ready to start defining some workflows. On first run, it will prompt you to download existing workflows from the server. If it does not or this is a second run, click on <em>Workflow | Download Workflows from Server</em>. A good reason to do this is that you will get all predefined workflows in YouTrack which serve as great examples for defining new ones.</p> <p align="left">If all goes well, you should end up with your Workflow explorer having some entries:</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image5.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb5.png" width="382" height="134"></a></p> <p align="left"> <p align="left">2. Define a new workflow</p> <p align="justify">The next step is to define a new Workflow. Every workflow we define is imported in to YouTrack as its own package. As such, a workflow allows us to group several specific rules together. We’ll get into what rules are in a moment.</p> <p align="left">To create a new Worfklow click on the Create Workflow icon on the toolbar</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image6.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb6.png" width="226" height="88"></a></p> <p align="left"> <p align="left">Give it a name. I’ve followed the convention to use server_name-project_name-description_of_workflow:</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image7.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb7.png" width="296" height="143"></a></p> <p align="left">Next step is to decide what type of workflow this is:</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image8.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb8.png" width="273" height="165"></a></p> <p align="justify">In this scenario, we’re going to create a <em>Stateless Rule</em> (we’ll cover the other two options in a future blog post). A Stateless Rule means it doesn’t hold any state. In our case, we want to make sure every time a field holds a specific value (Paused) a comment is required. We’re not interested at this point, what the previous value of the field was, so we won’t need to hold state.</p> <p align="justify">Here’s where it gets interesting. When you create the rule, a new editor is opened up. This is the rules editor. It’s a complete IDE (the power of MPS at work) that provides us with all the goodies such as syntax highlighting, code analysis and other things. First thing it prompts us for is a name.</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image9.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb9.png" width="397" height="156"></a></p> <p align="left"> <p align="justify">We’re going to call this rule <em>Changing to pause requires comments. </em>Note that for the rule name, spaces are allowed:</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image10.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb10.png" width="425" height="141"></a></p> <p align="left"> <p align="justify">The next step is to define the actual rule. Now as a newbie, it would kind of hard to know what can be done here. Fortunately, when we downloaded all the workflows, these all serve as examples. Added to that, we have Intellisense available too. Placing our cursor on the <strong>&lt;issue…&gt;</strong> and hitting Ctrl+Space, we get:</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image11.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb11.png" width="541" height="153"></a></p> <p align="left"> <p align="justify">This has been filtered down to everything starting with ‘S’ because we’re interested in dealing with a State change. We can see that we have the <em>State </em>entry available. If you’re wondering where these values come from, much like YouTrack Search Facility, it provides us with information not only on fields, but also on values of these fields. Entering <em>State </em>now displays:</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image12.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb12.png" width="715" height="214"></a></p> <p align="left"> <p align="justify">and we can select the <em>becomes </em>entry, which as the hint (albeit incorrectly) says, is an event that occurs when the value changes to a specific value. Unsurprisingly, <em>becomes </em>takes a value:</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image13.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb13.png" width="327" height="66"></a></p> <p align="left"> <p align="justify">which is the new value of the field. In our case this would be ‘Paused’. A couple of powerful things to notice before we continue. If we enter ‘Paused’ prior to creating that as a value on the server, the workflow editor will warn us that this value does not exist. It will even provide us with an <em>Intention </em>(in ReSharper known as QuickFix) to create this value for all projects:</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image14.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb14.png" width="402" height="95"></a></p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image15.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb15.png" width="330" height="280"></a></p> <p align="left"> <p align="justify">The next part is to define the actual action we want to take place when this condition is met. Here we can use the <em>assert </em>statement</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image16.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb16.png" width="300" height="64"></a></p> <p align="left"> <p align="left">filling in the condition and the message appropriately. In our case it would be</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image17.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb17.png" width="506" height="98"></a></p> <p align="left"> <p align="justify">With that, we’re pretty much done defining our rule. The only thing left to do is push it to the server to make it available and then use it in the project.</p> <p align="left">3. To upload the workflow with the server, we can select the <em>Workflow | Upload Workflows to Server </em>menu entry</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image18.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb18.png" width="284" height="253"></a></p> <p align="left"> <p align="left">waiting for the upload to finish</p> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image19.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb19.png" width="619" height="395"></a></p> <p align="left"> <p align="justify">4. Once that is complete, we then go back to YouTrack and attach the newly created workflow to the project. We select again the <em>Workflow </em>tab on the corresponding project and click on <em>Attach Workflow</em></p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image20.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb20.png" width="391" height="153"></a></p> <p align="left"> <p align="justify">If the uploading in Step 3 succeeded, we should now see our new Workflow in the Dialog box.</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image21.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb21.png" width="423" height="360"></a></p> <p align="left"> <p align="left"> <p align="justify">We’re done. Now when we change the state of an issue to Paused, it should display a message prompting for a comment</p> <p align="left"> <p align="left"><a href="http://hhariri.files.wordpress.com/2012/02/image22.png"><img style="background-image:none;padding-left:0;padding-right:0;display:inline;padding-top:0;border-width:0;margin:10px 0 0;" title="image" border="0" alt="image" src="http://hhariri.files.wordpress.com/2012/02/image_thumb22.png" width="609" height="44"></a></p> <p align="left"> <h3 align="left">Summary</h3> <p align="justify">In this post I gave you an introduction to the YouTrack Workflow editor and we saw how to create a simple rule, upload it to YouTrack and put it in action. What’s more, the editor is based on MPS and hopefully you have seen the power that MPS can offer us. In the next posts I’ll show you how to create more complex workflows that involve scheduling and state machines.</p>