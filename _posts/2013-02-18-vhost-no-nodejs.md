---
layout: post
title: vhost no node.js utilizando express
date: 2013-02-18 00:00:00
category: blog
---

# {{ page.title }}

Sempre utilizo virtual hosts quando trabalho com Apache, o que permite, dentre
várias coisas, ter vários domínios apontando para a mesma porta de um ip.

Como, nos últimos meses, praticamente abandonei o Apache em favor do node.js
como servidor, precisei achar uma alternativa.

Utilizando o framework [Express][expressjs], o processo é muito simples.

<pre><code class="javascript">var express;
express = require('express');

// Primeiro aplicativo
appOne = express();

// Segundo aplicativo
appTwo = express();

// Terceiro aplicativo
api = express();

// Aplicativo principal
app = express();

// Os parâmetros do express.vhost são o domínio e o aplicativo
app.use(express.vhost('app-one.com', appOne));
app.use(express.vhost('app-two.com', appTwo));
app.use(express.vhost('api.dominio.com', api));

app.listen(80);
</code></pre>

E voilà!

O roteamento acontece da forma como você provavelmente está acostumado:

<pre><code class="javascript">appOne.get('/', function(req, res) {
    res.send('Tudo funcionando!')
});
</code></pre>

Qualquer dúvida, coloca aí nos comentários.

[expressjs]: http://expressjs.com/ "Express.js"