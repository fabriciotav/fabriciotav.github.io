---
layout: post
title: Salvar Screenshots no Mac OS X
date: 2012-07-07 00:00:00
category: snippets
---

# {{ page.title }}

Para alterar o local padrão de salvamento das screenshots tiradas no Mac, basta abrir o Terminal e digitar isso:

<pre><code class="bash">
defaults write com.apple.screencapture location /path/
</code></pre>

Onde `/path/` é o caminho da pasta que você escolher. Após essas mudanças é preciso digitar:

<pre><code class="bash">
killall SystemUIServer
</code></pre>