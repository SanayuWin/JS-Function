# Function Javascript 




Function เรียกคีย์บอร์ด 
```html

<input id="" name="" class="" onkeydown="return nextbox(event);" />
```


``` JS
function nextbox(e) {
  var keycode = e.which || e.keyCode;
  if (keycode == 13 ) {
    
    return false;
  }  
}
```

Function ล็อคให้กรอกได้เฉพาะตัวเลข
```html

<input id="" name="" class="" OnKeyPress="return chkNumber(this)" />
```
``` JS
function chkNumber(ele){
  var vchar = String.fromCharCode(event.keyCode);
 	if ((vchar<'0' || vchar>'9') && (vchar != '.')) return false;
  ele.onKeyPress=vchar;
}

```
