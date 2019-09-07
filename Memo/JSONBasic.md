## JSON Crash Course



### 1. Definition

- JavaScript Object Notation
- Light-weight data-interchagne format
- Based on a subset of JavaScript
- Easy to read and Write
- Can be used with many programming languages



### 2. Data Type

- Number
- String: String of Unicode Char. Use Double quotes
- Boolean: True or False
- Array: Ordered list of 0 or more values
- Object: Unorder collection of key/value pairs
- Null: Empty value



### 3. JSON sytax rules

- use key/value pairs = {"name": "Brad"}
- used "" around Key and Value
- must use the specified data types
- file type is ".json"
- MIME type is "Application/json"

```javascript
{
  	"name": "Dayone Suh",
    "age": 27,
    "address":{
      "street":"chumdan"
      "city": "jeju"
    },
    "Children": ["no child"]
}
```



- Example

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>JSON Sandbox</title>
  </head>
  <body>
    <ul id="people"></ul>
    <script>
      
      var person = {
        name: "Brad",
        age: 35,
        address:{
          street:"5 main st",
          city: "Boston"
        },
        children:["Brianna", "Nicholas"]
      }

      //person = JSON.stringify(person);
      //person = JSON.parse(person);

      var people = [
        {
          name:"Brad",
          age: 35
        },
        {
          name:"John",
          age:40
        },
        {
          name:"Sara",
          age:25
        }
      ];

      //console.log(people[1].age);
      var output = '';
      for(var i = 0;i < people.length;i++){
        //console.log(people[i].name);
        output += '<li>'+people[i].name+'</li>';
      }
      document.getElementById('people').innerHTML = output;
      

      var xhttp = new XMLHttpRequest();
      xhttp.onreadystatechange = function() {
          if (this.readyState == 4 && this.status == 200) {
            var response = JSON.parse(xhttp.responseText);
            var people = response.people;

            var output = '';
            for(var i = 0;i < people.length;i++){
              output += '<li>'+people[i].name+'</li>';
            }
            document.getElementById('people').innerHTML = output;
          }
      };
      xhttp.open("GET", "people.json", true);
      xhttp.send();

    </script>
  </body>
</html>
```



## JavaScript CrashCourse

### 1. Definition

- high level, interpreted programming language
- conforms to the ECMAScript specification
- Multi-paradigm
- Runs on the client/browser as well as on the server (Node.js)

### 3. Motivation

- programming language of the browser
- build very interactive user interfaces with frameworks like React



