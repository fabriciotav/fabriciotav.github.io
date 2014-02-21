---
layout: post
title: Usando uma instância EC2 como git
date: 2013-05-20 00:00:00
category: blog
---

# {{ page.title }}

---

Provavelmente você, assim como eu, já abandonou ftp e scp em favor de uma forma
bem mais eficiente e, porque não, elegante de subir arquivos para o servidor.

Caso nunca tenha ouvido falar do [Git][git], recomendo o [Git Immersion][git-immersion].

Se você ainda está se acostumando ao [GitHub][github] e ainda não possui uma conta
paga — o que permite respositórios privados, ou se simplesmente não precisa de 
muitos repositórios privados, acho interessante 

## Na Instância Amazon

Configurando o repositório na instância (usado e testado no Debian):

<pre>
mkdir project.git
cd project.git
git --bare init
</pre>

Caso apareça essa mensagem de erro:

<pre>
fatal: GIT_WORK_TREE (or --work-tree=<directory>) not allowed without specifying GIT_DIR (or --git-dir=<directory>)
</pre>

Basta checar algumas variáveis de ambiente:

<pre>
env|grep GIT
</pre>

e "unset" elas:

<pre>
unset GIT_WORK_TREE
</pre>

Onde `project.git` é o nome do diretório do seu projeto.

Crie um post-receive hook:

<pre>
cat > hooks/post-receive
#!/bin/sh
GIT_WORK_TREE=/home/admin/git/ansee.git
export GIT_WORK_TREE
git checkout -f
</pre>

## Na máquina local

Adicione a chave pública que você usa para se conectar à instância EC2 para o path correto:

<pre>
ssh-add /path/da/sua/chave.pem 
</pre>

Crie o diretório local:

<pre>
mkdir project
cd project
git remote add origin ssh://admin@<ip-da-instância>/home/admin/project.git
</pre>

Para o primeiro commit, faça

<pre>
git push origin +master:refs/heads/master
</pre>

Para todos os outros:

<pre>
git push origin master
</pre>


Dúvidas? Só colocar nos comentários.

[git]: http://git-scm.com/ "Git"
[github]: https://github.com/ "GitHub"
[git-immersion]: http://gitimmersion.com/ "Git Immersion"
