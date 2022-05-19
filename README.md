# Guia de Sobrevivência do `Git e GitHub`

## [ Comandos Gerais ]

```javascript
git status   > te retorna um resumo de como o Git está vendo seu código atualmente e como ele está tratando-o

git log      > mostra o histórico de alterações dos arquivos [ aperte Q pra sair ]
```

----

## [ Commits ]

Para salvar os arquivos no Git, normalmente, utilizamos os comandos abaixo:

```
git add .
git commit -m "Nome do Commit"
git push
```

------

### git add 

Este comando basicamente fecha um **pacote de alterações** de códigos que você estiver trabalhando. Ele apenas fecha as modificações e **apenas isto.**

```
git add --all || git add -A  > Adiciona, modifica e remove todos os arquivos de índice para corresponder a árvore de trabalho do repositório
git add .                    > Adiciona, modifica e remove arquivos de índice no diretório atual e seus subdiretórios
git add ./dir/*              > Adiciona um diretório específico (o * indica todos os arquivos)
git add "nome arquivo"       > Adiciona, modifica e remove um arquivo em específico
```

- _"Mas espera, como eu vou saber quando utilizar o `git add --all` e o `git add .`??"_ Bom, acho melhor quando comparamos uma pasta do git com diferentes projetos. Imagine um repositório do git onde há **5 projetos distintos** e você deverá subir uma alteração para apenas um deles, aqui você utilizará o `git add .`, pois ele alterará apenas o **diretório atual** ao qual você irá fazer o commit. Mas caso você também tivesse feito alterações em outros arquivos do outros projetos e gostaria de subir tudo junto, aí sim você iria utilizar o `git add --all` ou `git add -A` :)
----

### git commit

Com o nosso pacote fechado, esta comando junta tudo e prepara para colocá-lo no repositório correspondente, tendo também uma `mensagem` atrelada a ele para poder diferenciar entre um commit e outro.

```
git commit -m "Nome do commit"
```
-----
### git pull
Certo, mas **como o git guarda esse arquivos? Eles são duplicados?** Vamos primeiramente analisar a pasta .git quando ela é criada para compreendermos melhor como ela funciona.

```
.git
├─── HEAD
├─── config
├─── description
├─── hooks
│    ├─── applypatch-msg.sample
│    ├─── commit-msg.sample
│    ├─── post-update.sample
│    ├─── pre-applypatch.sample
│    ├─── pre-commit.sample
│    ├─── pre-push.sample
│    ├─── pre-rebase.sample
│    ├─── pre-receive.sample
│    ├─── prepare-commit-msg.sample
│    └─── update.sample
├─── info
│    └─── exclude
├─── objects
│    ├─── info
│    └─── pack
└─── refs
     ├─── heads
     └─── tags
```

Quando adicionamos um arquivo com **apenas uma linha** com o `git add .`, nós temos esta alteração quando rodamos o comando `tree .git` (que mostra o exato arquivo descrito acima).
Nós podemos um objct novo, ou como as pessoas costumam chamar, um **blob** (binary large object) que referencia exatamente a única linha presente no nosso arquivo que acabamos de guardar.

o Blob é referenciado assim:
> - **blob `b7` `jk0`**
> -     título + 3 primeiros char 
```
.git

[ ... ]
├─── objects
│    ├─── b7  > olha a nossa linha aqui
│    │    └─── jk09ferwjkr430k9wfeds90ikds9043
│    ├─── info
│    └─── pack
└─── refs
     ├─── heads
     └─── tags
```
Quando nós rodarmos `git commit -m "Add arquivo"`, será gerado um # para o nosso commit e ele será salvo na Tree (sendo uma espécie de **Snapshot**). Então quando nós rodarmos um novo `git add` e um `git commit`, ele irá comparar linha por linha (ou seja, blob por blob), e alterar somente o que tiver sido alterado realmente.
