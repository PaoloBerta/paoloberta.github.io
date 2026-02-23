---
layout: page
permalink: /publications/
title: Publications
description: Publications by categories in reversed chronological order, distinguished by selected papers, other papers, and book chapters.
nav: true
nav_order: 2
---
<!-- _pages/publications.md -->
<!-- Bibsearch Feature -->
{% include bib_search.liquid %}

<div class="publications">

<h2>Main Publications</h2>
{% bibliography --query @*[keywords!=selected] %}

<h2>Other Publications</h2>
{% bibliography --query @*[keywords=selected] %}

</div>
