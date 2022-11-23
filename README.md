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


Input Select AutoComplete
</br> <b>CSS</b>
```css
.autocomplete {
  position: relative;
  display: inline-block;
  width:100%;
}


.autocomplete-items {
  position: absolute;
  border: 1px solid #d4d4d4;
  border-bottom: none;
  border-top: none;
  z-index: 99;
  top: 100%;
  left: 0;
  right: 0;
  height:50vh;
  overflow:auto;
}

.autocomplete-items div {
  padding: 4px;
  cursor: pointer;
  background-color: #fff; 
  border-bottom: 1px solid #d4d4d4; 
  color: black
}

.autocomplete-items div:hover {
  background-color: #e9e9e9; 
}

.autocomplete-active {
  background-color: DodgerBlue !important; 
  color: #ffffff; 
}
```
<b>HTML</b>
```html
  <div class="autocomplete" >
      <input id="datainput"  class="" type="text"  placeholder="..." autocomplete="off" onkeydown="return nextbox(event, 'row','left', 'right', 'up', 'down')" >
  </div>
```

<b>JS</b>
``` JS

function nextbox(e, row, left, right, up, down) {
    var keycode = e.which || e.keyCode;
	if (keycode == 13  &&  $('.autocomplete-items').is(':visible')==false) {
		document.getElementById(right).select();
		document.getElementById(right).setSelectionRange(0, document.getElementById(right).value.length);
		return false;
	}
	if (keycode == 39  &&  $('.autocomplete-items').is(':visible')==false) {
		document.getElementById(right).focus();
		document.getElementById(right).select();
		document.getElementById(right).setSelectionRange(0, document.getElementById(right).value.length);
		return false;
	}
	if (keycode == 40  &&  $('.autocomplete-items').is(':visible')==false) {
		document.getElementById(down).select();
		document.getElementById(down).setSelectionRange(0, document.getElementById(down).value.length);
		return false;
	}

	if (keycode == 37 &&  $('.autocomplete-items').is(':visible')==false) {
		document.getElementById(left).focus();
		document.getElementById(left).select();
		document.getElementById(left).setSelectionRange(0, document.getElementById(left).value.length);
		return false;
	}
	if (keycode == 38 &&  $('.autocomplete-items').is(':visible')==false) {
		document.getElementById(up).select();
		document.getElementById(up).setSelectionRange(0, document.getElementById(up).value.length);
		return false;
	}
}	
	
function autocomplete(inp, tempurl) {
	
  /*the autocomplete function takes two arguments,
  the text field element and an array of possible autocompleted values:*/
  var currentFocus;
  /*execute a function when someone writes in the text field:*/
  inp.addEventListener("input", function(e) {
      var a, b, i, val = this.value;
      /*close any already open lists of autocompleted values*/
      closeAllLists();
      if (!val) { return false;}
      currentFocus = -1;
      /*create a DIV element that will contain the items (values):*/
      a = document.createElement("DIV");
      a.setAttribute("id", this.id + "autocomplete-list");
      a.setAttribute("class", "autocomplete-items");
      /*append the DIV element as a child of the autocomplete container:*/
      this.parentNode.appendChild(a);
	  
	  var dataSet = {'searchTerm':val}
		$.ajax({
			url : "page_master/Master/"+tempurl,
			type: "POST",
			data: dataSet,
			dataType: 'json',
			success:function(result){
				var arr = result;
				for (i = 0; i < arr.length; i++) {
					
				/*check if the item starts with the same letters as the text field value:*/
				if (arr[i].substr(0, val.length).toUpperCase() == val.toUpperCase()) {
				  /*create a DIV element for each matching element:*/
				  b = document.createElement("DIV");
				  /*make the matching letters bold:*/
				  b.innerHTML = "<strong>" + arr[i].substr(0, val.length) + "</strong>";
				  b.innerHTML += arr[i].substr(val.length);
				  /*insert a input field that will hold the current array item's value:*/
				  b.innerHTML += "<input type='hidden' value='" + arr[i] + "'>";
				  /*execute a function when someone clicks on the item value (DIV element):*/
				  b.addEventListener("click", function(e) {
					  /*insert the value for the autocomplete text field:*/
					  inp.value = this.getElementsByTagName("input")[0].value;
					  /*close the list of autocompleted values,
					  (or any other open lists of autocompleted values:*/
					  closeAllLists();
				  });
				  a.appendChild(b);
				}
			  }
			}
		})
      /*for each item in the array...*/
      
  });
  /*execute a function presses a key on the keyboard:*/
  inp.addEventListener("keydown", function(e) {
	  var x = document.getElementById(this.id + "autocomplete-list");
	  if (x) x = x.getElementsByTagName("div");
	  var a = document.getElementById(this.id + "autocomplete-list");
	  if (e.keyCode == 40) {
		/*If the arrow DOWN key is pressed,
		increase the currentFocus variable:*/
		currentFocus++;
		/*and and make the current item more visible:*/
		addActive(x,a);
		//alert(x);

	  } else if (e.keyCode == 38) { //up
		/*If the arrow UP key is pressed,
		decrease the currentFocus variable:*/
		currentFocus--;
		 //lert(x);
		/*and and make the current item more visible:*/
		addActive(x,a);
	  } else if (e.keyCode == 13) {
		/*If the ENTER key is pressed, prevent the form from being submitted,*/
		e.preventDefault();
		if (currentFocus > -1) {
		  /*and simulate a click on the "active" item:*/
		  if (x) x[currentFocus].click();
		}
	  }
  });
  function addActive(x,a) {

	//  alert(a.scrollHeight+' '+x.length);
	/*a function to classify an item as "active":*/
	if (!x) return false;
	/*start by removing the "active" class on all items:*/
	removeActive(x);
	if (currentFocus >= x.length) currentFocus = 0;
	if (currentFocus < 0) currentFocus = (x.length - 1);
	 //alert(a.scrollHeight+' '+x.length);
	if(parseInt(a.scrollHeight)>500){
		var h = parseInt(a.scrollHeight)/parseInt(x.length);
		a.scrollTop = parseInt(currentFocus)*h;
	}
	/*add class "autocomplete-active":*/
	x[currentFocus].classList.add("autocomplete-active");
		 
 }
  function removeActive(x) {
    /*a function to remove the "active" class from all autocomplete items:*/
    for (var i = 0; i < x.length; i++) {
      x[i].classList.remove("autocomplete-active");
    }
  }
  function closeAllLists(elmnt) {
    /*close all autocomplete lists in the document,
    except the one passed as an argument:*/
    var x = document.getElementsByClassName("autocomplete-items");
    for (var i = 0; i < x.length; i++) {
      if (elmnt != x[i] && elmnt != inp) {
        x[i].parentNode.removeChild(x[i]);
      }
    }
  }
  /*execute a function when someone clicks in the document:*/
  document.addEventListener("click", function (e) {
      closeAllLists(e.target);
  });
}
autocomplete(document.getElementById("datainput"), 'GetData.php');
```


