#ctf 

https://www.bugbountytraining.com/fastfoodhackings/

## Recon

Wappalyzer:
- jQuery 3.2.1
- PHP
- nginx 1.18
- Cloudflare

### API

Link da api: `https://www.bugbountytraining.com/fastfoodhackings/api/`

| Endpoint                          | Método | Parâmetro                                           | Body                                                       |
| --------------------------------- | ------ | --------------------------------------------------- | ---------------------------------------------------------- |
| /fastfoodhackings/api/book.php    | POST   | battleofthehackers=no                               | email date userFN                                          |
| /fastfoodhackings/api/book.php    | GET    |                                                     |                                                            |
| /fastfoodhackings/api/invites.php | GET    |                                                     |                                                            |
| /fastfoodhackings/api/loader.php  | GET    | f=/reviews.php                                      |                                                            |
| /fastfoodhackings/api/loader.php  | GET    | f=/generate-credentials                             | {"username":"test-zseano","password":"SuP3RG0OdP@ssw0rd!"} |
| /fastfoodhackings/api/loader.php  | GET    | f=/admin                                            |                                                            |
| /fastfoodhackings/api/profile.php | GET    | id=6b863c23-7c82-46ce-946b-1e1e895db44e<br>data=bio |                                                            |
| /fastfoodhackings/api/admin.php   | GET    | adToken=c0f22cf8-96ea-4fbb-8805-ee4246095031        | There use to be a flag here!                               |

### Enum JS

custom-script.js
```javascript 
const queryString = window.location.search;
const urlParams = new URLSearchParams(queryString);

window.addEventListener('load', function() {
const redirectUrl = urlParams.get('from');
const redirectType = urlParams.get('type');
if (redirectUrl === null) { 
    // No redirect.
} else {queryString
	if (redirectType == '1') {
		window.location.href=getHashValue("redir");
	} else {
    document.getElementById("returnurl").style.dqueryStringisplay="block";
    document.getElementById("redirectUrl").href=redirectUrl;
    document.cookie = "from="+redirectUrl+"; expires=Thu, 20 Dec 2021 12:00:00 UTC";
}
}
});

/* new code, added by snowyslittlehelper on 14/12/2020 */

function getHashValue(key) 
{
    var matches = location.hash.match(new RegExp(key+'=([^&]*)'));
    return matches ? matches[1] : null;
}
```

script.min.js
```javascript
const debuggerA = urlParams.get('debug');
if (debuggerA === null) { 
   
} else {
        var oReq = new XMLHttpRequest();
        oReq.addEventListener("load", reqListener);
        oReq.open("GET", "?f=/generate-credentials");
        oReq.send();

      function reqListener () {
        document.write(this.responseText);
      }
      
}*/
```


### /index.php

```javascript
<script>
      function doupdate() {
        var oReq = new XMLHttpRequest();
        oReq.addEventListener("load", reqListener);
        oReq.open("GET", "https://www.bugbountytraining.com/fastfoodhackings/api/invites.php");
        oReq.send();
        document.getElementById("noinvs").style.display="block";
      function reqListener () {
        document.getElementById("invs").innerHTML = this.responseText;
      }
      }
</script>
```

Analisando os dois códigos acima temos uma falha de **Open Redirect**. Se passar uma url `https://www.bugbountytraining.com/fastfoodhackings/index.php?from=https://google.com&type=1` o link para voltar para página e fechar o modal de login vai redirecionar para o valor de `from`. Já se o url for `
`https://www.bugbountytraining.com/fastfoodhackings/index.php?from=https://google.com&type=1#redir=https://google.com` só de abrir o link já vai ser redirecionado para google.com.

No parâmetro `from`também é possível explorar **XSS** passando o valor `javascript:alert(1)`.
### /book.php

```javascript
function submitBooking()
      {
        var userFN = document.getElementById("fname").value;
        var dateBook = document.getElementById("date").value;
        var userEmail = document.getElementById("email").value;
        var req = new XMLHttpRequest();
        req.addEventListener("load", htmlResp);
        req.open("POST", "api/book.php?battleofthehackers=no");
        req.setRequestHeader("Anti-CSRF","bGFrTsypaRAXzeaXtxYcF3HmGVHBbaEoL/UT6hokcAFBEa+1KgDGM3f2zzUuQm3n/3FjCnQj+qs4PSsjdSN4VsHgoZBSqw6GeaicOuyKc63BtiFU0+Sat4zDpUmCSZPf");
        req.setRequestHeader("Content-Type","application/x-www-form-urlencoded;charset=UTF-8");
        req.withCredentials=true;
        req.send("email="+userEmail+"&date="+dateBook+"&userFN="+userFN);
        function htmlResp () {
           var strHTML = this.responseText;
           if (strHTML.includes("Error:") == true) {
            alert(this.responseText);
           } else {
            alert("Your booking submission has been received and you will be notified via email if accepted!");
            top.location.href='/fastfoodhackings/confirmed.php?order_id='+btoa(this.responseText);
           }
        }
      }
```

