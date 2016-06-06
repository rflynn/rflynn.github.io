---
layout: post
title: "Silent Truncation is Very, Very Ba"
date: 2012-05-03
summary: Silent truncation is when a third party entrusted with storing your data decides to toss some of it away without telling you. It's insane behavior and it happens all the time.
---

Silent truncation is when a third party entrusted with storing your data
decides to toss some of it away without telling you.
It's insane behavior and it happens all the time.
<p>

The crux of the matter is that data should never be irreversibly and
unaccountably modified as a side effect of storage.
So which common programming tools will leave your data on the cutting room
floor and which won't?


<a name="ints"></a>
<h2>Integers</h2>

<p>

Integers are a fundamental data type.
Since <a href="http://en.wikipedia.org/wiki/Integer_overflow">integer overflow</a>
is a frequent problem many tools do at least some checking.
Some tools automatically convert to floating point, which is a lossy operation.

<p>

Let's test printing 2<sup>64</sup> (18446744073709551616)

<p>

Tool | In | Out | Error
-----|----|-----|-------
<a href="http://en.wikipedia.org/wiki/C_(programming_language)">C</a> (gcc 4.6) | <code>printf("%llu", 18446744073709551616ULL);</code> | foo.c:4:20: warning: integer constant is too large for its type [enabled by default] | &mdash;
<a href="http://en.wikipedia.org/wiki/Java_(programming_language)">Java</a> 1.6 | <code>System.out.format("%u", 18446744073709551616);</code> | foo.java:3: integer number too large: 18446744073709551616 | &mdash;
<a href="http://en.wikipedia.org/wiki/C_Sharp_(programming_language)">C#</a> | <code>System.Console.WriteLine("{0}", 18446744073709551615);</code> | foo.cs(5,74): error CS1021: Integral constant is too large | &mdash;
<a href="http://www.php.net/">PHP</a> 5.3 | <code>echo 18446744073709551616;</code> | 1.844674407371E+19 | 448384
<a href="http://www.php.net/">PHP</a> 5.3 | <code>printf("%u", 18446744073709551616);</code> | 0 | 18446744073709551616
<a href="http://python.org/">Python</a> 2.7 | <code>print 18446744073709551616</code> | 18446744073709551616 | 0
<a href="http://en.wikipedia.org/wiki/JavaScript">Javascript</a> | <code>18446744073709551616</code> | 18446744073709552000 | 384
<a href="http://perl.org/">Perl</a> 5.14 | <code>print 18446744073709551616</code> | 1.84467440737096e+19 | 48384
<a href="http://ruby-lang.org/">Ruby</a> 1.9 | <code>p 18446744073709551616</code> | 18446744073709551616 | 0
<a href="http://www.postgresql.org/">Postgresql</a> 9.1 | <code>select 18446744073709551616;</code> | 18446744073709551616 | 0
<a href="http://sqlite.org/">sqlite</a>3 | <code>select 18446744073709551616;</code> | 1.84467440737096e+19 | 48384
<a href="http://www.mysql.com/">MySQL</a> 5.5 | <code>select 18446744073709551616;</code> | 18446744073709551616 | 0

<p>

github uses kramdown, which can't actually render tables because its programmers are hipster twats, which is why the rest of this page looks like shit

<table class="borderrows" width="100%">

<tr>
    <th>Tool
    <th>In
    <th>Out
    <th>Error

<!--tr>
    <td>C
    <td>
<code>
int64_t n = 9223372036854775808;<br>
printf("%" PRId64 "\n", n);
</code>
    <td class="error">-9223372036854775808
-->

<tr>
    <td><a href="http://en.wikipedia.org/wiki/C_(programming_language)">C</a> (gcc 4.6)
    <td><code>printf("%llu", 18446744073709551616ULL);</code>
    <td width="30%">foo.c:4:20: warning: integer constant is too large for its type [enabled by default]
    <td align="center">&mdash;

