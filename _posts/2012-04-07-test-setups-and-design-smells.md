---
layout: post
title: Test setups and design smells
categories:
- General
tags:
- Architecture
- Design
- Unit Testing
status: publish
type: post
published: true
meta:
  reddit: a:2:{s:5:"count";i:0;s:4:"time";i:1366805828;}
  tagazine-media: a:7:{s:7:"primary";s:53:"http://hhariri.files.wordpress.com/2012/04/image6.png";s:6:"images";a:6:{s:53:"http://hhariri.files.wordpress.com/2012/04/image5.png";a:6:{s:8:"file_url";s:53:"http://hhariri.files.wordpress.com/2012/04/image5.png";s:5:"width";s:3:"794";s:6:"height";s:3:"418";s:4:"type";s:5:"image";s:4:"area";s:6:"331892";s:9:"file_path";s:0:"";}s:59:"http://hhariri.files.wordpress.com/2012/04/image_thumb5.png";a:6:{s:8:"file_url";s:59:"http://hhariri.files.wordpress.com/2012/04/image_thumb5.png";s:5:"width";s:3:"794";s:6:"height";s:3:"418";s:4:"type";s:5:"image";s:4:"area";s:6:"331892";s:9:"file_path";s:0:"";}s:53:"http://hhariri.files.wordpress.com/2012/04/image6.png";a:6:{s:8:"file_url";s:53:"http://hhariri.files.wordpress.com/2012/04/image6.png";s:5:"width";s:3:"680";s:6:"height";s:3:"609";s:4:"type";s:5:"image";s:4:"area";s:6:"414120";s:9:"file_path";s:0:"";}s:59:"http://hhariri.files.wordpress.com/2012/04/image_thumb6.png";a:6:{s:8:"file_url";s:59:"http://hhariri.files.wordpress.com/2012/04/image_thumb6.png";s:5:"width";s:3:"680";s:6:"height";s:3:"609";s:4:"type";s:5:"image";s:4:"area";s:6:"414120";s:9:"file_path";s:0:"";}s:53:"http://hhariri.files.wordpress.com/2012/04/image4.png";a:6:{s:8:"file_url";s:53:"http://hhariri.files.wordpress.com/2012/04/image4.png";s:5:"width";s:3:"519";s:6:"height";s:3:"470";s:4:"type";s:5:"image";s:4:"area";s:6:"243930";s:9:"file_path";s:0:"";}s:59:"http://hhariri.files.wordpress.com/2012/04/image_thumb4.png";a:6:{s:8:"file_url";s:59:"http://hhariri.files.wordpress.com/2012/04/image_thumb4.png";s:5:"width";s:3:"519";s:6:"height";s:3:"470";s:4:"type";s:5:"image";s:4:"area";s:6:"243930";s:9:"file_path";s:0:"";}}s:6:"videos";a:0:{}s:11:"image_count";s:1:"6";s:6:"author";s:7:"5078411";s:7:"blog_id";s:8:"11677451";s:9:"mod_stamp";s:19:"2012-04-07
    17:25:44";}
  _elasticsearch_indexed_on: '2012-04-07 17:18:23'
---
<p>Reviewing some code today, I keep stumbling across private methods that are encapsulating test setups. A probable reason for this is that the authors want to prevent having to repeat the same code over and over again. It seems like common sense. I mean after all, that’s one way of complying with DRY.&nbsp; </p> <p>The problem however is that often this kind of encapsulation is hiding an underlying design problem.</p> <p>As an example, I’m going to use one of my all time favorite code-bases that I reference in talks I give on MVC and design in general:</p> <p>&nbsp;</p> <p><a href="http://hhariri.files.wordpress.com/2012/04/image5.png">
<img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:inline;border-top:0;border-right:0;padding-top:0;" title="image" border="0" alt="image" src="{{ site.images }}/tsd-1.png" width="794" height="418"></a></p> <p>&nbsp;</p> <p>Look at the <em>Authenticated_Shipping_With_EmtpyCart</em> test. It’s short and straightforward. However, notice how the test class has a <em>TestBase: </em></p> <p><em></em>&nbsp;</p> <p><a href="http://hhariri.files.wordpress.com/2012/04/image6.png"><img style="background-image:none;border-bottom:0;border-left:0;margin:0;padding-left:0;padding-right:0;display:inline;border-top:0;border-right:0;padding-top:0;" title="image" border="0" alt="image" src="{{ site.images }}/tsd-2.png" width="680" height="609"></a></p> <p>&nbsp;</p> <p>I chopped off the remaining part as it wouldn’t fit on the screen. You can see the <a href="http://erpstore.codeplex.com/SourceControl/changeset/view/88814#1575530">entire class here</a>.&nbsp; It’s just wrong on so many levels.</p> <p>If you look at the controller this is testing (<em>CheckoutController</em>), you’ll see:</p> <p><a href="http://hhariri.files.wordpress.com/2012/04/image4.png"><img style="background-image:none;border-bottom:0;border-left:0;padding-left:0;padding-right:0;display:inline;border-top:0;border-right:0;padding-top:0;" title="image" border="0" alt="image" src="{{ site.images }}/tsd-3.png" width="519" height="470"></a></p> <p>&nbsp;</p> <p>If I were to write that out every time I would need to test the controller, alarm signals would raise that I’m doing something wrong. If I’m hiding it away in a base class or a method, it’s harder to notice.&nbsp; </p> <p>So next time you feel the urge to encapsulate some setup in a method or base class, ask yourself why. If you don’t come up with a good reason, re-think your design. </p> <p>&nbsp;</p> <p><strong>Side-note</strong>: Some people have lately been complaining that IoC Containers lead to these kind of design smells also, allowing developers to by injection-happy when dealing with dependencies. It might be true, although I believe that if you are correctly writing the necessary tests, you’d notice this problem early on. The code above also uses a container in the tests (and doesn’t even use conventions for setting it up). That’s another reason I’m against using containers in <em>unit </em>tests<em>. </em></p>