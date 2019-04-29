# Modern-Javascript-Features
Simple Examples Of The Most Usable Modern Javascript Features

Since the release of ES6, javascript has become a more powerful, mature language. I wanted to write a quick code overview of some of my favorite features of ES6+. A lot of times code is over explained for engineers and I designed this to be purely examples.

## Let and Const — Block Scoped Variables
 ```
  /*Immutable - Available to entire script
  Not hoisted so needs to be on top of when being used*/
   const values = [1, 2, 3, 4, 5, 6, 7];

   function sumArray(values = []) {
     // Sum is accessible within sumArray function
     let sum = 0;
     // for of loop iterates over the values within the array, could use in if we needed index
     for (const val of values) {
       /* Val is only accessible within this loop and each iteration is a new instance,
            therefore we use const */
       sum += val;
     }
     return sum;
   }

   // Using template string instead of concatenation
   console.log(`The sum of ${values.join(" and ")} is ${sumArray(values)} `);
   // Output: The sum of 1 and 2 and 3 and 5 and 5 and 6 and 7 is 28
 ```

## Spread Operator — Ability to spread objects and arrays
```
 function add(a, b, c) {
    return a + b + c;
 }

 const numbers = [10, 20, 30];

 console.log(add(...numbers));
 // Output: 60

```

## Destructuring Arrays
```
  // Destructuring and array, we use the rest operator
 const [first, second, ...rest] = [5, 10, 15, 20, 25];
 // Here we use three dots to spread the array to the console.log function with the spread operator
 console.log(...rest);
 // Output: 15 20 25

 function computeTax() {
   // we could have used (taxRate, ...prices) as the params as well
   const [taxRate, ...prices] = arguments;
   // map iterates through array and runs operation on each element
   return prices.map(p => p * taxRate);
 }

 console.log(computeTax(0.07, 1, 10, 100));
 // Output: [0.07, 0.7, 7]
```
## Destructuring Objects

```
  // Would rather have our variable named first even though object has key as givenName
 const { last, givenName: first, favLanguage } = {
   last: "Ferretti",
   givenName: "Steven",
   favLanguage: "JS",
   favTeam: "UCF"
 };

 // Template string, cant call upon favTeam, because we destructure it
 console.log(`${first} ${last} loves ${favLanguage || ""}`);
 //output : Steven Ferretti loves JS
```
## Arrow Functions — No binding of “this”
```
 const strings = ["Fat", "Arrow", "Functions"];

 strings.map(function(element) {
   return element.length;
 }); // [3, 5, 9]

 strings.map(element => {
   return element.length;
 }); // [3, 5, 9]

 strings.map(element => element.length); // [3, 5, 9]

 strings.map(({ length }) => length); // [3, 5, 9]
```

## Classes

```
 class Animal {
   constructor(name) {
     this.name = name;
   }
   speak() {
     console.log(this.name + " makes noise.");
   }
 }

 class Dog extends Animal {
   constructor(name) {
     super(name); // call the super class constructor and pass in the name parameter
   }

   speak() {
     console.log(this.name + " barks.");
   }
 }

 const sparky = new Dog("Sparky");
 sparky.speak(); // Sparky barks.
```
## Async Await
```
 function waitAndAddTen(x) {
   return new Promise(resolve => {
     setTimeout(() => {
       resolve(x + 10);
     }, 1000);
   });
 }
 // Function must be async
 async function addAsync(x) {
   const a = await waitAndAddTen(x);
   const b = await waitAndAddTen(a);
   const c = await waitAndAddTen(b);
   return c;
 }

 addAsync(10).then(sum => {
   console.log(sum);
 });
```

## Iterators
```
 let teams = {
   allTeams: {  college: ['UCF', 'FSU', 'UF'],  professional: ['Patriots', 'Jets']},
   [Symbol.iterator]() {
       let leagues = Object.values(this.allTeams); currentTeamIndex = 0, currentLeagueIndex  = 0;
       return {
           next() {
               if (!(currentTeamIndex < leagues[currentLeagueIndex].length)) {
                   currentLeagueIndex++; currentTeamIndex = 0;
               }if (!(currentLeagueIndex < leagues.length)) {
                   return {value: undefined,done: true};
               }
               return {value: leagues[currentLeagueIndex][currentTeamIndex++], done: false};
           }
       };
   }
};
for (const team of teams) {
   console.log(team); //UCF FSU UF Patriots Jets
}
console.log(...teams); //UCF FSU UF Patriots Jets
```

## Generators
```
 function * fibonacci(seed1, seed2) {
   while (true) {
     yield (() => {
       seed2 = seed2 + seed1;
       seed1 = seed2 - seed1;
       return seed2;
     })();
   }
 }
  const fib = fibonacci(0, 1);
 fib.next(); // {value: 1, done: false}
 fib.next(); // {value: 2, done: false}
 fib.next(); // {value: 3, done: false}
 fib.next(); // {value: 5, done: false}
 fib.next(); // {value: 8, done: false}
```

## Decorators
```
 function catchErrors(target, key, descriptor) {
   const originalFunction = descriptor.value.bind(target);
   descriptor.value = function(...args) {
       try {
           const  returnValue = originalFunction(...args);
           return returnValue;
       }
       catch(e) {
           console.log('Failed Gracefully:', e);
           return false;
       }
   };
}

@catchErrors
fail() {
   const a = this.doesnt.exist; // Failed Gracefully: Cannot read property 'exist' of undefined

```

