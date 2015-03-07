---
layout: post
title: "Visualizing Unicode Codespace"
date: 2013-01-03
summary: Unicode defines a vast address space for all the world's languages — but what does it all look like, exactly?
---

Unicode defines a vast address space for all the world's languages &mdash; but what does it all look like, exactly?
<p>

Unicode at 1 pixel per <a href="http://en.wikipedia.org/wiki/Code_point">code point</a>. Click on the squares.

<p style="clear:both">

<div style="float:left">
<embed src="/vis/unicode-codespace/vis-unicode-codespace-1280.svg" type="image/svg+xml" width="1280" height="1024"></embed>
</div>

<p style="clear:both">

<div style="max-width:273px; float:right; margin-left:1em; margin-right:0; margin-bottom:5px; background-color:#fdfdea; border:1px solid #ccc; padding:3px">
    <a href="http://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane"><img src="/vis/unicode-codespace/Roadmap_to_Unicode_BMP.png" width="273" height="273" border="0"></a>
<a href="http://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane">Wikipedia vis</a>, not bad &#x263A
</div>


Wikipedia has a nice
<a href="http://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane">space-fill block layout</a> for the <i>Basic Multilingual Plane</i>. For me it's instantly intuitive, but it isn't interactive, the address labels distract, the range name labels are too far from the data and the ranges don't directly correspond with the canonical <a href="http://www.unicode.org/Public/UNIDATA/Blocks.txt">Unicode Block names</a>.

<h3>What I see</h3>
<ul>
    <li>Unicode codespace is vastly empty
    <li>Even within the 3 first, mostly-populated planes there are unexplained gaps. Perhaps padding for future expansion, or previously-used space revoked? <a href="http://en.wikipedia.org/wiki/Resource_allocation">Resource allocation</a> is hard.
    <li>Technical note about the visualization itself: the image looks noticeably different on each of Chrome, Firefox, Safari, Internet Explorer and Opera. It looks best in Chrome and Safari and worst in Internet Explorer.
</ul>

<h3>Other Visualizations</h3>
<ul style="list-style-image:url(/i/icon/16/wikipedia.png)">
    <li><a href="http://en.wikipedia.org/wiki/Plane_(Unicode)#Basic_Multilingual_Plane">Nice space-fill block layout of the Basic Multilingual Plane</a>
    <li><a href="http://en.wikipedia.org/wiki/Unicode#Code_point_planes_and_blocks">Table-based summary of Unicode planes and code point ranges</a> &mdash; condenses the unused Planes nicely, something my vis doesn't do
    <li><a href="http://en.wikipedia.org/wiki/UTF-8#Description">Layout of UTF-8 variable-length encoding</a>
</ul>

<h3>Links</h3>
<ol>
    <li><a href="http://www.unicode.org/Public/UNIDATA/Blocks.txt">Unicode Character Databases: Blocks.txt</a>
    <li><a href="vis-unicode-codespace/vis-unicode-codespace.svg">fullsize interactive SVG image</a>
    <li><a href="https://github.com/rflynn/vis-unicode-codespace">project on github with raw data and a python script for generating the visualization</a>
</ol>
