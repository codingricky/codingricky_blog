+++
author = "Ricky Yim"
categories = ["Play!", "Play", "sql", "debugging"]
date = 2013-09-23T02:00:00Z
description = ""
draft = false
slug = "printing-out-sql-statements-in-play-part-2"
tags = ["Play!", "Play", "sql", "debugging"]
title = "Printing out SQL statements in Play (part 2)"
aliases = [
    "/printing-out-sql-statements-in-play-part-2/"
]
+++

This is a follow up to my original post on <a href="http://codingricky.ghost.io/printing-out-sql-statements-in-play/">printing sql statements in Play</a>. The method I described earlier no longer works with Play 2.2.X. 

I've found a simpler method that currently works with Play 2.1.X and Play 2.2.X.

Just go and edit your <b>application.conf</b> file and add the following lines.

<pre class="prettyprint">
db.default.logStatements=true
logger.com.jolbox=DEBUG
</pre>

Restart your application and you should see something like the following in your logs.

<pre class="prettyprint">
[debug] c.j.b.PreparedStatementHandle - select t0.uuid as c0, t0.email as c1, t0.creation_time as c2, t0.expiration_time as c3, t0.is_sign_up as c4 from persistable_token t0
</pre>
