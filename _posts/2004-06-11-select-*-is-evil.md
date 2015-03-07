---
layout: post
title: "Select * Is Evil"
date: 2004-06-11
summary: You may be in the habit of using <code>SELECT *</code> in your SQL queries. There are several very good reasons to break this habit.
---

<p>
You may be in the habit of using <code>SELECT *</code> in your SQL queries. There are several very good reasons not to.
</p>

<h2>Self-Documentation Lost</h2>
If you've ever had the task of diving into someone else's code in the attempt to make bugfixes, changes or even just understand it then you may have run across SQL queries such as:

<blockquote>
<pre>
<code>
SELECT *
FROM products
WHERE prod_id = <i>n</i>;
</code>
</pre>
</blockquote>

Very nice, legal SQL, sure. But what are we getting back? 1 field? 10? 100? What are the field names and in which order do they appear? Yes, you may have the luck of having a whole db layout right in front of you, but maybe not. Maybe there is some internal documentation, but maybe it's from last August when the db was built and maybe it's changed since then. Maybe there is no documentation except the database itself. Bad practice? Sure, but welcome to the real world.Either you're going to have to dig through the docs or you're going to have to connect to the db and figure out what the table looks like. However, even though you now have the table layout in front of you, <b>you still don't know what assumptions the original coder was making</b> nor <b>which data he intends to actually use until he uses it!</b>.
<p>
Maybe there are 8 fields returned and the one of them is misspelled. If the field name is in the query then you can visually match it up; it's right in front of you. Otherwise you consult the docs. Is it _id or id? Maybe the docs say id and the field name in the table is _id.
<p>
Consider:

<blockquote>
<code>
<pre>
SELECT id
      ,desc
      ,img_url
      ,category
      ,title
      ,serial
      ,color
      ,mystery_field
FROM products
WHERE prod_id = <i>n</i>;
</pre>
</code>
</blockquote>

It takes up more vertical space, sure. But the few extra lines are more than worth it. The query is nicely formatted and completely <a href="http://dictionary.reference.com/search?q=unambiguous">unambiguous</a>. I know exactly what I'm looking for and what I'm getting back if the query succeeds. The query goes from a being black hole to being what all good code can be: <b>self-documenting</b>
<p>
<h2>Broken Contract</h2>
<p>
Tables change. Hopefully these changes are planned out and for a good reason. Sometimes they're not. <i>login</i> changes to <i>username</i> to avoid ambiguity. Your manager orders you to change a bunch of fields because he doesn't like leading underscores. <i>_key</i> changes to anything else because whomever thought _key was a good name for a field in the first place was fired 6 months ago.
<p>
When a column name changes, obviously anything that depended on the old name has to change as well. You may ask &quot;What's wrong with this?&quot;

<blockquote>
<pre><code>SELECT * FROM products;</code></pre>
</blockquote>

Lots, but some of the problems might not be obvious. The problem will be afterwards when your code tries to do something with the <i>login</i> field which is now the <i>username</i> field.  The query will run absolutely fine, <b>even though the coder's assumption is now <u>wrong</u></b>.  There is no <i>login</i> field anymore. <b>The database doesn't know anything is wrong because you're just saying &quot;give me everything you've got&quot; and it will comply</b>.
<p>
What we want to do is have <b>a contract with the database</b>. We want to tell it <b>exactly what we want</b> and it will tell us whether or not it's there. If we issue the query
<blockquote><pre><code>SELECT nonexistent_field
FROM products;</code></pre>
</blockquote>

our database will tell us right away that it's never heard of this field. For example:

<blockquote class="error">
ERROR 1054 (42S22): Unknown column 'nonexistant_field' in 'field list'
</blockquote>

or