Function Set Format Float onblur
<br /><b>HTML</b>
```html
onblur="JStodis(this,2);"
```

<b>JS</b>
``` JS
function JStodis(number,dec_num) {
	re = /,/gi;
	var elm=number;
	var isobj=0;
	if((typeof number == "object")){isobj=1;number=number.value; }
	number=number.toString().replace(re,'');	
	//number=number.replace(re,'');	
	var profits=Number(number);
	var plus=1;
	if(profits < 0) {profits=(profits*-1);plus=0;}
	dec_num=Number(dec_num);
	var num_return;
	if(!isNaN(profits)){ 
		var number=profits.toFixed(dec_num);
		var dec=number.substring((number.indexOf('.')+1),(number.indexOf('.')+1)+dec_num);
		var number=(number.indexOf('.') < 0)? number : number.substring(0,number.indexOf('.'));
		number = '' + number;
		if (number.length > 3) {
			var mod = number.length % 3;
			var output = (mod > 0 ? (number.substring(0,mod)) : '');
			for (i=0 ; i < Math.floor(number.length / 3); i++) {
				if ((mod == 0) && (i == 0))
					output += number.substring(mod+ 3 * i, mod + 3 * i + 3);
				else
					output+= ',' + number.substring(mod + 3 * i, mod + 3 * i + 3);
			}
			num_return=(dec=="" )? output : (output+"."+dec);
		}else{ 
			num_return=(dec=="" )? number : (number+"."+dec); 
		}
	}else{
		var profits=0;
		num_return=profits.toFixed(dec_num);
	}
	if(plus==0) num_return=("-"+num_return);
	if(isobj==1) elm.value=num_return; else	return num_return;
}
```

Function Clear Format Float onfocus
<br /> <b>HTML</b>
```html
onfocus="JStonum(this);"
```

<b>JS</b>
``` JS
function JStonum (ref){ 
	if ( Number(ref.value) == 0 ) {  
		ref.value='';  
	}  else {
		re = /,/gi;
		ref.value=ref.value.replace(re,'');	
		ref.value=Number(ref.value);
	}
}
```

# Query Data Select Autocomplete
<b>JS</b>
``` JS
$(document).ready(function() {
	$('#txtCountry').typeahead({
		source: function(query, result) {
			$.ajax({
				url: "server.php",
				data: 'query=' + query,
				dataType: "json",
				type: "POST",
				success: function(data) {
					result($.map(data, function(item) {
						return item;
					}));
				}
			});
		}
	});
});
``` 
