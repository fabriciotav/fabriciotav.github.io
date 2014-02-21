---
layout: post
title: Conceitos básicos do Ember.js
date: 2013-02-20 00:00:00
category: blog
---

# {{ page.title }}

---

<img src="/img/posts/emberjs/outdated.png" alt="Post desatualizado">

**Atenção!** Esse post está desatualizado, e utiliza a versão v.1.0.0-rc.1.

---

**Obs. 1:** Esse post assume um conhecimento básico de JavaScript.

**Obs. 2:** Decidi não traduzir vários termos em inglês para o português por serem
conceitos fundamentais ao Ember. Quando possível, utilizo de termos em português.
Entretanto, não se preocupe, pois todos os conceitos estão explicados. Qualquer dúvida
quanto a isso, coloque nos comentários.

**Obs. 3:** Este não pretende ser um guia completo, apenas uma introdução. 
Trabalharei conceitos mais avançados em futuros posts.

## O que é o Ember.js?

---

Do próprio [site][ember]:

> A framework for creating ambitious web applications.

Em uma tradução livre: Um framework para criação de grandes aplicações web.

Vou falar sobre os conceitos básicos necessários para a criação de um micro
aplicativo (lista de pessoas e seus frameworks favoritos), que será construído ao longo do texto. A versão final está [aqui][blocks-4980834-raw].

## Por quê utilizar o Ember?

---

Se você já teve a experiência de criar uma página web, com várias requisições
ajax e modificações da interface de usuário, sabe que, em pouco tempo, o código
vira um espaguete. Extremamente complicado de manter, principalmente quando
há mais de uma pessoa trabalhando no projeto.

Portanto, se há a necessidade de ações CRUD - *create* (criar), *read* (ler), *update*
(alterar) e *delete* (remover) - na página, e você deseja melhorar a performance,
evitando recarregamento a cada ação, o padrão de arquitetura 
<abbr title="Model View Controller">MVC</abbr>, adotado pelo Ember, vai 
facilitar sua vida.

É claro que é possível ter código bem estruturado sem a utilização de um
framework. Entretanto, quando a sua base de código começar a crescer e crescer,
e quando começar projetos novos, vai perceber que estará escrevendo as
mesmas funções para cada uma das aplicações.

> "Trivial choices are the enemy" - [Yehuda Katz][twitter-wycats], Ember Core Team

Ember se apóia bastante no paradigma *convention over configuration*.
Quase sempre o que é necessário para a aplicação será gerado automaticamente, em
memória, sem precisar, explicitamente, instanciar classe alguma.

Isso não significa que não seja possível configurá-lo para ficar exatamente da
forma como te agrada. Apenas elimina as decisões triviais que toda aplicação
requer ao longo do seu desenvolvimento.

## Padrão de arquitetura <abbr title="Model View Controller">MVC</abbr>

---

MVC significa Model View Controller. Não vou entrar em detalhes sobre a história
desse padrão, nem sua evolução ao longo dos anos. Falarei apenas dos conceitos
necessários para entender e construir uma aplicação com Ember.

MVC no Ember é como MVC em aplicações desktop. Portanto, se você já utiliza 
algum framework MVC no servidor, como Django, Zend ou Ruby on Rails, deixe de 
lado por um momento os conceitos que você já sabe.

## Application

---

Para criar uma aplicação, primeiramente é necessário instanciar a classe 
`Ember.Application`.

<pre><code class="javascript">window.App = Ember.Application.create();
</code></pre>

`App` será o *namespace* do aplicativo. Aqui usei a palavra App, mas pode ser
qualquer outra. A convenção é que o nome comece com letra maiúscula.

Todas as classes serão instanciadas sob esse namespace.

## Templates

---

Toda a interface de usuário residirá nos templates. O Ember utiliza, por padrão,
o [Handlebars][handlebars]. É possível utilizar outro template, mas, a não ser
que você saiba bem o que está fazendo, não recomendo. Ganhamos várias coisas
ao utilizar o Handlebars, sem precisar preocupar com coisas triviais.

O template default do aplicativo é chamado `application`, e é criado da
seguinte forma:

<pre><code class="html">{% raw %}&lt;script type="text/x-handlebars" data-template-name="application"&gt;
Esse é o template padrão.

{{ outlet }}
&lt;/script&gt;
{% endraw %}</code></pre>

Esse é o [menor aplicativo possível][blocks-4988275], criado com Ember. Muita 
coisa está acontecendo nos "bastidores". Vejamos:

