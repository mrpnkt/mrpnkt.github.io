---
title: Security Thinking
date: '2016-04-03 00:00:00'
categories: []
layout: post
tags:
- homework
- security
slug: security-thinking
---
This week i watched [a documentary](https://www.youtube.com/watch?v=ITEXGdht3y8) about the Chernobyl Disaster and since i had just read through the teachings of Sun Tzu and listened to the first lectures of Comp3/9441 a short review of this event from a security perspective seemed appropiate for this weeks blog entry.

I don't know much about nuclear physics and i will not go too deep into what exactly happened inside the reactor, I will instead address a few points of failure worth remarking even 30 years later and very related to security thinking and a security engineering mindset.

> "Security is all about **'how does a system behave when we are out of spec?'**"

Richard Buckland

> “The odds of a meltdown are one in 10,000 years. The plants have safe and reliable controls that are protected from any breakdown with three safety systems.”

Vitali Sklyarov, Minister of Power and Electrification of Ukraine (Feb, 1986)

![]({{ site.baseurl }}/forestryio/images/Screenshot - 03222016 - 032730 PM.png)

The events that led to the meltdown of reactor 4 on april 26, 1986 are related to a test procedure. It was [intented to confirm the capability of bridging a 45 second gap](http://chernobylgallery.com/chernobyl-disaster/cause/) between an external power failure and full power from emergency generators.

The test procedure was to begin with an automatic emergency shutdown. No detrimental effect on the safety of the reactor was anticipated, so the test program was not formally coordinated with either the chief designer of the reactor or the scientific manager. Instead, it was approved only by the director of the plant (and even this approval was not consistent with established procedures). In fact the test had been conducted 3 times before with negative results and should have been completed before the reactor even opened. The station managers presumably wished to correct this at the first opportunity, which may explain why they continued the test even when serious problems arose, and why the requisite approval for the test had not been sought from the Soviet nuclear oversight regulator.

> "The reactor was in an unstable configuration that was clearly outside the safe operating envelope established by the designers."

[Wikipedia](https://en.wikipedia.org/wiki/Chernobyl_disaster#Conditions_before_the_accident)

According to a [report released in 1992 by the IAEA](http://www-pub.iaea.org/MTCD/publications/PDF/Pub913e_web.pdf) Nuclear Safety Advisory Group (INSAG) the main reasons for the accident lie in the "peculiarities of physics and in the construction of the reactor": The design was very unstable at low power levels, and prone to suddenly increasing energy production to a dangerous level. This counter-intuitive behaviour and property of the reactor was unknown to the crew.

Moreover the report states the **inadequate “culture of safety”** at all levels. Deficiency in the safety culture was inherent not only at the operational stage but also, and to no lesser extent, during activities at other stages in the lifetime of nuclear power plants (including **design, engineering, construction, manufacture and regulation**).

Little known fact and tragic irony: the motivation leading the soviet government to issue tests on all its reactors was the bombing of russian build iraqui nuclear reactor by israeli airforce a few years before Chernobyl. Russian scientists demanded test to see what would happen if their energy system came under attack.

What really happened was fifty tons of nuclear fuel blown into the atmosphere in the largest nuclear disaster the world has witnessed.

> "If words of command are not clear and distinct, if orders are not thoroughly understood, the general is to blame. But if his orders are clear, and the soldiers nevertheless disobey, then it is the fault of their officers."

Sun Tzu

The poor quality of operating procedures, command chain and instructions, and their conflicting characters suggest human failure as main cause for the catastrophe, but failure by design and little foreknowledge is more likely to be the probable cause.

![]({{ site.baseurl }}/forestryio/images/Screenshot - 03222016 - 044814 PM.png)

As the INSAG-Report concludes: “The accident can be said to have flowed from a deficient safety culture, not only at the Chernobyl plant, but throughout the Soviet design, operating and regulatory organizations for nuclear power that existed at that time.”

> “To ... not prepare is the greatest of crimes; to be prepared beforehand for any contingency is the greatest of virtues."

Sun Tzu

The art of war teaches us to rely not on the likelihood of the enemy's not coming, but on our own readiness to receive him; not on the chance of his not attacking, but rather on the fact that we have made our position unassailable.

> On April 27, the residents of Pripyat were evacuated — about 36 hours after the accident had occurred. By that time, many were already complaining about vomiting, headaches and other signs of radiation sickness. Officials eventually closed off an 18-mile (30 km) area around the plant; residents were told they would be able to return after a few days, so many left their personal belongings and valuables behind.

[livescience](http://www.livescience.com/39961-chernobyl.html)

Chernobyl is a perfect example of mitigation done wrong: The power plant lacked a proper containment structure. At any moment the public was not fully informed; workers, firefighters, soldiers and volunteers[ high dosis of radiation](http://chernobylgallery.com/chernobyl-disaster/radiation-levels/) when sent to seal the fallout aftermath. And while 'only' two workers died in the explosions many died hours, months and years later. [The magnitude](https://metofficenews.files.wordpress.com/2011/04/chernobylcaesium-600.gif) of the incident came to light when Swedish nuclear power authorities noted unusually high rates of radiation in routine measurements at a plant north of Sweden.Chernobyl is a perfect example of mitigation done wrong: The power plant lacked a proper containment structure. At any moment the public was not fully informed; workers, firefighters, soldiers and volunteers[ high dosis of radiation](http://) when sent to seal the fallout aftermath. And while 'only' two workers died in the explosions many died hours, months and years later. The magnitude of the incident came to light when Swedish nuclear power authorities noted unusually high rates of radiation in routine measurements at a plant north of Sweden.

Chernobyl happened 30 ago and fifty-seven accidents have occurred since. While the actual impact of nuclear accidents to nature, economy and society may be a subject to debate, there are still many lessons that have to been learned.

![]({{ site.baseurl }}/forestryio/images/stuxnet-1024x549.png)

[In the](http://www.businessinsider.com/stuxnet-was-far-more-dangerous-than-previous-thought-2013-11?op=1) [wake of](http://spectrum.ieee.org/telecom/security/the-real-story-of-stuxnet) [Stuxnet](http://arstechnica.com/tech-policy/2012/06/confirmed-us-israel-created-stuxnet-lost-control-of-it/) the importance to think about, assess, fix and mitigate the known and unkown vulnerabilities in our [energy supply system](https://krypt3ia.wordpress.com/2016/03/05/%d0%bf%d1%80%d0%b8%d0%ba%d0%b0%d1%80%d0%bf%d0%b0%d1%82%d1%82%d1%8f%d0%be%d0%b1%d0%bb%d0%b5%d0%bd%d0%b5%d1%80%d0%b3%d0%be-the-first-attack-on-infrastructure/) cannot be stressed enough. Chernobyl showed us what could happen when the public is not aware of the inherent risks that emerge with revolutionary technologies.