<tr>
    <td><a href="http://en.wikipedia.org/wiki/Java_(programming_language)">Java</a> 1.6
    <td><code>System.out.format("%u", 18446744073709551616);</code>
    <td>foo.java:3: integer number too large: 18446744073709551616
    <td align="center">&mdash;

<tr>
    <td><a href="http://en.wikipedia.org/wiki/C_Sharp_(programming_language)">C#</a>
    <td><code>System.Console.WriteLine("{0}", 18446744073709551615);</code>
    <td>foo.cs(5,74): error CS1021: Integral constant is too large
    <td align="center">&mdash;

<tr>
    <td><a href="http://www.php.net/">PHP</a> 5.3
    <td><code>echo 18446744073709551616;</code>
    <td class="error">1.844674407371E+19
    <td class="error" align="right">448384

<tr>
    <td><a href="http://www.php.net/">PHP</a> 5.3
    <td><code>printf("%u", 18446744073709551616);
    <td class="error">0
    <td class="error" align="right"><small>18446744073709551616</small>

<tr>
    <td><a href="http://python.org/">Python</a> 2.7
    <td><code>print 18446744073709551616</code>
    <td>18446744073709551616
    <td align="right">0

<tr>
    <td><a href="http://en.wikipedia.org/wiki/JavaScript">Javascript</a>
    <td><code>18446744073709551616</code>
    <td class="error">18446744073709552000
    <td class="error" align="right">384

<tr>
    <td><a href="http://perl.org/">Perl</a> 5.14
    <td><code>print 18446744073709551616</code>
    <td class="error">1.84467440737096e+19
    <td class="error" align="right">48384

<tr>
    <td><a href="http://ruby-lang.org/">Ruby</a> 1.9
    <td><code>p 18446744073709551616</code>
    <td>18446744073709551616
    <td align="right">0

<tr>
    <td><a href="http://www.postgresql.org/">Postgresql</a> 9.1
    <td><code>select 18446744073709551616;</code>
    <td>18446744073709551616
    <td align="right">0

<tr>
    <td><a href="http://sqlite.org/">sqlite</a>3
    <td><code>select 18446744073709551616;</code>
    <td class="error">1.84467440737096e+19
    <td class="error" align="right">48384

<tr>
    <td><a href="http://www.mysql.com/">MySQL</a> 5.5
    <td><code>select 18446744073709551616;</code>
    <td>18446744073709551616
    <td align="right">0

</table>

<p>

<a name="float"></a>
<h2>Floating Point Numbers</h2>

<p>

Floating point numbers are often
<a href="http://stackoverflow.com/questions/3730019/why-not-use-double-or-float-to-represent-currency">used incorrectly in business-type applications</a> by
<a href="http://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html">programmers who do not really understand them</a>.
While
<a href="http://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html">floating point rounding can be taken into effect when one is careful</a>,
many tools silently truncate and/or round values differently.

<p>

Let's test printing <a href="http://en.wikipedia.org/wiki/Pi" style="font:x-large serif" alt="Pi">&pi;</a> to 17 decimal places (3.141 592 653 589 793 23)

<p>

<table class="borderrows" width="100%">

<tr>
    <th>Tool
    <th>In
    <th>Out
    <th>Error

<tr>
    <td>C
    <td><code>printf("%.17f", 3.14159265358979323);</code>
    <td>3.141592653589793<a class="error">12</a>
    <td class="error" align="right" nowrap>2.1e-16

<tr>
    <td>Java 1.6
    <td><code>System.out.format("%.17f", 3.14159265358979323);</code>
    <td>3.141592653589793<a class="error">00</a>
    <td class="error" align="right" nowrap>2.3e-16

<tr>
    <td>C#
    <td><code>string.Format("{0:0.00000000000000000}", 3.14159265358979323);</code>
    <td>3.14159265358979<a class="error">000</a>
    <td class="error" align="right" nowrap>3.23e-15

