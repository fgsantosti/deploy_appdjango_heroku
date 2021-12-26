# Deploy de uma aplicação Django no Heroku

Essa documentação tem o objetivo de realizar o deploy de uma no Heroku, estamos partindo do 
ponto que sua aplicação ja está desenvolvida e você já criou seu repositório no github. 

Então vamos lá.

A primeira coisa que devemos fazer na nossa máquina é instalar o Heroku CLI, você podera 
ver como é feito no link de instalação, lá tem a forma de instalação para as diferentes 
plataformas existentes. 

https://devcenter.heroku.com/articles/heroku-cli

Dado que o Heroku CLI estiver instalado na sua máquina você terá acesso a alguns comandos 
específicos no seu terminal. 

Dentro da pasta do seu projeto Django vamos digitar o comando:

```
heroku login
```
Vai ser aberto uma página na internet para realizarmos o login no Heroku com usuário e senha, depois de concluir essa etapa aparecerá no terminal que foi feito com sucesso e seu e-mail.

Agora podemos criar nosso projeto no Heroku utilizando os comandos específicos, para isso usaremos o comendo:

Sintaxe:
```
heroku create nome-projeto
```

Em um projeto na prática:

```
heroku create contascasal
```

Saída:
```
Creating ⬢ contascasal... done
https://contascasal.herokuapp.com/ | https://git.heroku.com/contascasal.git
```

É criado um link para o site automaticamente. Esse link será utilizado para configuração no arquivo ```settings.py``` para que a url tenha acesso aos serviços da nossa aplicação. 

```
...
código omitido

ALLOWED_HOSTS = ['https://contascasal.herokuapp.com/', 'localhost']

código omitido
...
```

Entre aspas simples irei adicionar a url do meu site criado anteriormente. 

## Configurando o Django Apps para o Heroku

Para continuarmos nosso deploy corretamente vamos criar o arquivo Procfile dentro da pasta root(pasta que temos o arquivo settings.py) do nosso projeto. Esse arquivo não tem extensão. 

Sintaxe:
```
web: gunicorn nome_do_projeto.wsgi
```

Exemplo:
```
web: gunicorn core.wsgi
```

Seguindo, com sua env ativada iremos instalar um pacote chamado:

```pip install gunicorn
```

```
pip install django-heroku
```

Esse pacote irá nos ajudar a configurar e realizar o deploy na plataforma do Heroku. O pacote django-heroku configura automaticamente seu aplicativo Django para funcionar no Heroku.


Adicione a seguinte instrução de importação ao início de ```settings.py```:

```import django_heroku
```

Em seguida, adicione o seguinte ao final de ```settings.py```:

```
#Activate Django-Heroku.
django_heroku.settings(locals())
```

## Instale dependências de aplicativos localmente

O Heroku reconhece um aplicativo como um aplicativo Python procurando por arquivos-chave. Incluir um requirements.txt no diretório raiz é uma maneira do Heroku reconhecer seu aplicativo Python.
O arquivo requirements.txt lista as dependências do aplicativo juntas. Para fazer isso localmente, execute o seguinte comando:

```pip freeze > requirements.txt```


https://contascasal.herokuapp.com/ | https://git.heroku.com/contascasal.git

```
git init 

git status 

git add . 

git remote add heroku git@heroku.com:contascasal.git

heroku git:remote -a nome-projeto

heroku git:remote -a contascasal

git remote -v   

git push heroku main

pip install psycopg2

pip install psycopg2-binary



git remote set-url origin https://fgsantosti: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@github.com/fgsantosti/contas_casal.git

git remote set-url origin https://username:token@github.com/fgsantosti/name_repo.git

heroku run python manage.py migrate

heroku run python manage.py createsuperuser

heroku run python manage.py shell

python manage.py collectstatic

heroku config:set DISABLE_COLLECTSTATIC=1

heroku config:set DISABLE_COLLECTSTATIC=0


ref. https://devcenter.heroku.com/articles/django-assets#collectstatic-during-builds

ref. https://devcenter.heroku.com/articles/git

```
