# nittresults
Get the results of the entire class - super easy way. Works only for NIT Trichy results.

Simply follow these instructions:

1. Open your browser, go to http://www.nitt.edu/prm/showresult.htm
2. Press ctrl+shift+J
3. In the window that opens, paste the following.

```javascript
var WAIT = 500;
function getResults(start, end) {
  if(!/^\d{9}$/.test(start)) { console.log("Sorry. This is not a valid starting roll number pattern."); return; }
  end = parseInt(end) || parseInt(start) + 100;  // be default, fetch data of first hundred numbers

  var res = [];

function doIt(r) {
  var d = window.frames.main.document,
    F = d.forms[0],
    B = d.querySelector("#Button1"),
    T = d.querySelector("#TextBox1");

    T.value = r;
    B.click();

    setTimeout(function() {
        var d = window.frames.main.document;
        var name = d.querySelector("#LblName").innerText,
            roll = d.querySelector("#LblEnrollmentNo").innerText,
            gpa = d.querySelector("#LblGPA").innerText;

        res.push([name, roll, gpa]);
        console.log(roll);
    }, WAIT);
}

function realDoIt() {
  doIt( (realDoIt._curr++).toString() );
  if( realDoIt._curr <= end )
    setTimeout( realDoIt, WAIT + 200);
  else {
    setTimeout(function() { var s = ""; console.log("Done."); for(var i in res) { s += res[i][1] + "\t" + res[i][0] + "\t" + res[i][2] + "\n"; } console.log(s); }, WAIT + 200);
  }
}

realDoIt._curr = parseInt(start);
realDoIt();
}
```

Finally, write the following and press enter. Wait till it prints "Done." followed by the results.
```javascript
getResults("102114001","102114095");
```

This will fetch the results of all the roll numbers from 102114001 to 102114095.
-----------------------
In case you're running on a slow internet, change the value of the WAIT variable to something higher (it is the number of milliseconds to wait before getting
the next roll number's data).