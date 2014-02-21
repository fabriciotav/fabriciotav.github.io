---
layout: post
title: Convenções de Nomenclatura no Ember.js
date: 2013-02-21 00:00:00
category: draft
---

# {{ page.title }}

Muito do que o Ember automaticamente faz por você depende dos nomes utilizados
para classes, objetos e *properties*. Portanto, é fundamental conhecer a forma
que o Ember espera encontrar tais nomes. Vamos lá.

## Classes

Todas as classes devem ser [*CamelCase*][camel-case].

*Routes*, *views* e *controllers* devem ter essa especificação após o nome. Por 
exemplo, um *resource* (existem *Routes* do tipo *resource* e do tipo *route*) 
com nome `people`, deve gerar:

<pre class="prettyprint linenums">
App.PeopleRoute = Ember.Route.extend({});
App.PeopleController = Ember.ObjectController.extend({});
// ou 
// App.PeopleController = Ember.ArrayController.extend({});
App.PeopleView = Ember.View.extend({});
</pre>

Um model com nome *people* deve ser da seguinte forma:

<pre class="prettyprint linenums">
App.People = Ember.Object.extend({});
// ou, no caso do Ember Data
App.People = DS.Model.extend({});
</pre>

Mixins possuem nomes como os models.

## Instâncias

O Ember cria automaticamente instâncias das classes na inicialização da
aplicação. Então não há necessidade de fazer algo como:

<pre class="prettyprint linenums">
App.PeopleView = Ember.View.create({});
</pre>

Nos casos em que seja necessário, os nomes devem ser [*camelBack*][camel-back].
Como no exemplo abaixo.

<pre class="prettyprint linenums">
App.peopleView = Ember.View.create({});
</pre>

## Templates

Templates de resources, devem ter como nome o nome do resource. No caso de um
resource com nome `people`:

<pre class="prettyprint linenums">
&lt;script type="text/x-handlebars" data-template-name="people"&gt;
&lt;/script&gt;
</pre>

Routes do tipo `route` devem vir após o nome do resource "pai", da seguinte
forma:

<pre class="prettyprint linenums">
// No caso do route padrão
&lt;script type="text/x-handlebars" data-template-name="people/index"&gt;
&lt;/script&gt;

// No caso de um route com nome `new`
&lt;script type="text/x-handlebars" data-template-name="people/new"&gt;
&lt;/script&gt;
</pre>

## Properties

Todas properties devem ser camelBack.






[camel-case]: http://en.wikipedia.org/wiki/CamelCase
[camel-back]: http://en.wikipedia.org/wiki/CamelBack