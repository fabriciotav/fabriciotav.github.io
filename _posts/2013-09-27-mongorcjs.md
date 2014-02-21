---
layout: post
title: .mongorc.js
date: 2013-09-27 00:00:00
category: snippets
---

# {{ page.title }}

Para não ter que ficar sempre digitando `db.collections.find().pretty()` no mongo shell, basta criar o arquivo `$HOME/.mongorc.js` e adicionar:

<pre><code class="javascript">DBQuery.prototype._prettyShell = true</code></pre>
