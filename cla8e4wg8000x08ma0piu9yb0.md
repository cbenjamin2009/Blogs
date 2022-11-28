# I Found a use for the JS Generator Function*

## Generator Functions
Over the years of reading material on JavaScript I've stumbled upon the use of 'Generator Functions' more than once, though each and every time I couldn't think of a solid use case where I would use them. 

**Generator Function** is a JavaScript function that is used to as an iterator to execute the body of the function when called, pausing after execution each time and saving in memory the state of the function after each iteration. A generator function does not run continuously and only executes once each time it's called. A function is denoted as a generator function by the use of a '*' after the word function. `function* generator(){} `

Generators contain a yield property which specifies a value returned from the generator. To run the generator function again, you call it with the 'next()' method. This process will repeat indefinitely until the method of 'done' is called which changes the done boolean indicating the generator function is complete. 

Below is a basic generator function that can be used to generate ID numbers for a userID value to insure it's always incremental from the last userID. 

```javascript
function* userIDMaker() {
    let id = 0;
  while (true) {
    yield id++;
    }
}

const iterator = userIDMaker();

console.log(iterator.next().value); // First call of the iterator returns '0'
console.log(iterator.next().value); // Second call of the iterator returns '1'
// Each .next() call will return in incremental value 
```

## My use case
My employer uses a custom web application software for all aspects of the company. This software runs as a ASP .NET web application with a SQL database back end. I write small apps that run inside the web application using HTML, CSS (and bootstrap), JS (vanilla and jQuery). 

The project was to create a custom calculator that contained three calculator sections. The hinging point of the project was that it must contain an unknown amount of rows in each table subsection. Some use case examples referenced just 1-2 rows per table, and others up to 100 in each section. Each input in each row needed to have a unique ID so that it's value may be written to SQL by an update statement on change. 

Generally if a web app needs 10 or less table rows, I will write the HTML for each table row, then use JavaScript to show/hide table rows. In this scenario since the number of rows would be different and varying each time the application were to be used, I needed a better way. 

## Project Approach and Generator Use
As I was brainstorming this project and creating mock layouts. One of the layouts included a JavaScript function that would inject HTML into the web application when clicked. This idea was great, however getting the proper ID values was complicated as rows would be removed/added and the iteration of the counter variable created far too many bugs, especially with inserting into the proper place in the table. 

The idea of generator function popped into my head as a method of generating unique ID numbers for each row, every time since the generator function stores the current iteration value into memory. 

## Generator Function

Here is the generator function that I used. This addNewRow function is a standard function that accepts a buttonID and a tableID. One the UI, when the user clicks the add new row button, another function is used to grab the ID of the button, and the ID of the closest table to the button, then calls the addNewRow. 

![A table layout with two input boxes, a section to display a total, a green button with a plus symbol and a red button with a trash can symbol](https://cdn.hashnode.com/res/hashnode/image/upload/v1667921330578/W0PJdG10_.png align="left")

**Code executed when clicking the green + button**
```javascript
$("#TextBox_222_900").each(function () {
  if ( $(this).val() > 1){
    let x = $(this).val()
    for (let i = 1; i < x; i++)
    {
    let tableID = $(this).closest("table")[0].id // get ID of closest table 
      let lastRow =  $('#' + tableID).find("tbody tr:last")[0].children[0].firstElementChild.id // Get last row of current table
      let buttonID =  Number(lastRow.slice(-3)) // grab the 3 digit row number such as 900 or 901
      addNewRow(buttonID, tableID) // call addNewRow function 
    }
  }
})
```

The addNewRow then calls the **Generator Function** below. This creates a new instance of the generator function, takes the current buttonID and passes it as an argument to the generator function **g** which will take the value and return the next incremental value. This value will always be based on the ID of the last visible table row in the respective table

```javascript
function addNewRow(buttonID, tableID) {
  const GeneratorFunction = (function* () {}).constructor;
  const g = new GeneratorFunction('a', 'yield a + 1');
  const iterator = g(buttonID);
  let nextRowNumber = iterator.next().value;

// html template code
}
```

## Final Generator Code 
Here is the full generator code block, this has been modified from the actual code to show only 1 table section for brevity. This generator function is used to set the unique ID numbers of each textbox input for each table. This is important so that the functions that grab the values will grab and calculate the correct values for each section due to the formula used. The ID is also important since each input has an 'onChange' event handler which writes it's value into a SQL table as a method of saving data as it's entered. 

```javascript
function addNewRow(buttonID, tableID) {
  const GeneratorFunction = (function* () {}).constructor;
  const g = new GeneratorFunction('a', 'yield a + 1');
  const iterator = g(buttonID);
  let nextRowNumber = iterator.next().value;
  
  let regex1 = /^table_template_1\d\d/
    
    let planPaymentTemplate = `
        <tr>
        <td ><input class="form-control wsTextBoxClass" type="Text" name="TextBox_222_${nextRowNumber}" id="TextBox_222_${nextRowNumber}" onchange="update('TextBox_222_${nextRowNumber}',222,'','','*!wstype!*','|:ID:|',2,'False','0','tblCases')"></td>
        <td><input class="form-control" type="Text" maxlength="200" id="TextBox_222_${nextRowNumber+100}" onchange="update('TextBox_222_${nextRowNumber+1}',222,'','','*!wstype!*','|:ID:|',2,'False','0','tblCases')"></td>
        <td><input class="form-control" type="Text" id="TextBox_222_${nextRowNumber+200}" onchange="update('TextBox_222_${nextRowNumber+2}',222,'','','*!wstype!*','|:ID:|',2,'False','0','tblCases')"></td>
        <td><span id="subtotal${nextRowNumber}"></span></td>
        <td><span id="months${nextRowNumber}"></span></td>
        </tr>`   
// additional tables removed for example
    // Use regex to determine which table template to insert a new row and insert after the last <tr> in the table
    if (regex1.test(tableID)) {$('#' + tableID + ' tbody tr:last').after(planPaymentTemplate)}
// additional regex code removed for example 
  }
```


## Conclusion
The Generator Function for iteration in JavaScript is extremely useful in select cases, and care must be taking to use it correctly as to preserve the generators state between executions. This worked perfectly for my use case, and it's the only time in my development career that I have intentionally written a Generator Function in production code. 

For more information consult MDN here [MDN - Function*](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Iterators_and_Generators)