1. o aplicativo é inicializado;
2. o template `application` é renderizado;
3. automaticamente é criado um route, que reflete o estado do
aplicativo na URL.

O `{{ '{ outlet '|prepend:'{' }}}}` serve como um espaço reservado para outros 
templates.

Ao estender o controller abaixo, tornamos disponível alguns dados para o
template `application`.

<pre><code class="javascript">App.ApplicationController = Ember.ObjectController.extend({
    nome: 'Eddard',
    sobrenome: 'Stark'
});
</code></pre>

<pre><code class="html">&lt;script type="text/x-handlebars" data-template-name="application"&gt;
Olá, {{ '{ nome '|prepend:'{' }}}} {{ '{ sobrenome '|prepend:'{' }}}}!

{{ '{ outlet '|prepend:'{' }}}}
&lt;/script&gt;
</code></pre>

O resultado HTML será:

<pre><code class="html">&lt;div id="ember151" class="ember-view"&gt;
    Olá, Eddard Stark!
&lt;/div&gt;
</code></pre>

Não se preocupe com os atributos HTML `id` e `class` por enquanto.

## Router

---

O router é responsável pela manutenção do [estado][state-machine]
do aplicativo. Como estamos construindo um aplicativo web, que roda em um
navegador, seria ótimo se o estado ativo fosse refletido na URL.

Bem, o Ember faz isso automaticamente.

Vamos criar uma nova rota para listar as pessoas do nosso app.

<pre><code class="javascript">App.Router.map(function(){
    this.resource('people');
});
</code></pre>

<pre><code class="html">&lt;script type="text/x-handlebars" data-template-name="people"&gt;
&lt;h1&gt;Pessoas&lt;/h1&gt;
&lt;/script&gt;
</code></pre>

Isso criará automaticamente a seguinte URL:
    
<pre><code class="http"><span class="request"><span class="string">http://dominio.com/#/people</span></span></code></pre>
    

Ao ser acessada, o template `people` será renderizado e adicionado no outlet 
do template `application`.

Além da manutenção dos estados, o router é também responsável por atribuir
aos controllers a representação de um ou mais models, e lidar com eventos.

### Resources e routes

Há duas formas de se criar um novo route:

<pre><code class="javascript">App.Router.map(function(){
    // Cria um route e um novo namespace
    this.resource('people');

    // Cria um route no namespace que o route está aninhado
    this.route('new');
});
</code></pre>

Ao criar um novo *resource*, será criado também um novo namespace; quando
é criado um *route*, o namespace é o do route "pai". Isso vai afetar a forma
como se declara os nomes dos controllers, views e templates. Ao criar os routes 
como no código acima, temos o seguinte:

<table class="table">
    <thead>
        <tr>
            <th>URL</th>
            <th>Nome do route</th>
            <th>Controller</th>
            <th>Route</th>
            <th>Template</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>/</code></td>
            <td><code>index</code></td>
            <td><code>App.IndexController</code></td>
            <td><code>App.IndexRoute</code></td>
            <td><code>index</code></td>
        </tr>
        <tr>
            <td><code>/people</code></td>
            <td><code>people</code></td>
            <td><code>App.PeopleController</code></td>
            <td><code>App.PeopleRoute</code></td>
            <td><code>people</code></td>
        </tr>
        <tr>
            <td><code>/new</code></td>
            <td><code>new</code></td>
            <td><code>App.NewController</code></td>
            <td><code>App.NewRoute</code></td>
            <td><code>new</code></td>
        </tr>
    </tbody>
</table>

A medida que a complexidade da aplicação aumenta, será necessário ter routes 
aninhados. Eles podem ser criados da seguinte forma:

<pre><code class="javascript">App.Router.map(function(){
    this.resource('people', function() {
        this.route('new');
    });
});
</code></pre>

E os nomes ficam assim:

<table class="table">
    <thead>
        <tr>
            <th>URL</th>
            <th>Nome do route</th>
            <th>Controller</th>
            <th>Route</th>
            <th>Template</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>/</code></td>
            <td><code>index</code></td>
            <td><code>App.IndexController</code></td>
            <td><code>App.IndexRoute</code></td>
            <td><code>index</code></td>
        </tr>
        <tr>
            <td>N/A</td>
            <td><code>people</code></td>
            <td><code>App.PeopleController</code></td>
            <td><code>App.PeopleRoute</code></td>
            <td><code>people</code></td>
        </tr>
        <tr>
            <td><code>/people</code></td>
            <td><code>people.index</code></td>
            <td><code>App.PeopleIndexController</code></td>
            <td><code>App.PeopleIndexRoute</code></td>
            <td><code>people/index</code></td>
        </tr>
        <tr>
            <td><code>/people/new</code></td>
            <td><code>people.new</code></td>
            <td><code>App.PeopleNewController</code></td>
            <td><code>App.PeopleNewRoute</code></td>
            <td><code>people/new</code></td>
        </tr>
    </tbody>
</table>

Veja que, ao aninhar o route `new` ao resource `people`, automaticamente
foi criado um novo route, o `index`. Todo resource que tiver routes 
aninhados, terá um route index.

### Dynamic segments

Ao acessar um determinado estado da aplicação a partir de uma URL, o route 
utiliza a informação da URL para determinar o model correspondente, e carregar
esse model para o controller.

A versão final do router do nosso aplicativo será:

<pre><code class="javascript">App.Router.map(function(){
    this.resource("people", function() {
        this.resource('person', { path: ':person_id' });
        this.route('new');
    });
});
</code></pre>

Repare que o valor da property `path`, no resource person, possui um `:`, 
seguido do nome do resource junto com um `_id`. Isso faz com que, ao acessar a 
URL `/people/3`, o router automaticamente busque pelo model `App.Person` com id 
3.

Caso haja interesse em ter uma URL com nome diferente do nome do route, como,
por exemplo, o nome "pessoas" para o route `people`, é só adicionar o valor à 
property `path`.

<pre><code class="javascript">App.Router.map(function(){
    this.resource("people", { path: '/pessoas' }function() {
        this.resource('person', { path: ':person_id' });
        this.route('new');
    });
});
</code></pre>

Os nomes ficam assim:

<table class="table">
    <thead>
        <tr>
            <th>URL</th>
            <th>Nome do route</th>
            <th>Controller</th>
            <th>Route</th>
            <th>Template</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td><code>/</code></td>
            <td><code>index</code></td>
            <td><code>App.IndexController</code></td>
            <td><code>App.IndexRoute</code></td>
            <td><code>index</code></td>
        </tr>
        <tr>
            <td>N/A</td>
            <td><code>people</code></td>
            <td><code>App.PeopleController</code></td>
            <td><code>App.PeopleRoute</code></td>
            <td><code>people</code></td>
        </tr>
        <tr>
            <td><code>/people</code></td>
            <td><code>people.index</code></td>
            <td><code>App.PeopleIndexController</code></td>
            <td><code>App.PeopleIndexRoute</code></td>
            <td><code>people/index</code></td>
        </tr>
        <tr>
            <td><code>/people/:id</code></td>
            <td><code>person</code></td>
            <td><code>App.PersonController</code></td>
            <td><code>App.PersonRoute</code></td>
            <td><code>person</code></td>
        </tr>
        <tr>
            <td><code>/people/new</code></td>
            <td><code>people.new</code></td>
            <td><code>App.PeopleNewController</code></td>
            <td><code>App.PeopleNewRoute</code></td>
            <td><code>people/new</code></td>
        </tr>
    </tbody>
</table>

## Models

---

Os models são responsáveis pelo controle dos dados da aplicação. São
completamente independentes da interface do usuário, apesar de serem
requisitados por ela. Ao sofrer atualização, o model notifica os *observers*,
que traduz isso para a <abbr title="User Interface">UI</abbr>.

No Ember, os models são javascript objects ligeiramente modificados (para 
suportar *bindings* e outras funcionalidades).

### Definindo um *Model*

<pre><code class="javascript">App.Person = DS.Model.extend({
    firstName: DS.attr('string'),
    lastName: DS.attr('string'),
    fullName: function() {
        return this.get('firstName') + ' ' + this.get('lastName');
    }.property('firstName', 'lastName')
});
</code></pre>

Estou utilizando o Ember Data, uma biblioteca responsável por carregar
dados do servidor, fazer alterações no navegador, e salvar as alterações de 
volta ao servidor. Esta biblioteca ainda não está mesclada ao Ember, mas o será 
em breve.

As properties `firstName` e `lastName` são carregadas diretamente do servidor 
através de alguma [REST API][fabriciotav-rest-api], e serão persistidas ao 
servidor após as alterações. A property `fullName` é uma *computed property*, 
que tem seu valor determinado a partir de alguma função. No código acima,
ela é composta do nome e sobrenome (firstName e lastName).