<tr>
    <td>PHP 5.3
    <td><code>printf("%.17f", 3.14159265358979323);</code>
    <td>3.141592653589793<a class="error">12</a>
    <td class="error" align="right" nowrap>2.1e-16

<tr>
    <td>Python 2.7
    <td><code>'%.17f' % 3.14159265358979323</code>
    <td>3.141592653589793<a class="error">12</a>
    <td class="error" align="right" nowrap>2.1e-16

<tr>
    <td>Javascript
    <td><code>console.log(3.14159265358979323)</code>
    <td>3.141592653589793<a class="error">__</a>
    <td class="error" align="right" nowrap>2.3e-16

<tr>
    <td>Perl 5.14
    <td><code>printf "%.17f", 3.14159265358979323</code>
    <td>3.141592653589793<a class="error">12</a>
    <td class="error" align="right" nowrap>2.1e-16

<tr>
    <td>Ruby 1.9
    <td><code>printf "%.17f", 3.14159265358979323</code>
    <td>3.141592653589793<a class="error">12</a>
    <td class="error" align="right" nowrap>2.1e-16

<tr>
    <td>Postgresql 9.1
    <td><code>select 3.14159265358979323;</code>
    <td>3.14159265358979323
    <td align="right">0

<tr>
    <td>sqlite3
    <td><code>select 3.14159265358979323;</code>
    <td>3.14159265358979<a class="error">___</a>
    <td class="error" align="right">3.23e-15

<tr>
    <td>MySQL 5.5
    <td><code>select 3.14159265358979323;</code>
    <td>3.14159265358979323
    <td align="right">0

</table>

<p>

Amazingly, 9 of the 11 tools yield a value different from the original
input, yet none emit so much as a warning. More amazing still is
that these industry-standard tools yield <em>3</em> distinct answers.

<p>

