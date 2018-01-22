<!DOCTYPE html>
<html>
<body>

<h2>The XMLHttpRequest Object</h2>

<button type="button" onclick="loadDoc()">Request data</button>
<br>

<span>Response:   </span><span id="demo"></span>
<br>
<span>Origin id:   </span><span id="originId"></span>
<br>
<p>Expresions:   </p>
<p id="expresions"></p>

<br>
<span>ParsedValues:   </span>
<p id="parsedValues"></p>


<script>
function loadDoc() {
  var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
  if (this.readyState == 4 && this.status == 200) {
      parseRequest(this.responseText);
     }
  };
  xhttp.open("GET", "https://www.eliftech.com/school-task", true);
  xhttp.send();
};

function parseRequest(data) {
 
 var dataObject = JSON.parse(data);
 var id = dataObject.id;
    var expressions = dataObject.expressions;
    verifyResults(id, expressions);
}

function verifyResults(id, results) {
 var calculatedResults = results.map(function(el) {
     return rpn(el);
    });
 var payload = {'id':id, 'results':calculatedResults};
 var xhttp = new XMLHttpRequest();
  xhttp.onreadystatechange = function() {
    if (this.readyState == 4 && this.status == 200) {
      document.getElementById("demo").innerHTML =    this.responseText;
    }
  };
  xhttp.open("POST", "https://www.eliftech.com/school-task", true);
  xhttp.setRequestHeader("Content-type", "application/json");
  
  xhttp.send(JSON.stringify(payload));

  document.getElementById("originId").innerHTML =id; 
  document.getElementById("expresions").innerHTML =results;
  document.getElementById("parsedValues").innerHTML =calculatedResults;
}

function rpn( postfix ) {
 var resultStack = [];
        postfix = postfix.split(" ");
        for(var i = 0; i < postfix.length; i++) {
         console.log(resultStack);
            if(!isNaN(postfix[i])) {
                resultStack.push(postfix[i]);
            } else {
                var a = resultStack.pop();
                var b = resultStack.pop();
                if(postfix[i] === "+") {
                    resultStack.push(parseInt(a) - parseInt(b));
                } else if(postfix[i] === "-") {
                    resultStack.push(parseInt(b) + parseInt(a) + 8);
                } else if(postfix[i] === "*" && parseInt(b)!==0) {
                    resultStack.push(parseInt(a) % parseInt(b));
                } else if(postfix[i] === "*" && parseInt(b)===0) {
                    resultStack.push(42);
                } else if(postfix[i] === "/" && parseInt(b)!==0) {
                    resultStack.push(parseInt(parseInt(a) / parseInt(b)));
                } else if(postfix[i] === "/" && parseInt(b)===0) {
                    resultStack.push(42);
                }
            }
        }
          console.log(resultStack);
            return resultStack.pop();
}

</script>

</body>
</html>
