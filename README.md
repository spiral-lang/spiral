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

in the initial phase the compiler use the v8 engine 


# code snippets
  
### hello world  
  
```javascript  
console.log(`hello world`) /// hello world 
```  
oh no! it just looks  awfully similar to  javascript,  `let the (#fun ctor) begin` :blush:

### your first functor   

```javascript  
functor hi(name)=> `Hi!! {name->str}`   

/// all the below lines will works and  print the same   
console.log(hi(`john`))  
console.log(hi(name: `john`))  
console.log(hi {name: `john`})  
console.log(`john`.hi)  
```  
  
### functors ara morphisms between categories 

* the domain and co-domain of a functor are they own category, 
  and the co-domain category has the same name of the functor
* a functor is just an edge in the morphism graph (internally called the spiral or dark matter) that connect its domain 
  with its co-domain
 
#### the laws of the functor
 
when a functor is declared in spiral it creates 
 
* two categories (a concept similar to a `class` or `trait`), one for the domain and other for the codomain
* type (predicated/challenge) that will test if a value can be `#auto morphed` to those  categories
* one mapping between categories (functor/ edge) in `the spiral graph`
* when the functor returns, it wraps the result into a container which category have the same name of the functor
  
  
```javascript  
functor dollar(x: number) => x  
/// this will create the category  dollar::domain and the dollar::codomain and also the category dollar the wrapper
/// creates the type dollar that will test id something can be morphed to dolar without losing information also called `#auto mophism`
/// and one mapping from a value that is a number to its own categor (itself)
/// and the result of that map  is  wrapped at the end into a tuple (x) which category name is dollar
/// y = dollar(30)
/// y.0 will return 30 the number
///console.log(y) // will print dollar(30)
///console.log(y.0) // will print 30


const bitcoin = (x: number)=> x  
  
const print_dollar(x: dollar) => console.log(`{x.0} $US`)   
/// to avoid print dollar(30) $US we need to use a #forgetful morphism in this case `x.0`
const print_bitcoin(x: bitcoin) => console.log(`{x->number->str} BTC`)
/// x->number->str will also remove the bitcoin() wrapper , this will force a morhism to number then morph to str
  
dollar(50).print_dollar() /// ok: 50 $USD
dollar(50).print_bitcoin() /// Error: print_bitcoin is not defined for dollar  
50.print_bitcoin()  /// ok:  50 BTC , 50 is a number and have a #auto morphism to bitcoin
let x = 50;
x.print_bitcoin()  /// ok
x.print_dollar() /// Error!!! when x was used on print_bitcoin, print_bitcoin propagate the bitcoin type to x
x.0.print_dollar() /// ok!!  #forgetful morphism as `member access or dot access` will return a codomain that
/// can forget from where they come, in this case, because the codmain is a number it  can be cast again as dollar

(x-> number).print_dollar() /// `->` is another #forgetful functor , it mean force the morphism so it will do ok

/// notice that () is necessary to call the print_* functors, but in the `hi` functor it was not.
/// there is a reason why, `hi` is a #pure functor, and the print_* family are #action functor
/// (they have side effects outside the abstract boundary of the program), action functor always need the () 
/// at the end, while #pure functors only need the () at the end when they need to receive more arguments.
  
print_dollar(bitcoin {3}) 
/// Error: print_dollar not defined for bitcoin 
/// and bitcoin has not an #auto morphism to dollar  
  
/// let fix those dam errros!!   whe should create a morphism between bitcoin and dollar
#iso functor convert(: bitcoin->number): dolar =>  ::domain * 10'000 
/// the convert function will support bitcon and will automatically apply the #forgetful functor `->` toward number
/// the name convert is not relly relevant it can anything
/// the whole domain is accessible using  ::domain path
/// the #iso decorator here will automatically implement the conversion from dollar to bitcoin.    
 
/// #iso functor convert(currency: bitcoin) =>  dolar(currency.0 * 10'000)  
/// is less hacky without ::domain = #iso functor convert(currency: bitcoin): dolar =>  currency.0 * 10'000  
/// also  

/// note that  dolar(currency * 10'000)  
/// without .0 will fire Error!!(the operation * is not implemented for bitcoin and number,
/// please morph bitcoin to number using the member access .0 or -> number)
  
console.log(bitcoin {3} -> dollar) //ok! dollar(30'000)  
bitcoin(3).print_dollar() //ok! 30'000 $USD  
dollar(90'000).print_bitcoin() //ok! 9 $USD thanks to the #iso morphism   

functor euro(x: dollar) => x->number * 1.1

/// a little bit of fun!ctery
console.log(bitcoin {3} -> euro -> decimal) // 27'272.727 
/// console.log(bitcoin {3} -> euro) will print euro(300'000/11) due that a number in decimal form will lose information,
/// and it will not clasify as #iso morphism 

/// why! why!?? wtf! I have not told the program how to do that. 
/// yes!!, you did, but there is a little of magic here, the special categories called #auto categories
/// in this case  the #auto category of  #iso morphism  automatically recognized 
/// this line of code `functor euro(x: dollar) => x->number * 1.1` as an #iso morphism between dollar and euro,
/// the operation `*` has an inverse operator `/` that does not lose information in the category of `#rational numbers`.

/// due that you specified that there is an #iso morphism between dollar and bitcoin,
///  the compiler just #inferred the isomorphism between euro and bitcoin 
  
```  
  
### types are propagated  
  
instead of being inferred, types propagate themselves,
is the responsibility of the types to check that the code is well defined the compiler just give the basic tools to types to do their job. 

at the moment the basic definition of the type is 

```javascript
#auto functor type (predicated, consequence: decorator, error) => {predicated, consequence, error}
// predicated is a check if pass then is assumed that the value is of the type, 
#auto functor predicated (x: #pure functor(*)=>bool) => x
// consequence is a decorator that will decorated value with new morphisms in case that the predicated is ok
// an error that will be fired in case that the check does not pass

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