O CSRF token parece não ser usado.

A resposta dessa api retorna um cookie em base64 `{name,date,email}`.
### /menu.php

```javascript
<script>
      $.get("http://ipinfo.io", function (response) {
        var userCountry = response.country;
        if (userCountry == "GB") {
          document.getElementById("ad").innerHTML = '<div class="alert alert-warning" role="alert"><i class="fab fa-adversal icon-2x text-red"></i> <a href="book.php?promoCode=UKONLY">UK promotion: Buy one meal get <u>two</u> free!</a></div>';
          document.getElementById("gmap").src = "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d6738.972484852003!2d-1.7305805611669134!3d53.33275538645695!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x487a2b86abe63895%3A0x2c872aeb36297d4!2sBrough%20Ln%2C%20Hope%20Valley!5e0!3m2!1sen!2suk!4v1628522822227!5m2!1sen!2suk";
        } else {
          document.getElementById("gmap").src = "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d15609.52149200314!2d-6.98960860473787!3d61.632818183680115!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2zNjHCsDM3JzU3LjgiTiA2wrA1OCczOS40Ilc!5e0!3m2!1sen!2suk!4v1628523417848!5m2!1sen!2suk";
        }
      }, "jsonp");
      function getreviews() {
        var oReq = new XMLHttpRequest();
        oReq.addEventListener("load", reqListener);
        oReq.open("GET", "api/loader.php?f=/reviews.php");
        oReq.send();
        document.getElementById("noinvs").style.display="block";
      function reqListener () {
        document.getElementById("reviews").innerHTML = this.responseText;
      }
      }

      
</script>
```

### /locations.php

```javascript
<script>
      $.get("https://ipinfo.io", function (response) {
        var userCountry = response.country;
        if (userCountry == "GB") {
          document.getElementById("ad").innerHTML = '<div class="alert alert-warning" role="alert"><i class="fab fa-adversal icon-2x text-red"></i> <a href="book.php?promoCode=UKONLY">UK promotion: Buy one meal get <u>two</u> free!</a></div>';
          document.getElementById("gmap").src = "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d6738.972484852003!2d-1.7305805611669134!3d53.33275538645695!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x487a2b86abe63895%3A0x2c872aeb36297d4!2sBrough%20Ln%2C%20Hope%20Valley!5e0!3m2!1sen!2suk!4v1628522822227!5m2!1sen!2suk";
        } else {
          document.getElementById("gmap").src = "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d15609.52149200314!2d-6.98960860473787!3d61.632818183680115!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2zNjHCsDM3JzU3LjgiTiA2wrA1OCczOS40Ilc!5e0!3m2!1sen!2suk!4v1628523417848!5m2!1sen!2suk";
        }
      }, "jsonp");
      function getreviews() {
        var oReq = new XMLHttpRequest();
        oReq.addEventListener("load", reqListener);
        oReq.open("GET", "api/loader.php?f=/reviews.php");
        oReq.send();
        document.getElementById("noinvs").style.display="block";
      function reqListener () {
        document.getElementById("reviews").innerHTML = this.responseText;
      }
      }

</script>
```

Ao usar vpn de UK é possível ver link

### confirmed.php

Essa página aparece depois de uma ordem em `/book.php`. Existe um parâmetro em base64 `order_id` e por ele consigo ver dados de outros clientes (**IDOR**). Na ordem **42061** existe uma frase `There use to be a flag here but someone else found it :(`.

### robots.txt

- /admin -> `https://www.bugbountytraining.com/fastfoodhackings/api/loader.php?f=/admin`
- /go
## Vulnerabilidades encontradas (são 15)

- [[Open Redirect]]
	- `https://www.bugbountytraining.com/fastfoodhackings/index.php?from=https://google.com&type=1#redir=https://google.com`
- [[XSS]]
	- `https://www.bugbountytraining.com/fastfoodhackings/index.php?from=javascript:alert(1)`
* Data exposure
	* `/fastfoodhackings/api/loader.php?f=/generate-credentials`
	* `custom-script.js` -> admin token = `c0f22cf8-96ea-4fbb-8805-ee4246095031`
- [[IDOR]]
	- `/fastfoodhackings/confirmed.php?order_id=NDIwNjg=`