Toda vez que qualquer uma das properties do model `App.Person` for alterada, 
a computed property também será.

### Definindo uma *Store*

A *Store* é o repositório que contém todos os models já carregados, além 
de ser responsável pelo carregamento dos models que ainda não foram 
carregados.

<pre><code class="javascript">App.Store = DS.Store.extend({
    revision: 11,
    adapter: DS.RESTAdapter.create();
});
</code></pre>

A property `revision` é o número de revisão da API, utilizada para a
notificação de alterações que podem quebrar código já existente. O `adapter` 
é responsável pela comunicação com o servidor.

O Ember Data possui dois adapters padrões: `DS.FixtureAdapter` e o `DS.RESTAdapter`. 
O primeiro é utilizado para dados *hard coded* (extremamente útil para prototipagem 
de uma aplicação, já que independe do backend), enquanto que o segundo é usado 
quando se tem um servidor REST.

Para esse aplicativo, vamos usar uma REST API criada em node.js, que persiste os 
dados apenas no processo. Essa REST API possui apenas um resource:

<pre><code class="http">
<span class="request">GET <span class="string">http://api.dadospublicos.org/people</span></span>
<span class="request">GET <span class="string">http://api.dadospublicos.org/people/:id</span></span>
<span class="request">POST <span class="string">http://api.dadospublicos.org/people</span></span>
<span class="request">DELETE <span class="string">http://api.dadospublicos.org/people/:id</span></span>
</code></pre>

Para utilizá-la na aplicação, vamos configurar a sua url no adapter.

<pre><code class="javascript">App.RESTSerializer = DS.RESTSerializer.extend({
    init: function() {
        this._super();

        this.map('App.Person', {
            frameworks: {embedded: 'always'}
        });

        this.configure('plurals', {
            person: 'people'
        });
    }
});

App.RESTAdapter = DS.RESTAdapter.extend({
    url: 'http://api.dadospublicos.org',
    bulkCommit: false,
    serializer: App.RESTSerializer.create()
});

App.Store = DS.Store.extend({
    revision: 11,
    adapter: App.RESTAdapter.create();
});
</code></pre>

## Controllers

---

Um controller é responsável por representar um model para um template e por
armazenar properties que não serão salvas no servidor.

Para representar um único model, utiliza-se o `Ember.ObjectController`. Quando
é necessário representar uma matriz (array) de models, utiliza-se o 
`Ember.ArrayController`.

É no route que se determina qual será o model que será representado no 
controller. Para carregar o model `App.Person` no controller `App.PersonController`,
utilizamos a property `model`, como mostrado abaixo.

<pre><code class="javascript">App.PeopleRoute = Ember.Route.extend({
    model: function() {
        return App.Person.find();
    }
});
</code></pre>

### Adicionar pessoa à lista

O controller `App.PeopleNewController` será o responsável por adicionar novas
pessoas à lista. Precisamos, então, configurar seu model. Como o route `people.new`
é para a criação de uma nova pessoa, não há model algum sendo carregado.

<pre><code class="javascript">App.PeopleNewRoute = Ember.Route.extend({
    model: function() {
        // Model vazio
        return null;
    },

    setupController: function(controller) {
        controller.startEditing();
    },

    exit: function() {
        this._super();
        this.controllerFor('people.new').stopEditing();
    }
});
</code></pre>

Toda a manipulação do model é feita no controller.

<pre><code class="javascript">App.PeopleNewController = Ember.ObjectController.extend({
    startEditing: function() {
        this.transaction = this.get('store').transaction();
        this.set('content', this.transaction.createRecord(App.Person, {}));
    },

    stopEditing: function() {
        if (this.transaction) {
            this.transaction.rollback();
            this.transaction = null;
        }
    },

    save: function() {
        console.log( "return", this.validate() );
        if (this.validate()) {
            this.transaction.commit();
            this.transaction = null;
        } else {
            alert('Nome inválido');
        }
    },

    validate: function() {
        var firstName, regex;

        regex = /^[A-ZÄÁÀËÉÈÍÌÖÓÒÚÙÑÇa-zäáàëéèíìöóòúùñç][A-ZÄÁÀËÉÈÍÌÖÓÒÚÙÑÇa-zäáàëéèíìöóòúùñç ]{1,70}[A-ZÄÁÀËÉÈÍÌÖÓÒÚÙÑÇa-zäáàëéèíìöóòúùñç]$/;
        firstName = this.get('content').get('firstName');
        firstNameOk = regex.exec(firstName);

        if (firstNameOk) {
            return true;
        } else {
            return false;
        }
    },

    transitionAfterSave: function() {
        if (this.get('content.id')) {
            this.transitionToRoute('people.index');
        }
    }.observes('content.id'),

    cancel: function() {
        this.stopEditing();
        this.transitionToRoute('people.index');
    },

    addFramework: function() {
        this.get('content.frameworks').createRecord();
    },

    removeFramework: function(framework) {
        framework.deleteRecord();
    }
});
</code></pre>

