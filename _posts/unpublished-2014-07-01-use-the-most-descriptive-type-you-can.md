---
layout: post
title: "Use the Most Descriptive Type You Can"
date: 2014-07-01
---

If you find yourself commenting on a value's units, delete the comment and reflect the unit in the code itself.

Strolling along one day in <a href="http://en.wikipedia.org/wiki/C_Sharp_(programming_language)">C#</a> land&hellip;

<code style="white-space:pre">// 15 minutes in milliseconds
const int RefreshDelayTime = 1000 * 60 * 15;
</code>

It's not <em>bad</em> &mdash; it <em>has</em> comments, and they're correct &mdash; but elsewhere in the program this is just a naked C-like int floating around waiting to be misused.
<p>
You have to hope the value is always used properly as milliseconds &mdash; and even dicier &mdash; converted properly to other units of time.

<p>

Instead, use a built-in type that allows an unambiguous and yet more concise representation of your value and nuke the redundant type-esque <code>&hellip;Time</code> suffix.

<p>
<code>var RefreshDelay = TimeSpan.FromMinutes(15);</code>
</p>

The unit of time is explicit on the way in and the way out: <code>Convert.ToInt32(RefreshDelay.TotalMilliseconds)</code>

Oh, and <code>const var RefreshDelay</code> would be even better, but C# isn't that sophisticated.


<h3>References</h3>
<ul>
    <li><a href="http://www.craigkerstiens.com/2014/05/07/Postgres-datatypes-the-ones-youre-not-using/">Postgres Datatypes – the Ones You're Not Using.</a>
    <ul>
        <li><a href="http://www.postgresql.org/docs/8.4/static/datatype.html">PostreSQL Data Types</a>
    </ul>
    <li><a href="http://msdn.microsoft.com/en-us/library/system.aspx#Structures">System structures</a>
</ul>
