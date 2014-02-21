---
layout: post
title: Handlebars Helpers com Ember.js
date: 2013-02-20 00:00:00
category: blog
---

# {{ page.title }}

---

[Handlebars][handlebars] é um ótimo *template engine* JavaScript construído 
sobre o [Mustache][mustache]. Se ainda não conhece, dá uma olhada no 
[site][handlebars], dá pra ter uma noção bem bacana do que ele é capaz.

## Criando um Helper
---

Para criar um novo handlebar helper para, por exemplo, um botão de 
submit form, usa-se o seguinte:

<pre><code class="javascript">Handlebars.registerHelper('submitButton', function(text) {
    return new Handlebars.SafeString(
        '&lt;button type="submit"&gt;' + text + '&lt;/button&gt;'
    );
});
</code></pre>

Entretanto, às vezes é interessante usar um helper para algum tipo de 
formatação, como, por exemplo, centavos para real:

<pre><code class="javascript">Handlebars.registerHelper('real', function(centavos) {
    var real = Math.floor(centavos / 100);

    return real;
});
</code></pre>

Template:
<pre class="html"><code>{% raw %}&lt;div&gt;
    R$ {{real totalCost}}
&lt;/div&gt;
{% endraw %}</code></pre>

Saída HTML:
<pre class="html"><code>&lt;div&gt;
    R$ 150,43
&lt;/div&gt;
</code></pre>


Se o valor da variável `totalCost` for alterado, o helper só imprimirá o novo 
valor se a página for recarregada. O que não é bem o que queremos quando 
estamos usando o Ember.js.

## Criando um Bound Helper

---

Em situações onde queremos que o helper seja chamado toda vez que o valor da 
variável for modificado, utilizamos o `Ember.Handlebars.registerBoundHelper`.

<pre><code class="javascript">Ember.Handlebars.registerBoundHelper('real', function(centavos) {
    var real = Math.floor(centavos / 100);

    return real;
});
</code></pre>

Dessa forma, sempre que o valor da variável alterar, o helper será chamado.

[handlebars]: http://handlebarsjs.com/ "Handlebars.js website"
[mustache]: http://mustache.github.com/ "Mustache website"