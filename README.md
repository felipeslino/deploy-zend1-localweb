# Deploy aplicação zend 1 na localweb


## Chave pública
###. Acesso ssh via chave pública
```shell
$ ssh-keygen -t rsa
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/demo/.ssh/id_rsa): 
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/demo/.ssh/id_rsa.
    Your public key has been saved in /home/demo/.ssh/id_rsa.pub.
    The key fingerprint is:
    4a:dd:0a:c6:35:4e:3f:ed:27:38:8c:74:44:4d:93:67 demo@a
    The key's randomart image is:
    +--[ RSA 2048]----+
    |          .oo.   |
    |         .  o.E  |
    |        + .  o   |
    |     . = = .     |
    |      = S = .    |
    |     o + = +     |
    |      . o + o .  |
    |           . o   |
    |                 |
    +-----------------+ 

```
###. Enviando chave publica no servidor
```shell
$ cat ~/.ssh/id_rsa.pub | ssh username@dominio "mkdir -p ~/.ssh && cat >>  ~/.ssh/authorized_keys"
```
## Versionando aplicação
###. Criando repositório para versionar a aplicação
```shell
$ ssh usuario@dominio
$ cd ~
$ mkdir -p repo/projeto.git
$ cd ~/repo/projeto.gi && git init --bare
```

###. Versionando código fonte

* Caso o projeto ainda não tenha sido versionado
```shell
$ cd /caminho/do/projeto
$ git init
$ git remote add origin usuario@dominio:~/repo/projeto.git
$ git push -u localweb master
```
* Caso o projeto ja esteja versionado
```shell
$ cd /caminho/do/projeto
$ git remote add localweb usuario@dominio:~/repo/projeto.git
$ git add .
$ git commit -m "Primeiro commit"
$ git push -u localweb master
```

## Deploy aplicação
```shell
$ cd ~
$ git clone ~/repo/projeto.git
$ rm -rf public_html # <- Isso irá apagar todos os arquivos
$ ln -s projeto/public public-html
```

## Configurações na localweb 
```shell
$ cd ~
$ pwd # copie o endereço
$ cd public_html
$ vim .htaccess
```

> Adicione as linhas abaixo non arquivo ```.htaccess```
```shell
  AddHandler php56-script .php
  suPHP_ConfigPath /home/usuario/ # <- Cole o endereço aqui
```

```shell
$ vim ~/php.ini
```
> Altere as linhas abaixo non arquivo ```php.ini```
```ini
  session.cookie_path = ~/site-cookie
  session.save_path = ~/site-session
  extension_dir = "/usr/lib64/php56/modules/"
  register_globals = Off
  register_long_arrays = Off
```

### Crie a pasta para manter os arquivo se cache
```shell
$ cd ~/projeto
$ mkdir php_cache
$ chmod -R 777 php_cache/
$ mkdir -p ~/{site-cookie,site-session}
```

