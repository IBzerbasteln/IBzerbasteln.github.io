---
layout: post
title: a post with bibliography
date: 2023-07-12 09:56:00-0400
description: an example of a blog post with bibliography
tags: formatting bib
categories: sample-posts
giscus_comments: true
related_posts: false
related_publications: true
---

This post shows how to add bibliography to simple blog posts. We support every citation style that [jekyll-scholar](https://github.com/inukshuk/jekyll-scholar) does. That means simple citation like {% cite stewens_return_2022 %}, multiple citations like {% cite dunne_ethics_2022 stewens_burmese_2021 %}, long references like {% reference stewens_law_2024 %} or also quotes:

{% quote stewens_law_2024 %}
Lorem ipsum dolor sit amet, consectetur adipisicing elit,
sed do eiusmod tempor.

Lorem ipsum dolor sit amet, consectetur adipisicing.
{% endquote %}

If you would like something more academic, check the [distill style post]({% post_url 2018-12-22-distill %}).
