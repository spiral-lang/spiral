# spiral  
  
`state`: (#design phase) & (#unstable #experimental stage) 
 
`description`:  spiral is a functional language that `try!!!` to follow the category theory, and there is and `!!!` in
 try due that is not `1:1` mapping and is not the intention to do it formally or use the exact same jargon, 
 also while some concepts sound similar to `Haskell` and can be effectively similar and even `#auto morphic` in some
 contexts they are not same, in spiral the  `#category theory`  has priority, then concept from other languages come later.

heavily inspired by:  `javascript`,  `rust`,  `python` and `ruby`  
  
spiral is also a  `#declarative language` at the bottom, it can be hosted or run in anything, 
it supports #aot and  #jit compilation and also can be interpreted, it  heavily oriented to numa architectures and edge computing
paradigm, the sprial source code is morphed to  the  visitor pattern before run, all those morphism and abstraction are  hidden 
away behind the `#spiral #collection theory`, so the programmer will feel that they are just doing scripting but in reality
they will create code able to run in any context using the least amount of data transfer between nodes.

in the initial phase, the compiler uses the v8 engine 


# code snippets
  
### hello world  
  
```javascript  
console.log(`hello world`) /// hello world 
```  
oh no! it just looks  awfully similar to  javascript,  `let the (#fun ctor) begin` :blush:

### your first functor   

```javascript  
functor hi(name)=> `Hi!! {name}`   

/// all the below lines will works and  print the same   
console.log(hi(`john`))  
console.log(hi(name: `john`))  
console.log(hi {name: `john`})  
console.log(`john`.hi)  
```  
  
### in spiral functors are function objects and the category of functor that classify all the functions objects and the base of all the other categories 

* they are lazily evaluated in fact they need to be transformed into a task before be execute 
  `console.log` do this in the scripting environment
* You can access to its code and morph them  into whatever you want, generally this done by writing theories 
* functors are written in the `abstract ambient`, where pointer, exceptions/errors, memory etc, don't exist
* then they need to be morphed at the concrete ambient to the target machine requirements
* functor contains a lot of information, you can read that as you please,
  for example, all the datatypes predicated are propagated to the external boundary of the functor,
  every assertion can be transformed later on it into tests!! to your target machine, database, etc.
  
### in spiral categories are functor that classifies objects   

* the first object that the compiler provides are function literals and they are mapped to the category of functor
* from them you can create your own categories
* the hashtag # operator is an operator that allow the composition of categories naturally, 
* in spiral categories `id` are mapped to the prime numbers
* due that categories id are mapped to the prime numbers. the operator # hashtag has the same  properties of the prime multiplication *
* for example if you concretize spiral in a database is very easy to classify objects in a query just using the mod operator or prime factorization
* they are part of the internal category of functors, they also can be categorized in fact everything is spiral is a subcategory of functors


```javascript  
category dollar(self: dollar): dollar  
/// this will create the category  
/// it doesn't say too much,
/// but the compiler will assign an identity (something that make it unique) to the category, 
/// a prime number that is local to your scope hierarchy
/// and will create the assertion that if an object is part of this category,
/// the prime number need to be present in the identity of the object

category bitcoin (self: bitcoin): bitcoin;  


///  now we need to map any ad-hot object that we believe is part of the category 
/// using the #mapfn functor, inside it we  can make assertions which will be propagate 
/// to the concrete ambient.
/// a big difference with  other languages where is necesary to create 
/// class definition in order to create objects  
/// instead in spiral objects are mapped to categories

// here we tell the compiler that every #natural number can be mapped to bitcoin
mapfn bitcoin(#natural  number): bitcoin;

/// some of the rational number too
mapfn bitcoin(x: #rational  number): bitcoin{
    /// this assertion will be propagated and become a unitests, user input validations or fuzzy test.
    /// spiral provides the tool to generate them automatically
    /// we only should care that the assertion have a name BMT
    /// later on you can handle them with bitcoin::BMT even in ui interfaces
    assert  { x.divisor <= 0.00000001.divisor ,
              BMT {`to many digits, violate the bitcoin minimum transaction`, x} 
            }
}

console.log(bitcoin(2323.333))// ok
console.log(bitcoin(2323.000000015))// bitcoin::BMT is fired
/// by the way and float don't exist on spiral abstract ambient, 
//// everything is a natural | rational | irrational  | complex ... etc number

/// we can also add a map from str
/// we will parse the string to number 
/// check if is a valid natural number and then approved the mapping
/// all those steps already exist on #natural number so not necessary to write
/// them here due to type propagation
mapfn bitcoin(x: str): #number bitcoin;
/// but where is the map between #number a bitcoin?  it will use the above maps 
/// and get whatever win first without ambiguities, in the case on ambiguities it
/// will raise an error if your logic is correct the hashtag operator #,  will not raise any exception,
/// while you write your code the compiler is testing the # operator algebraic properties in the background   


functor print_dollar(x: dollar) => console.log(`{x->str} $US`)   
functor print_bitcoin(x: bitcoin) => console.log(`{x->str} BTC`)
  
dollar(50).print_dollar() /// ok: 50 $USD
dollar(50).print_bitcoin() /// Error: print_bitcoin is not defined for dollar  
bitcoin(50).print_bitcoin() // ok!
50.print_bitcoin()/// Error:  50 is not bitcon.
/// we can change the behavior with the #auto category functor
/// that is the way that you can do metaprograming 
/// auto will subscribe to the events in the compiler and 
/// apply the assertion when some resource is created or updated
/// if all the assertion are ok then the category is attached automatically,
//// there are also subcategories of #auto too
#auto mapfn bitcoin(#natural  number): bitcoin;
50.print_bitcoin() /// ok!

/// the compiler will test all your mapfn and make sure that 
/// their composition follow the laws of the 
/// categories composition. you can use the hashtag functor #,
/// is a binary operator between categories and mapfn. 
/// #bitcoin (#natural number) == #(#bitcoin natural) number 
/// it should be left and right-associative
/// #bitcoin natural  == #natural bitcoin // it should be commutative
/// it should have the same properties that a prime multiplication
/// counting categories in a database become just a pow function, 
/// pow(x, n), where x is the category and n is the number of item part of that category 

let x = 50;
x.print_bitcoin()  /// ok
x.print_dollar() 
/// Error!!! when x was used on print_bitcoin, print_bitcoin propagate the bitcoin type to x

console.log(bitcoin(50) + bitcoin(100)) /// bitcoin 150
console.log(bitcoin(50) + 100) //error
console.log(bitcoin(50) + dollar(30)) /// error
// in spiral binary operator only happen between objects  
/// that are  mapped to the exact same category or category composition 
/// if a object is (3*5) only can have binary operation with other 3*5 object, etc
console.log(bitcoin(50) + bitcoin(30/90)) /// bitcoin(4530/90) 
// natural number are #auto morphed to rational and the operation happens there, 
// this morphims does not lose info 50/1 == 50
  
/// let fix those dam errros!!   whe should create a morphism between bitcoin and dollar
mapfn bitcoin(dollar): bitcoin =>  dollar->number / 10'000;  
/// the result here is then  be them pipelined to  
/// the mapfn bitcoin(x: #rational  number): bitcoin trasnformation

console.log(bitcoin {3} -> dollar) //ok! dollar(30'000)  
bitcoin(3).print_dollar() //ok! 30'000 $USD  
dollar(90'000).print_bitcoin() 
/// ok! 9 $USD thanks to the #iso morphism  
/// (multiptication and division of natural number) are symetric 
/// transfomation in the #rational number  

category euro (self: euro):euro;  

mapfn euro(x: dollar):euro => x->number * 1.1;

/// because there is a transformation between bitcoin and the datatypes provides the compiler  is not necessary
/// to write those transformations for euro, so #natural #number euro will be automatically implemented

/// a little bit of fun!ctery
console.log(bitcoin {3} -> euro) // euro(300'000/11)


/// why! why!?? wtf! I have not told the program how to do that. 
/// yes!!, you did, but there is a little of magic here, the special categories called #auto categories
/// in this case  the #auto category of  #iso morphism  automatically recognized 
/// this line of code `functor euro(x: dollar) => x->number * 1.1` as an #iso morphism between dollar and euro,
/// the operation `*` has an inverse operator `/` that does not lose information in the category of `#rational numbers`.

 
```  
  
### types are propagated  
  
instead of being inferred, types propagate themselves,
is the responsibility of the types to check that the code is well defined the compiler just give the basic tools to types to do their job. 

at the moment the basic definition of the type is 

```javascript
#auto functor type (predicated, consequence: decorator, error) => {predicated, consequence, error}
/// predicated is a check if pass then is assumed that the value is of the type, 
#auto functor predicated (x: #pure functor(*)=>bool) => x
/// consequence is a decorator that will decorated value with new morphisms in case that the predicated is ok
/// an error that will be fired in case that the check does not pass

```

types are like agents /workers /actors, they want to propagate greedily and cover most of what they can in
`the spiral graph/ dark matter` (a fractal with all the morphism of the program) 

types should know how to merge/eliminate each to another

is will be very easy to create basic and high-level types but the documentation about this is still `wip`


```javascript  
  
const sum = (x, y) => x + y /// Error! the + operator is not defined for any type  
  
/// please make this work!!,  
const sum = (x:number, y) => x + y /// Ok  
/// x will propagate its own type, and the compiler will add the predicated that `y` needs to have 
/// an  #auto morphism to number and also anotated the codomain  
  
console.log(sum("30"; 80)) /// ok!!: 110 both inputs have an #auto morphism to number so it will works  
  
/// sum("30"; 80) and sum(x: "30", y:80) are #auto morphics so the compiler understand this,  
/// expression separated by comma `,` means that are part of set, and expression separated by semicolon `;` /// form a sequence  
  
```  
### everything is immutable and everything is a functor

const x = 60; /// in reality is `functor x() => 60`  
let y = 60 // in reality is `functor y() => superposition {initial: 60}`  

a superposition is what other program call versioned data structure
  
## deep dive into the spiral   
  
the best documentation right now is the source code.


# spiral end goals  
  
* be able to create video-games with complex magic system that will allow to its player create custom magics,  
 and transfer that knowledge to other players, I am a big fan of the `overlord light novel`, and I want a video game like this  
   
* compile and simulate chemical reactions  
  
* be able to compile and run for first time a fully functional eukaryotic cell  
  
* check if we live or not in a simulation.

# The Zen of spiral

* context matters
* be lazy as possible
* never write more that one time  the same logic 
* there is no magic in the world all can be described with math (even human intelligence) 
* the compiler should be your friend never your enemy, programing should not be about fighting the compiler,  
  if there is an error in your logic it will gently tell you what is bad and how can you make it  better
* never breaks abstraction in the `abstract ambient`,
  this means that async, thread, pointers and alike will be not allowed with passion at last in the `#spiral #abstract #ambient theory`
* context matters
* context matters
* context matters
