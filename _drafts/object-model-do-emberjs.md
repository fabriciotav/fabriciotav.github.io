---
layout: post
title: Ember.js Object Model
date: 2013-02-21 00:00:00
category: blog
---

# {{ page.title }}

---

É fundamental conhecer bem o Object Model do Ember.js para poder entender toda 
a "mágica" por trás do framework. Se você lê inglês, recomendo checar os guias 
no [site][guides] do Ember.js, especialmente sobre o [Object Model][guides-object-model].
Caso ainda não conheça o framework, comece por [aqui][ember-introducao].

## Classes e Instâncias

---

Para definir uma nova classe, usa-se o método `extend` do `Ember.Object`.

<pre><code class="javascript">App.Mogwai = Ember.Object.extend({
    eatAt: function(time) {
        console.log(time);
    },

    gremlin: false
});
</code></pre>

Para instanciar uma classe, usa-se o método `create`. Ao instanciar uma classe, 
é possível passar atributos, mas não novos métodos.

<pre><code class="javascript">App.mogwai = App.Mogwai.create({
    name: 'Gizmo'
});
</code></pre>

É possível, entretanto, definir uma subclasse, a partir de outra. Para 
sobrescrever métodos existentes, mas ainda acessar a implementação da 
classe pai, chame o método `_super`.

<pre><code class="javascript">App.Gremlin = Ember.Mogwai.extend({
    eatAt: function(time) {
        this._super();
    }
});
</code></pre>

Ou:

<pre><code class="javascript">App.GremlinView = Ember.View.extend({
  tagName: 'span' // o padrão é `div`
});
</code></pre>

### Inicialização instâncias

Quando uma classe é instanciada, o método `init` é chamado automaticamente. É 
o melhor lugar para fazer qualquer tipo de configuração necessário.

<pre><code class="javascript">
Person = Ember.Object.extend({
  init: function() {
    var name = this.get('name');
    alert(name + ", reporting for duty!");
  }
});

Person.create({
  name: "Stefan Penner"
});

// alerts "Stefan Penner, reporting for duty!"
</code></pre>

Para acessar atributos de uma instância, utilize os métodos `get` e `set`, caso 
contrário, observers, computed properties e templates não vão funcionar como
esperado.

<pre><code class="javascript">
var person = App.Person.create();

var name = person.get('name');
person.set('name', "Tobias Fünke");
</code></pre>


## Observers

---

Ember supports observing any property, including computed properties. You can set up an observer on an object by using the addObserver method.

<pre><code class="javascript">
Person = Ember.Object.extend({
  // these will be supplied by `create`
  firstName: null,
  lastName: null,

  fullName: function() {
    var firstName = this.get('firstName');
    var lastName = this.get('lastName');

    return firstName + ' ' + lastName;
  }.property('firstName', 'lastName')
});

var person = Person.create({
  firstName: "Yehuda",
  lastName: "Katz"
});

person.addObserver('fullName', function() {
  // deal with the change
});

person.set('firstName', "Brohuda"); // observer will fire
</code></pre>

Because the fullName computed property depends on firstName, updating firstName will fire observers on fullName as well.

Because observers are so common, Ember provides a way to define observers inline in class definitions.

<pre><code class="javascript">
Person.reopen({
  fullNameChanged: function() {
    // this is an inline version of .addObserver
  }.observes('fullName')
});
</code></pre>

You can define inline observers by using the Ember.observer method if you are using Ember without prototype extensions:

<pre><code class="javascript">
Person.reopen({
  fullNameChanged: Ember.observer(function() {
    // this is an inline version of .addObserver
  }, 'fullName')
});
</code></pre>

## Computed Properties

---


## Computed Properties e Aggregate Data com @each

---

## Bindings

---

## Reopening Classes e Instâncias

---

## Quando usar o quê

---



[guides]: http://emberjs.com/guides/ "Ember.js Guides"
[guides-object-model]: http://emberjs.com/guides/object-model/classes-and-instances/ "Ember.js - The Object Model"
[ember-introducao]: http://fabriciotav.org/blog/2013/02/20/conceitos-basicos-do-emberjs.html "Conceitos básicos do Ember.js"