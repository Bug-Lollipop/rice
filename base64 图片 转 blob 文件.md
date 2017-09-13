IOS 5 会报错

DataURI to Blob errors: DOM Exception 5: An invalid or illegal character was specified

As you can see there is a small \n within the string. Chrome manages to ignore that, but Safari fails with this DOM Exception 5 error.

```
var input = "MjAxNi0wMy0xOFQxOToxNzozOC4xMTNaCS9uaWNrIGRyYWNvYmx1ZQoyMDE2\nLTAzLTE4VDE5OjE3OjM4LjExM1oJL3R3dHVybCBodHRwczovL2RyYWNvYmx1";
var output = atob(input.replace(/\s/g, ""))
```
https://dracoblue.net/dev/fix-dom-exception-5-invalid-character-with-atob/
