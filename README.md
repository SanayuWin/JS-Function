# Function Javascript 




Function Get keyboard values
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

Function Locked to fill in only numbers.
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