Within those different return values we can see two distinct camps:
Java 1.6, C#, Javascript and sqlite3 all explicitly truncate the value
to either 14 or 15 digits &mdash; one wonders why they can't decide on one
(<a href="http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/float.h.html">&lt;float.h&gt;'s DBL_DIG on the test machine is 15</a>).
C and scripting languages based on it do not truncate, instead using the
nearest approximation that native floats can muster.

<p>

The reader may be familiar with the floating point model and respond:
"Floating point is not designed to be exact, it is designed to compactly
and efficiently handle larger values for scientific purposes than one
could with integers in the same memory", which is absolutely true.
But it's also the case that most tools intended for non-scientific purposes
offer no straight-forward alternative to floats for dealing with fractions and
are thus misused.

<p>

<a name="strings"></a>
<h2>Strings</h2>

Let's test storing strings in less space than they require.

<table class="borderrows" style="width:100%">
<tr>
    <th width="40%">Tool
    <th width="50%">In
    <th>Out
<tr>

    <td>C's
<a href="http://pubs.opengroup.org/onlinepubs/009695399/functions/strcpy.html">strcpy()</a>
won't truncate your data &mdash; it'll happily overwrite other data
<a href="http://www.phrack.com/issues.html?issue=49&id=14">including the stack, the program's CPU instructions, etc(!)</a>

    <td>
<code>
char buf[2];<br>
strcpy(buf, "hello world!");
</code>
    <td class="error">"hello world!\0"

<tr>
    <td style="font-size:smaller">
OpenBSD's C language
<a href="http://en.wikibooks.org/wiki/C_Programming/C_Reference/nonstandard/strlcpy">strlcpy()</a>,
on the other hand, was designed to silently truncate your data in the most
efficient manner possible.
It's designed as a more-secure drop-in replacement for the insecure
<a href="http://pubs.opengroup.org/onlinepubs/009695399/functions/strcpy.html">strcpy()</a>
and the bafflingly-flawed
<a href="http://pubs.opengroup.org/onlinepubs/009695399/functions/strncpy.html">strncpy()</a>.
<p>
It works so well that many operating systems have adopted it including
<a href="http://lxr.linux.no/linux+v3.3.2/lib/string.c#L146">the Linux kernel</a>,
though userland Linux applications usually supply their own because
<a href="http://lwn.net/Articles/33812/">glibc refuses to include it</a>.
    <td>
<code>
char buf[2];<br>
strlcpy(buf, "hello world!", sizeof buf);
</code>
    <td class="error">"h\0"

<tr>
    <td><a href="http://www.postgresql.org/">Postgresql 9.1</a>
will not store truncated data.
    <td>
<code>
create table foo (bar char(1));<br>
insert into foo values ("hello world!");
</code>
    <td>ERROR:  value too long for type character(1)
<tr>
    <td>
<a href="http://www.sqlite.org/">sqlite3</a> seemingly
avoids the problem by trading the length-limited SQL types
<code>char</code> and <code>varchar</code> with a
<a href="http://www.sqlite.org/limits.html">TEXT type &mdash; <em>but it does</em>
have an implicit limit [1..2<sup>31</sup>] that default to 1e9 bytes</a>...
what happens when we exceed that limit?
<p>
It does the right thing.
    <td>
<code style="font-size:small">
(echo 'create table foo (bar text); \<br>
&nbsp;echo -n 'insert into foo values("';\<br>
&nbsp;(perl -e 'print "a"x1000000001');\<br>
&nbsp;echo '");' ) | sqlite3
</code>
    <td>Error: near line 1: string or blob too big

<tr>
    <td>
<a href="http://dev.mysql.com/doc/refman/5.5/en/introduction.html">MySQL 5.5</a> will gladly toss your data away.
<p>
...we do get a <a href="http://dev.mysql.com/doc/refman/5.0/en/show-warnings.html">warning</a>
&mdash; not that I've ever seen a
<a href="http://stackoverflow.com/questions/47589/can-i-detect-and-handle-mysql-warnings-with-php">typical MySQL application that checked them</a>,
but at least it's within the realm of theoretical possibility.

    <td>
<code>
create table foo (bar char(1));<br>
insert into foo values ("hello world!");</code>
    <td class="error">"h"

<tr>
    <td><a href="http://msdn.microsoft.com/en-us/library/bb418433(v=sql.10).aspx">MS SQL 2012</a>
will automatically truncate your data based on the type, but you have to
<a href="http://msdn.microsoft.com/en-us/library/ms190368.aspx">ask for it for varchars</a>.
It'll gladly truncate <code>binary</code> and <code>varbinary</code> data without warning.
    <td>
<code>
SET ANSI_WARNINGS OFF;
</code>
</table>

</blockquote>

<p>

What programmers <i>think</i> they're saying when they set up an operation to
silently truncate is "255 characters should be enough for this text blurb"
&mdash; but most of the time what they're <i>really</i> saying is "I don't
know how long these text blurbs will be, but I don't want an enormous one to
waste a bunch of space, therefore we'll chop them off at 255."

<p>

But that's a cop-out &mdash; it's true that perhaps the strings should be length-limited, but the
decision should not fall to the database engine but to the program.
Indeed, the program may start off just as stupidly as the database by doing <code>substr(Blurb, 0, 255)</code>,
but over time the application can adapt while the database <code>varchar</code> never can.

<p>

Lacking explicit domain-specific information, <em>the only possible response when asked to store
information that cannot be accurately stored and retrieved in full must be an error</em>.
People behind quality tools recognize this and design them so that nothing else is possible.
<p>
<h2>See Also</h2>
<ul>
    <li><a href="http://tech.slashdot.org/story/14/07/22/1314231/mit-combines-carbon-foam-and-graphite-flakes-for-efficient-solar-steam-generation">MIT' Combines Carbon Foam and Graphite Flakes For Efficient Solar Steam Generati</a>
</ul>
