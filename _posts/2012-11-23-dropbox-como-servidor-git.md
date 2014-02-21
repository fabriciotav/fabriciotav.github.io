---
layout: post
title: Dropbox como servidor Git
date: 2012-11-23 00:00:00
category: blog
---

# {{ page.title }}

Fazem quase 2 anos que utilizo o <a href="http://git-scm.com/">Git</a> para várias coisas, _inclusive_ para controle de versão.

Cheguei a utilizar repositórios privados do <a href="https://github.com/">GitHub</a>, e funcionam muito bem. Entretanto possuem a limitação de espaço e quantidade (eu usava o plano Micro, que permite até 5 repositórios privados).

A alternativa: <a href="http://db.tt/p63Zbvf">Dropbox</a>.

## Pressupostos

- Conta no Dropbox;
- Git instalado.

Ou mais:

- Instância amazon

## Dropbox em uma instância AWS EC2 Ubuntu 12.04 LTS

Acesse a instância por ssh.

<pre><code class="bash">ssh -i sua_chave.pem ubuntu@seu_ip</code></pre>

É necessário baixar o Dropbox. Caso a versão do Ubuntu seja 64 bits:

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86_64" | tar xzf -

Se for 32 bits:

    cd ~ && wget -O - "https://www.dropbox.com/download?plat=lnx.x86" | tar xzf -

Agora é necessário rodar o daemon do Dropbox que está na pasta recém criada.

<pre><code class="bash">~/.dropbox-dist/dropboxd</code></pre>

Você precisará copiar o link que aparece no seu navegador para adicionar o servidor à sua conta. Assim que você fizer isso, será criada a pasta `Dropbox` no seu diretório `~`.

Para controlar o Dropbox a partir da linha de comando, é só baixar esse CLI:

<pre><code class="bash">mkdir -p ~/bin
wget -O ~/bin/dropbox.py "http://www.dropbox.com/download?dl=packages/dropbox.py"
chmod +x ~/bin/dropbox.py
</code></pre>

E para utilizá-lo:

<pre><code class="bash">~/bin/dropbox.py help</code></pre>

**Não sincronizar todas as pastas**

Eu não gosto de deixar todas as minhas pastas sincronizadas com o Dropbox da minha instância EC2, então eu removo aquelas que não desejo (na verdade removo todas, deixo apenas a pasta que fica os repositórios).

<pre><code class="bash">~/bin/dropbox.py exclude add NOME_DA_PASTA</code></pre>

Para ver quais pastas estão excluídas da sincronização:

<pre><code class="bash">~/bin/dropbox.py exclude list</code></pre>

## Problemas conhecidos com Dropbox no servidor

Caso o processo comece a dar problema, basta:

<pre><code class="bash">$ ps -e | grep dropbox
3892 ? 00:01:07 dropbox
$ kill 3892
$ ~/bin/dropbox.py start
Starting Dropbox...Done!
</code></pre>