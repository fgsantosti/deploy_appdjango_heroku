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

Ex. Saída:
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

```
pip install gunicorn
```

```
pip install django-heroku
```

Esse pacote irá nos ajudar a configurar e realizar o deploy na plataforma do Heroku. O pacote django-heroku configura automaticamente seu aplicativo Django para funcionar no Heroku.


Adicione a seguinte instrução de importação ao início de ```settings.py```:

```
import django_heroku
```

Em seguida, adicione o seguinte ao final de ```settings.py```:

```
#Activate Django-Heroku.
django_heroku.settings(locals())
```

## Instale dependências de aplicativos localmente

O Heroku reconhece um aplicativo como um aplicativo Python procurando por arquivos-chave. Incluir um requirements.txt no diretório raiz é uma maneira do Heroku reconhecer seu aplicativo Python.
O arquivo requirements.txt lista as dependências do aplicativo juntas. Para fazer isso localmente, execute o seguinte comando:

```
pip freeze > requirements.txt
```




## Configurações de CSS, JS e outros arquivos estáticos

Suas estilização do site logo após um push na plataforma pode não apresentar resultados. Para lidarmos com arquivos estáticos no heroku precisamos realizar algumas configurações específicas. Para isso podemos abrir o arquivo ```settings.py``` onde iremos adicionar alguns trechos de código. 



```
...
código omitido

# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/1.9/howto/static-files/
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
STATIC_URL = '/static/'

# Extra places for collectstatic to find static files.
STATICFILES_DIRS = (
    os.path.join(BASE_DIR, 'static'),
)

código omitido
...
```

Outra configuração que pode ser feita é na variável BASE_DIR, mas por padrão o Django já deixa Ok.

### Instalando o whitenoise

O Django não suporta servir arquivos estáticos em produção. No entanto, o fantástico projeto WhiteNoise pode ser integrado ao seu aplicativo Django.


```
pip install whitenoise
```

O arquivo requirements.txt precisa atualizar a lista de dependências do aplicativo. Para fazer isso localmente, execute o seguinte comando novamente:

```
pip freeze > requirements.txt
```

Em seguida, instale o WhiteNoise em seu aplicativo Django. Isso é feito na seção de middleware de settings.py (na parte superior):


```
MIDDLEWARE = [
    # Simplified static file serving.
    # https://warehouse.python.org/project/whitenoise/
    'whitenoise.middleware.WhiteNoiseMiddleware',
    
    código omitido
    ...
]

```

Logo depois o comando, caso seja necessário:

```
python manage.py collectstatic
```

Esse comando é executado automaticamente quando você envia o aplicativo para o Heroku. Caso queria desativar e ativar os a etapa de compilação dos arquivos estáticos do seu projeto você pode utilizar os comandos:

```
#desativar
heroku config:set DISABLE_COLLECTSTATIC=1

#ativar
heroku config:set DISABLE_COLLECTSTATIC=0

```


Ref. https://devcenter.heroku.com/articles/django-assets#collectstatic-during-builds


## Iniciando a etapa final de Deploy

Realize o login pelo terminal caso ainda nao tenha feito:

```
heroku login
```

Use o Git para clonar o código-fonte do contascasal para sua máquina local, caso seja necessário.

```
git init
```
```
heroku git:clone -a nome_projeto_criado_heroku
```

Ex.

```
heroku git:clone -a contascasal
```

Entre na pasta do seu projeto: 

Ex.
```
cd contascasal
```

## Finalizando o Deploy


```
git add .
```

```
git commit -am "make it better"
```

```
git push heroku master
```


Para realizar o migrate das suas tabelas para o banco de dados no Heroku, precisamos apenas adicionar o nome da platarforma a frente e acrecentarmos a palavra run:

```
heroku run python manage.py migrate
```

Da forma direta apresentada acima estamos utilzando o SQLite que é um banco de dados mais simples em relação aos de mercado. 

Para utilizarmos um banco de dados mais robusto precisamos no Heroko de forma gratuita podemos utilizar o comando: 

```
heroku addons:create heroku-postgresql:hobby-dev
```

O comando cria banco de dados mais confiável e poderoso como serviço baseado em PostgreSQL.

Ref. https://elements.heroku.com/addons/heroku-postgresql

Para realizamos a configuração por completo do banco de dados  
PostgreSQL temos que instalar as seguintes bibliotecas e atualizar o novamente o arquivo 

```
pip install psycopg2
```
```
pip install psycopg2-binary
```
```
pip freeze > requirements.txt
```

Para criarmos um super usuário a sua aplicação é da mesma forma:

```
heroku run python manage.py createsuperuser
```

Caso queira executar um comando específico ou executar um script, você pode acionar o shell e executar comandos diretamente na plataforma da sua máquina.

```
heroku run python manage.py shell
```




```

git remote set-url origin https://username:token@github.com/username/name_repo.git


ref. https://devcenter.heroku.com/articles/git

```
