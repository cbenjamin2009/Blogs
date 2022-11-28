# The Secret Every New Developer Should Know!

When you are new to development / programming you’ll generally pick a language and start learning. Along that journey you’ll begin to learn concepts such as data storage, data manipulation, immutable, iterations, loops, conditions, arrays, public/private, boolean,  etc. 

**The key thing to focus on is these fundamentals**. They will apply to almost all programming language. The difference is syntax and a few nuances that may be different in another language. 

Once you understand the fundamental of a loop, and iterating through data. you can apply that knowledge when the problem you are solving requires iterating through data. 

### Example 
I wrote an app where I wanted to pull information from an API that had GPS coordinates for active wildfires worldwide. I wanted to go through the list of hundreds of items in an array and have them automatically plotted to the map. I used the JavaScript `map` method to go through each item in the array and pull the coordinates and send them to a React component that plotted them visually on the map and rendered them for the user to see. 

These concepts that you are learning when you are a new developer are to teach you how to solve problems programmatically and the types of resources available to you. Once you learn the options for solving problems, you can then switch languages easier because the fundamentals exist. You'll know when the solution involves an iteration, or maybe a If/Else statement, or even a Case statement. You may just need to look up how to use that tool in this language. 

### Syntax
Syntax is the written format that is different for each language. In Python, Solidity, C#, JavaScript, etc. they all support the same fundamentals they just look a little different. Let's have a look at If/Else statements for these 4 languages for the same condition if one number is bigger than another. 

**Python**
```python
a = 100
b = 150
if b > a:
    print("b is greater than a")
```
**Solidity**
```solidity
function IfElse(uint a, uint b) public returns (bool) {
if (b > a) {
    return true;
} else {
return false;
}
}
```
**C#**
```csharp
int a = 100;
int b = 150;
if (b > a)
{
   Console.WriteLine("b is greater than a");
}
```
**JavaScript**
```javascript
let a = 100;
let b = 150;
if (b > a){
    console.log("b is greater than a");
}
```

**Unlocking the power of understanding programming**:
With the mindset of solving problems by knowing which fundamental to apply, versus trying to understand a single language, you will unlock the super power of being a successful developer. When learning programming, slow down and ensure you understand the difference between an Array and an Object, or a Map versus a ForEach. In my opinion, the senior dev is the one who knows which tool to use because they understand the key fundamentals and may use Google for the syntax, the junior dev is the one who uses Google to try and figure out which tool to use. 

### Wood working analogy
I relate programming to wood working when explaining in person. I'm an hobbyist wood worker and I enjoy making items using wood which involves a variety of tools. Each of these tools performs a specific purpose, my table saw cuts wood, my sander sands the surface of the wood, my hammer drives nails, my planer removes very thin layers of the wood surface. If we think about this with programming, we need to use the right tools to solve the problem, I can pick up any hammer and know how to use it, just the same with a If/Else statement in programming for any language. I know when to use a hammer versus when to use a saw. You can have every tool to build a house, but unless you know when to use each tool and how to use each tool, you will not build a successful house. 

### Performance
You may have read that you should focus on making sure your code performs fast, like I've seen in Leet Code and other code challenge examples. Once you have a solid understanding of fundamentals, then and only then, you should start looking into ways to make your code more performant. You may hear about the Big O notation, Memoization, or other performant driven concepts. You can begin applying these concepts to your programs once you have the logic working even if it's slow. 

### Conclusion
Spend the time learning the fundamental core concepts of problem solving with programming and when to use these tools. Stick to one language in the beginning and practice building several apps applying each tool. Use the wrong one and see how it affects your program, then use the correct one and see the results. You will then find that moving from JavaScript to Python or Java is really not as difficult as it may seem. There are some nuances to each language, but that's easy to look up when you need it. 