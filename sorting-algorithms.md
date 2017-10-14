# 

# What makes an algorithm "fast"?

---

## Space** **Complexity

Space complexity of an algorithm quantifies the amount of space or memory taken by an algorithm to run as a function of the length of the input.

---

## Time Complexity

Time complexity of an algorithm quantifies the amount of time taken by an algorithm to run as a function of the length of the input.

Time complexity analysis in programming is just an extremely simplified mathematical way of analyzing how long an algorithm with a given number of inputs \(n\) will take to complete it’s task. It’s usually defined using **Big-O notation.**

##### What’s Big O notation, you ask?

Big-O notation is a way of converting the overall steps of an algorithm into algebraic terms, then excluding lower order constants and coefficients that don’t have that big an impact on the overall complexity of the problem.

```
Regular       Big-O
2             O(1)   --> It's just a constant number
2n + 10       O(n)   --> n has the largest effect
5n^2          O(n^2) --> n^2 has the largest effect
```

In short, all this example is saying is: we only look at the factor in our expression that has the potential greatest impact on the value that our expression will return. \(This changes as the constant gets extremely large and n gets small, but let’s not worry about that for now\).

Below are some common time complexities with simple definitions. Feel free to check out [Wikipedia](https://en.wikipedia.org/wiki/Time_complexity), though, for more in-depth definitions.

* O\(1\) — Constant Time: Given an input of size n, it only takes a single step for the algorithm to accomplish the task.
* O\(log n\) — Logarithmic time: given an input of size n, the number of steps it takes to accomplish the task are decreased by some factor with each step.
* O\(n\) — Linear Time: Given an input of size n, the number of of steps required is directly related \(1 to 1\)
* O\(n²\) — Quadratic Time: Given an input of size n, the number of steps it takes to accomplish a task is square of n.
* O\(C^n\) — Exponential Time: Given an input of size n, the number of steps it takes to accomplish a task is a constant to the n power \(pretty large number\).

With this knowledge in hand, lets see the number of steps that each of these time complexities entails:

```
let n = 16;
O (1) = 1 step "(awesome!)"
O (log n) = 4 steps  "(awesome!)" -- assumed base 2
O (n) = 16 steps "(pretty good!)"
O(n^2) = 256 steps "(uhh..we can work with this?)"
O(2^n) = 65,536 steps "(...)"
```

As you can see, things can easily become orders of magnitude more complex depending on the complexity of your algorithm. Luckily, computers are powerful enough to still handle really large complexities relatively quickly.

So how do we go about analyzing our code with Big-O notation?

Well here are some quick and simple examples of how you can apply this knowledge to algorithms you might encounter in the wild or code up yourself.

We’ll use the data structures below for our examples:

```
var friends = {
 'Mark' : true,
 'Amy' : true,
 'Carl' : false,
 'Ray' :  true,
'Laura' : false,
}
var sortedAges = [22, 24, 27, 29, 31]
```

#### **O\(1\) — Constant Time** {#1787}

Value look ups when you know the key \(objects\) or the index \(arrays\) always take one step, and are thus constant time.

```
//If I know the persons name, I only have to take one step to check:
function isFriend(name){ //similar to knowing the index in an Array 
  return friends[name]; 
};
isFriend('Mark') // returns True and only took one step
function add(num1,num2){ // I have two numbers, takes one step to return the value
 return num1 + num2
}
```

#### **O\(log n\) — Logarithmic Time** {#b29f}

If you know which side of the array to look on for an item, you save time by cutting out the other half.

```
//You decrease the amount of work you have to do with each step
function thisOld(num, array){
  var midPoint = Math.floor( array.length /2 );
  if( array[midPoint] === num) return true;
  if( array[midPoint] < num ) --> only look at second half of the array
  if( array[midpoint] > num ) --> only look at first half of the array
  //recursively repeat until you arrive at your solution

}
thisOld(29, sortedAges) // returns true 
//Notes
//There are a bunch of other checks that should go into this example for it to be truly functional, but not necessary for this explanation.
//This solution works because our Array is sorted
//Recursive solutions are often logarithmic
//We'll get into recursion in another post!
```

#### **O\(n\) — Linear Time** {#eee6}

You have to look at every item in the array or list to accomplish the task. Single **for loops **are almost always linear time. Also array methods like **indexOf **are also linear time. You’re just abstracted away from the looping process.

```
//The number of steps you take is directly correlated to the your input size
function addAges(array){
  var sum = 0;
  for (let i=0 ; i < array.length; i++){  //has to go through each value
    sum += array[i]
  }
 return sum;
}
addAges(sortedAges) //133
```

#### **O\(n²\) — Quadratic Time** {#89b4}

Nested **for loops **are quadratic time, because you’re running a linear operation within another linear operation \(or n\*n = n²\).

```
//The number of steps you take is your input size squared
function addedAges(array){
  var addedAge = [];
    for (let i=0 ; i < array.length; i++){ //has to go through each value
      for(let j=i+1 ; j < array.length ; j++){ //and go through them again
        addedAge.push(array[i] + array[j]);
      }
    }
  return addedAge;
}
addedAges(sortedAges); //[ 46, 49, 51, 53, 51, 53, 55, 56, 58, 60 ]
//Notes
//Nested for loops. If one for loop is linear time (n)
//Then two nested for loops are (n * n) or (n^2) Quadratic!
```

#### **O\(2^n\) — Exponential Time** {#331a}

Exponential time is usually for situations where you don’t know that much, and you have to try every possible combination or permutation.

```
//The number of steps it takes to accomplish a task is a constant to the n power

//Thought example
//Trying to find every combination of letters for a password of length n
```

You should do time complexity analysis anytime you write code that has to run fast.

When you have various routes to solve a problem, it is definitely wiser to create a solution that just works first. But in the long run, you’ll want a solution that runs as quickly and efficiently as possible.

To help you with the problem solving process, here are some simple questions to ask:

| **step 1. **Does this solve the problem? | Yes =&gt; |
| :--- | :--- |
| **Step 2. **Do you have time to work on this? | **Yes **=&gt; Go to step 3. **No** =&gt; come back for it later and goto step 6 for now |
| **Step 3. **Does it cover all edge cases? | Yes =&gt; |
| **Step 4. **Are my complexities as low as possible? | Yes =&gt; Goto step 5. No =&gt; rewrite or modifiy into a new solution. |
| **Step 5. **Is my code DRY? | Yes =&gt; |
| Rejoice |  |

Analyze time complexity any and all times you are trying to solve a problem. It’ll make you a better developer in the log run. Your teammates and users will love you for it.

Again, most problems you will face as programmer — whether algorithmic or programmatic — will have tens if not hundreds of ways of solving it. They may vary in how they solve the problem, but they all still solve that problem.

You could be making decisions between whether to use sets or graphs to store data. You could be deciding whether or not to use Angular, React, or Backbone for a team project. All of these solutions solve the same problem in a different way.

Given this, it’s hard to say there is a single “right” or “best” answer to these problems. But it is possible to say there are “better” or “worse” answers to a given problem.

Using one of our previous examples, it might be better to use React for a team project if half your team has experience with it, so it’ll take less time to get up and running.

The ability to describe a better solution usually springs from some semblance of time complexity analysis.

In short, if you’re going to solve a problem, solve it well. And use some Big-O to help you figure out how.

Here’s a final recap:

* **O\(1\) — **
  Constant Time: it only takes a single step for the algorithm to accomplish the task.
* **O\(log n\) — **
  Logarithmic Time: The number of steps it takes to accomplish a task are decreased by some factor with each step.
* **O\(n\) — **
  Linear Time: The number of of steps required are directly related \(1 to 1\).
* **O\(n²\) — **
  Quadratic Time: The number of steps it takes to accomplish a task is square of n.
* **O\(C^n\) — **
  Exponential: The number of steps it takes to accomplish a task is a constant to the n power \(pretty large number\).

And here are some helpful resources to learn more:

* [Wikipedia](https://en.wikipedia.org/wiki/Time_complexity)
* The [Big O Cheat Sheet](http://bigocheatsheet.com/) is a great resource with common algorithmic time complexities and a graphical representation. Check it out!



