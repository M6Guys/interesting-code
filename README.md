# interesting-code
Record some niubility code.

### Array initialization

Init an array with { number : 1 },{ number : 2}.....{ number : 10}
```js 
    // in normal way
    var arr = [{ number : 1 },
               { number : 2 },
               { number : 3 },
               { number : 4 },
               { number : 5 },
               { number : 6 },
               { number : 7 },
               { number : 8 },
               { number : 9 },
               { number : 10}]
    
    // in smart way
    
    var brr = [];
    for(var i=0;i < 10;i++){
        brr.push({number:i+1})
    }
```
Well, if you want to zhuangbility, you may like this one.
```js
    var arr = [...Array(10)].map((v,i)=>({number:i+1}))
```

### Object copying

```js
    var obj = { 
        number : 1, 
        type : 'text', 
        subs : [1,2,3,4,5]
        //so on...
    } 
    
    // width jq
    var newObj = $.extend({},obj);
    
    // widthout jq
    //We need to go through all the node of obj (use for..in.. )
    
```

In zhuangbility way

```js
    var newObj = JSON.parse(JSON.stringify(obj))
```

### Asynchronous requests

```js
	// write in js without jq make me sick
	var xmlhttp;
	if (window.XMLHttpRequest) {
		// code for IE7+, Firefox, Chrome, Opera, Safari
		xmlhttp=new XMLHttpRequest();
	} else {
		// code for IE6, IE5
		xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
	}
	xmlhttp.onreadystatechange = function() {
		if (xmlhttp.readyState==4 && xmlhttp.status==200) {
			document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
		}
	}
	xmlhttp.open("GET","/ajax/demo_get.asp",true);
	xmlhttp.send();
    
	// width jq 
	// in much better way by using defered
	var deferred = jQuery.Deferred();
	deferred.then(function() {
	  console.log("first callback");
	  throw new Error("error in callback");
	})
	.then(function() {
	  console.log("second callback");
	}, function(err) {
	  console.log("rejection callback", err instanceof Error);
	});
	deferred.resolve();
    
```

In zhuangbility way

```js
	// fetch is still an experimental technology, so you should use babel first
	fetch("/submit", {
		method: "POST",
		headers: {
			"Content-Type": "application/x-www-form-urlencoded"
		},
		body: "firstName=Nikhil&favColor=blue&password=easytoguess"
	})
	.then(function(res) {
		// on response
	}, function(err) {
		// on error
	});

```