<blockquote class="error">
ERROR:  column "nonexistant_field" does not exist
</blockquote>
<p>
We can fix our code and be confident that <b>as long as the query is syntactically valid</b> and <b>as long as the the code accessing the resultset matches the fields in the query</b> then we won't have any problems accessing the data we need. With SELECT * we lose that contract and even though the queries are completing without error <b>we can no longer be confident about our assumptions</b>.
<p>
<h2>Size Matters</h2>
<p>
Why ask for everything when you'd be happy with less? Why send out a request for 10 fields, have the database do that extract work retrieving them, sending them back over the wire and processed and then not use them?
<p>
The real problem comes when you SELECT * from a table that is storing more than integers and small pieces of character data. Say you have a user table which contains a <i>username</i>, <i>pass</i> and <i>about_me</i> fields.  Maybe when you wrote the app only the first 2 fields existed, so you've been logging users into your webapp like so 

<blockquote>
<pre><code>SELECT *
FROM users
WHERE username = <i>u</i>
AND pass = PASSWORD(<i>p</i>);</code></pre>
</blockquote>

As long as you get 1 row back you can be sure the login is valid. Later on a VARCHAR or TEXT field was entered so your users could store a blurb about themselves. Suddenly instead of 2 small text fields, you're pulling hundreds or thousands of bytes you don't need, and that's just for one row.
<p>
Now multiply the above situation by <i>n</i> rows. Say you're listing users on your site, 100 at a time. If you pull out that <i>about_me</i> field needlessly and the average length is say, 250 bytes (about the length of this sentence), then you're wasting 25k worth of memory on 2 machines as well as the bandwidth to move it and the extra processing time it takes to handle the data <i>per page view</i>. Say you get a modest 1 page view per second. That's 1.5MB (25k * 60 kb/s) <i>per minute</i>'s worth of data filling your RAM and choking your NIC that is a complete and utter waste. This is on top of all the data you're actually using.
<p>
For tables of 1 million records, every byte you make the database process needlessly is a waste of nearly 1MB's worth of RAM, bandwidth and processing time on both the server and client machine. Even if you just pull one unneeded 10 byte field you're flushing almost 10MB down the toilet, right off the bat. You might think &quot;Yeah, but I'll never have 1 million records!&quot; That's a) probably not true and b) the wrong attitude. Make it a habit of writing code the proper, efficient, self-documenting way no matter what the scale.
<p>
<h2>Out Of Order</h2>
<p>
Get in the habit of specifying every field, every time. Even though your language might not care what order the fields are in, there are those APIs that do. And even if your language doesn't, there are still assumptions that can be made in code that will break if an existing column is ever reordered or deleted or a new column is ever created or an deleted.
<p>
<h4>When You Assume...</h4>
<p>
Any time you fetch an array from a resultset, that is, you depend on the columns to be in a certain order... if the columns ever change your code will break and it will be very difficult to track down the problem. For example:

<blockquote>
<code>
<pre>
sql := "SELECT * FROM products WHERE id = 5"
rs := db.query(sql)
if db.error
    error(db.errstr)
    exit
end if
# else, success
id, name = rs.nextrow()
print(id . ":" . name . "\n")
rs.free
</pre>
</code>
</blockquote>

This example assumes that the first 2 fields in the <i>products</i> table are <i>id</i> and <i>name</i>. Fine. Now, fast forward 6 months. People move on to different projects. Now imagine an upgrade: products are now associated with a category to better organize things:

<blockquote>
<code>
<pre>
ALTER TABLE products
ADD FOREIGN KEY cat_id
  REFERENCES categories(id)
  DEFAULT NULL
AFTER id;
</pre>
</code>
</blockquote>
<p>
The above code is now broken, because of its assumption of the <i>name</i> column's position. (When this is discovered you'll hear &quot;But it worked before!&quot;) This is trivially remedied by enumerating the columns in the query.
<p>
<h2>Don't Do It</h2>
<p>
In conclusion, there are many reasons not to use <code>SELECT *</code>. Even though SQL is written for a machine it must be clear and
concise to other people. So take the extra 20 seconds and enumerate all the fields you really need in your
SELECT clauses and your database, application and users will thank you for it; and you might just find that
a month from now you have some idea of what your own query is really doing :)
