---
layout: page
title: Setup
permalink: /setup/
contact: ADD CONTACT EMAIL
---
### Requirements:
Data Carpentry's teaching is hands-on, so participants are encouraged to bring in and use their own laptops to insure the proper setup of tools for an efficient workflow once you leave the workshop.  (We will provide instructions on setting up the required software several days in advance).

> ## Prerequisites
> There are no pre-requisites, and we will assume no prior knowledge about the tools.
{: .prereq}


### Contact:
Please email
{% if page.contact %}
  <a href='mailto:{{page.contact}}'>{{page.contact}}</a>
{% else %}
  <a href='mailto:{{site.contact}}'>{{site.contact}}</a>
{% endif %}
for questions and information not covered here.

### Setup

To participate in a Data Carpentry workshop,
you will need working copies of the software described below.
Please make sure to install everything and try opening it to make sure it works
**before** the start of your workshop. If you run into any problems,
please feel free to email the instructor or arrive early to your workshop on
the first day.

Participants should bring and use their own laptops to insure the proper setup of
tools for an efficient workflow once you leave the workshop.

This workshop will be using the software outlined in the install instructions below.
Please see the section for your operating system for those directions.
 - [Windows](#windows)
 - [Mac](#mac)
 - [Linux](#linux)

{% include windows.html %}
{% include mac.html %}
{% include linux.html %}
