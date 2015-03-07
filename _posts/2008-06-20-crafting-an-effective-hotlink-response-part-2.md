---
layout: post
title: "Crafting An Effective Hotlink Response, Part 2"
date: 2008-06-20
summary: I once believed an image response could shame or punish hotlinkers. It can't and won't. Here's what to do instead.
---

<p>
I once believed an image response could <a href="http://www.parseerror.com/blog/crafting-an-effective-hotlink-response-part-1">shame or punish hotlinkers</a>. It can't and won't. Here's what to do instead.
</p>

<p>
You can't shame or punish hotlinkers: others <em>will hotlink your hotlink image for themselves</em>. Your own effort redirected against you, like jujitsu.Therefore, the most effective response is nothing;
instruct our webserver to deny image requests that don't come from us.
In the widely-used <a href="http://httpd.apache.org/docs/2.2/">Apache webserver</a> it's called a <a href="http://httpd.apache.org/docs/2.2/mod/mod_rewrite.html">RewriteRule</a> and it looks like this:
<p>
Contents of <code>.htaccess</code> file in your site's directory:
<blockquote>
<table>
<tr><td style="color:#ccc">
<pre>
1
2
3
4
5
</pre>

<td style="border-left:1px solid #ccc; padding-left:5px">
<pre class="code">
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} (jpe?g|gif|png)$ [NC]
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^https?://([^.]+\.)*example.com/ [NC]
RewriteRule (.*) - [F,L]
</pre>
</table>
</blockquote>
<p>
The lines translate to:
<ol>
	<li>enable rewriting feature
	<li>if an image is requested...
	<li>from another website...
	<li>and if isn't <em>my</em> site (example.com)...
	<li>then Forbid that request
</ol>
<p>
Notes:
<ul>
	<li>change "example.com" on line 4 to your own website's address.
	<li>you may include multiple copies of line 4, one for each domain/website you host, or want to allow to hotlink. i do this for a few select places.
</ul>

There exists <a href="http://www.google.com/search?q=hotlink+redirect">many better explanations of this technique</a>.
<p>
<h2>Food for thought</h2>
By far the worst offenders are high-traffic msgboards that allow images. Users of these boards often do not even have their own site to host from, so hotlinking is the most straight-forward way of expressing themselves. A solution would be for msgboard sites to copy images hosted elsewhere and host them themselves, but that would cost them bandwidth. Another would be to require image URLs to be on a whitelist of domains that allow hotlinking, such as <a href="http://www.google.com/search?q=image+hosting+site">dedicated image hosting site</a>s.
<p>
I've considered attempting to punish hotlinkers by redirecting requests back at them. We could target the offending page, or possibly some other target such as /, /favicon.ico, /style.css. The goal being to exact a hotlink-allowance tax in the form of bandwidth. We could even target an intentionally non-existing file on the host such as /some-really-long-rude-string-that-tells-them-not-to-allow-hotlinks-and-uses-up-as-much-disk-space-as-possible.jpg in order to accumulate offending entries in their webserver's error log. This concept seems a bit juvenile, but then so is the web.
