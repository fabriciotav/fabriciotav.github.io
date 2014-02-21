---
layout: post
title: Precompilando templates Handlebars para aplicações Ember.js
date: 2013-05-31 00:00:00
category: blog
---

# {{ page.title }}

---

O [Ember.js][emberjs] utiliza, como padrão, o [Handlebars][handlebarsjs] para 
especificações de templates JavaScript. É possível utilizar outras formas de 
templating client-side, mas perde-se uma infinidade de vantagens já existentes, 
como helpers e bindings.

Quando o tamanho da aplicação não é maior do que alguns routes (diria algo em 
torno de 5), podemos facilmente lidar com todos os templates da seguinte forma:

<pre><code class="html">{% raw %}&lt;script type="text/x-handlebars" data-template-name="application"&gt;
Esse é o template padrão.

{{ outlet }}
&lt;/script&gt;
{% endraw %}</code></pre>

Ao integrar mais funcionalidades no Web App, consequentemente aumentando o número
de templates, esse modelo fica estremamente difícil de manter. Não é difícil 
chegar a mais de 100 templates em um Web App comercial.

Quando isso acontece, precisa-se de uma forma de manter a ordem, e uma saída 
interessante (diria obrigatória) é a precompilação dos templates que serão 
utilizados.
Dessa forma consegue-se separar cada template em um arquivo (criando uma estrutura
mais fácil de entender e manter), e aumenta-se a performance da aplicação, já 
que, quando requisitado, o template já estará pré-compilado.

Além disso, só é necessário colocar o runtime do Handlebars como dependência, que
na versão 1.0.0-rc.4 tem apenas 10kb (60kb a menos do que a versão com o compilador 
embutido).

Ok, mas como fazer?

Se você já está familiarizado com o [Node.js][nodejs], basta instalar o pacote
[em-hbs-precompiler][em-hbs-precompiler].

<pre>
  npm install -g em-hbs-precompiler
</pre>

A utilização é extremamente simples. Informa-se o diretório onde estão os 
templates, e o nome do arquivo que conterá os templates compilados.

<pre>
  precompilehbs -s diretorio/com/os/templates/ -o compilados.js
</pre>

Pronto. Agora todos seus templates estarão pré-compilados e com nome correto.

Caso haja sub diretórios no seu diretório de templates, a nomeação será como padrão do 
Ember.js: <strong>resource</strong> ou, em caso de templates em subdiretórios,
<strong>resource/route</strong>.

Dúvidas ou sugestões, coloque aí nos comentários!

[em-hbs-precompiler]: https://npmjs.org/package/em-hbs-precompiler "Ember.js Handlebars precompiler"
[emberjs]: http://emberjs.com/ "Ember.js"
[handlebarsjs]: http://handlebarsjs.com/ "Handlebars.js"
[nodejs]: http://nodejs.org/ "Node.js"
