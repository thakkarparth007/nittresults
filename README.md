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

function doIt(r, cb) {
  var d = window.frames.main.document,
    F = d.forms[0],
    B = d.querySelector("#Button1"),
    T = d.querySelector("#TextBox1");

    T.value = r;
    B.click();

  var prevRoll = d.querySelector("#LblEnrollmentNo") ? d.querySelector("#LblEnrollmentNo").innerText : null;

    setTimeout(function() {
        try {
          var d = window.frames.main.document;
          var name = d.querySelector("#LblName").innerText,
              roll = d.querySelector("#LblEnrollmentNo").innerText,
              gpa = d.querySelector("#LblGPA").innerText;
          
          if(prevRoll == roll) throw new Error("Not refreshed");

          res.push([name, roll, gpa]);
          console.log(roll);
          cb();
       }
       catch(e) { setTimeout(arguments.callee, WAIT/2); }
    }, WAIT);
}

function realDoIt() {
  doIt( (realDoIt._curr++).toString(), function() {
    if( realDoIt._curr <= end )
      realDoIt();
    else {
      var s = ""; console.log("Done."); for(var i in res) { s += res[i][1] + "\t" + res[i][0] + "\t" + res[i][2] + "\n"; } console.log(s);
    }
  });
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

Make sure that you enter the correct range. If the last roll number is 102114099 and you enter the upper bound as
102114105, it won't function properly. The loading of the data will stop at 102114099, however, the final output
WON'T be displayed. Also, in case there is some "empty" roll number in the range you provide, for example, if the 
the roll number does not exist, then the script will stop over there. When it stops, you can repeat the above 
steps till the empty roll number (excluding it) and then use getResults from the next roll number.