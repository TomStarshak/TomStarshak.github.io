---
layout: post
title: Yolov3 Invisibility Cloak
---

# Yolo

Yolo (You only look once) is a very popular object detection neural network primarily developed by Joseph Redmon and Ali Farhadi at the University of Washington. The third version in particular was well
received for it's (near) real-time inference speed and wonderfully readable [paper](https://arxiv.org/pdf/1804.02767.pdf) (pdf). Redmon subsequently quit computer vision research citing military applications
and privacy concernt.
<blockquote class="twitter-tweet"><p lang="en" dir="ltr">I stopped doing CV research because I saw the impact my work was having. I loved the work but the military applications and privacy concerns eventually became impossible to ignore.<a href="https://t.co/DMa6evaQZr">https://t.co/DMa6evaQZr</a></p>&mdash; Joe Redmon (@pjreddie) <a href="https://twitter.com/pjreddie/status/1230524770350817280?ref_src=twsrc%5Etfw">February 20, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

I have a lot of respect for that, especially as it pertains to facial recognition. Doubly so if you consider the apparently genocidal abuses taking place in [certain locations](https://www.theverge.com/2020/12/8/22163499/huawei-uighur-surveillance-facial-recognition-megvii-uyghur).

# Adversarial Examples

So, given the potential for abuse, are there ways to defeat CV models? Yes! [OpenAI says](https://openai.com/blog/adversarial-example-research/#:~:text=Adversarial%20examples%20are%20inputs%20to,like%20optical%20illusions%20for%20machines.):

>Adversarial examples are inputs to machine learning models that an attacker has intentionally designed to cause the model to make a mistake; they’re like optical illusions for machines.

Basically, if you alter some pixels in an image in a clever way, you can cause the machine learning model to classify the image as something it isn't. For example, here's a cat that is mistaken for quacamole.

<center><img src='/public/adversarial_attack_counter.png'></center>

Even more interestingly, this can be done for [objects in the real world](https://www.theverge.com/2017/11/2/16597276/google-ai-image-attacks-adversarial-turtle-rifle-3d-printed), not just digital images. 
I imagine this is a very interesting field of research for the military (camoflage to defeat automated targeting systems?) but also has interest for activists, politicians, religious minorities, etc.
I'm unaware of inconspicuous articles of clothing that can defeat CV systems, but Tom Goldstein at the University of Maryland developed a [shirt](https://www.cs.umd.edu/~tomg/projects/invisible/) that can defeat Yolov2 and a different one for Yolov3. They are available for purchase, so I doled out some money for the Yolov3 version so I could test it myself.

# Invisibility Cloak

<center><div style="width:400px"><img src='https://i.rocdn.com/v2/102717221?w=1024&h=1024' style="max-width:100%;"></div></center>


So, does it work? What about if I'm moving, wearing a scarf or jacket, or turned sideways? Does the video resolution matter?

First let me thank [theAIGuy](https://www.youtube.com/channel/UCrydcKaojc44XnuXrfhlV8Q), as I used his very easy Yolo [implementation](https://github.com/theAIGuysCode/Object-Detection-API) for these experiments.

### Baseline
<center><div style="width:400px"><img src='/public/invisibility_cloak_pics/base480p.gif' style="max-width:100%;"></div></center>

Huh, wasn't expecting that. That's with the full 62M parameter model. There's also a tiny model with ~9M parameters.

<center><div style="width:400px"><img src='/public/invisibility_cloak_pics/base480ptiny.gif' style="max-width:100%;"></div></center>

In this case the invisibility shirt does seem to be helping, although the detector still pics up some frames with the shirt on and misses some beforehand.

### Accessories
Here I try adding a scarf and a jacket.
<center><div style="width:400px"><img src='/public/invisibility_cloak_pics/scarf480p.gif' style="max-width:100%;"><img src='/public/invisibility_cloak_pics/scarf480ptiny.gif' style="max-width:100%;"></div></center>
<center><div style="width:400px"><img src='/public/invisibility_cloak_pics/jacket480p.gif' style="max-width:100%;"><img src='/public/invisibility_cloak_pics/jacket480ptiny.gif' style="max-width:100%;"></div></center>

So the shirt is definitely ineffective against the full Yolov3 model, but works quite well against the tiny model. The jacket does not affect the ability to fool the model, but the scarf does when covering the center of the shirt.


### Thoughts

Definitely an interesting technology. I'll have to do more research, but I believe that it's possible to create an adversarial example against an arbitrary number of models or ensemble of models. If so, then a shirt could be made that is effective against the full Yolo model as well as the tiny model, and indeed, against other detection models as well.