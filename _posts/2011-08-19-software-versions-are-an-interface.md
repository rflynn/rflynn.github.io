---
layout: post
title: "Software Versions Are An Interface"
date: 2011-08-19
summary: Version numbers have been in the open-source world's news lately. Mostly for the wrong reasons.
---

<p>
Version numbers have been in the open-source world's news lately. Mostly for the wrong reasons.
</p>

<style type="text/css">
th { background-color: #eef6ee }
</style>

<p>
<a href="http://en.wikipedia.org/wiki/Software_versioning">Software version numbers</a>
seem like a trivial subject, not worthy of serious consideration or debate.
It seems obvious enough &mdash; when you release new stuff the numbers go up, right?
What is there to talk about?
But version numbers <em>are</em> important. They are an interface as important
as any specification, protocol or library.  They communicate implications to
your customers, your clients, your users and tell them what to expect.
<p>
<b>Software versions are, in business terms,
<em><a href="http://www.merriam-webster.com/dictionary/actionable">actionable</a> data</em>,
indicating magnitude of change and thus approximate risk/rewards.</b>
<p>
Based on this information I can make a choice if I want to update, and how and when
to do so to mitigate risk.
A stable and sensible roadmap allows your customers, managers and developers to plan.
<p>
<h2 class="head-line">How Software Version Numbers Work</h2>
Software version numbers are typically composed of 3 components: major, minor and point release, in the form <i>x.y.z</i>.
<ul>
  <li>1.0 is a major version
  <li>1.1 is a minor update from 1.0
  <li>1.1.1 is a point release from 1.1
</ul>
<p>
<h3>Software Version Number Components and Their Meaning</h3>
<table class="border">
<tr>
  <td style="border:0">
  <th>Major
  <th>Minor
  <th>Point Release
<tr>
  <th>Reward
  <td>Major improvements that warrant potentially incompatible external interfaces
  <td>Feature improvement/addition to internal components
  <td>Bug fixes &mdash; removal/reduction of crashes, regressions or potential security threats. Otherwise, it can wait.
<tr>
  <th>Risk
  <td>Everything can break. Migration/update may be costly and require outside expertise.
  <td>Minimal
  <td>Minimal
<tr>
  <th>Implications of <u>Not</u> Updating
  <td>Eventual loss of commercial support
  <td>Potential loss of performance/feature improvements
  <td>Immediate/existing instability and/or security risk
<tr>
  <th>Approximate Decision Timeframe
  <td>Annual
  <td>Quarterly
  <td>Weekly
<tr>
  <th>Incompatibility
  <td>Acceptable (migration)
  <td>Not acceptable
  <td>Not acceptable
<tr>
  <th>Money
  <td>Commercial software re-licensing and support changes expected
  <td>No, existing licensing and support should not change
  <td>No
<tr>
  <th>Training
  <td>Administrator and user training expected to some degree.
  <td>Maybe some for administrators re: configuration
  <td>No
</table>
<p>
Far be it for me to dictate terms to software developers,
<a href="http://en.wikipedia.org/wiki/Software_versioning">there are many software versioning schemes out there</a>;
the important thing to realize is that the practical ones all account for a realistic
<a href="http://en.wikipedia.org/wiki/Software_release_life_cycle">software release cycle</a>,
which in turn accounts for <a href="http://en.wikipedia.org/wiki/Decision-making">decision-making</a>
within imperfect organizations composed of imperfect people.
<p>
And a lot of respectable real-world products indeed follow this pattern or
something like it.
<a href="software-version-anti-patterns">But not all...</a>
<p>
<h3>See Also</h3>
<ul>
  <li><a href="http://semver.org/">Semantic Versioning</a>
</ul>