E no template temos:

<pre><code class="html">{% raw %}
&lt;script type="text/x-handlebars" data-template-name="people/new"&gt;
&lt;form {{action save on="submit"}}&gt;

&lt;div&gt;
    {{view Ember.TextField placeholder="nome" valueBinding="firstName"}}
    {{nameValidation firstName}}
&lt;/div&gt;
&lt;div&gt;
    {{ view Ember.TextField placeholder="sobrenome" valueBinding="lastName"}}
&lt;/div&gt;
&lt;div&gt;
    {{ view Ember.TextField placeholder="twitter" valueBinding="twitter"}}
&lt;/div&gt;

{{#each frameworks}}
    &lt;div&gt;
        {{view Ember.TextField placeholder="framework" valueBinding="name"}}
        &lt;button {{action removeFramework this}}&gt;-&lt;/button&gt;
    &lt;/div&gt;
{{/each}}

&lt;div&gt;&lt;button {{action addFramework}}&gt;Adicionar framework&lt;/button&gt;&lt;/div&gt;
&lt;div&gt;
    {{submitButton "Salvar"}}
    &lt;button {{action "cancel"}}&gt;Cancelar&lt;/button&gt;
&lt;/div&gt;
&lt;/form&gt;
&lt;/script&gt;
{% endraw %}
</code></pre>


## Views

---

A responsabilidade da view é traduzir os eventos primitivos do navegador,
e.g. click, key press, em eventos que tenham algum significado para a sua 
aplicação.

As views são utilizadas quando há necessidade de componentes reutilizáveis,
como inputs, e.g. `Ember.TextField`, ou quando há eventos complexos.

No nosso aplicativo, não precisamos alterar comportamento de view
alguma. Apesar disso, as seguintes views foram geradas automaticamente:

<pre><code class="javascript">App.ApplicationView = Ember.View.extend();
App.PeopleView = Ember.View.extend();
App.PeopleIndexView = Ember.View.extend();
App.PeopleNewView = Ember.View.extend();
App.PersonView = Ember.View.extend();
</code></pre>

## Juntando tudo

---

Todo o código pode ser baixado [aqui][download] e o app pode ser visto 
[aqui][blocks-4980834-raw].

## Coisas importantes que não foram vistas aqui

---

Existem várias áreas extremamente importantes para o desenvolvimento de um 
aplicativo web, como:

1. depuração de erros;
2. testes;
3. organização de arquivos;
4. implementação em servidores.

O objetivo desse post é somente fazer com que você coloque a mão na massa e 
conheça esse excelente framework javascript. Em futuros posts cobrirei outros 
tópicos.

Dicas:

1. Utilize a versão não "minificada" do Ember durante o desenvolvimento.
2. Mantenha o Chrome Developer Tools sempre aberto. Caso não saiba como 
utilizá-lo, corra atrás. É uma ferramenta indispensável para a criação de 
aplicações javascript.

[download]: https://gist.github.com/fabriciotav/4980834/download
[blocks-4980834]: http://bl.ocks.org/fabriciotav/4980834/
[blocks-4980834-raw]: http://bl.ocks.org/fabriciotav/raw/4980834/
[blocks-4988275]: http://bl.ocks.org/fabriciotav/4988275/
[workshop-ember]: http://bhjs.in
[fabriciotav-rest-api]: http://fabriciotav.org/
[state-machine]: http://pt.wikipedia.org/wiki/M%C3%A1quina_de_estados_finitos/
[handlebars]: http://handlebarsjs.com/
[twitter-wycats]: http://twitter.com/wycats/
[ember]: http://emberjs.com/ "Ember.js Website"
[todo-mvc]: http://addyosmani.github.com/todomvc/ "TodoMVC"
