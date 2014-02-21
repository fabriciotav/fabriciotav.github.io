---
layout: post
title: Iniciar um repo git
date: 2012-07-15 00:00:00
category: snippets
---

# {{ page.title }}

Para iniciar um repositório git, com o propósito de servir como um repositório compartilhado:

<pre><code class="bash">mkdir repositorio.git
cd repositorio.git/
git --bare init
</code></pre>

Agora, no seu repositório local:

    git remote add origin /path/repositorio.git
