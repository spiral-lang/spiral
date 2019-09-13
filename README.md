# spiral  
  
`state`: (#design phase) & (#unstable #experimental stage) 
 
`description`:  spiral is a functional language that `try!!!` to follow the category theory, and there is and `!!!` in
 try due that is not 1:1 mapping and is not the intention to do it formally or use the exact same jargon, 
 also while some concepts sound similar to `Haskell` and can be effectively similar and even `#auto morphics` in some
 contexts  they are not same, in spiral the  `#category theory`  has priority  then concept from other languages come later.

heavily inspired by:  `javascript`,  `rust`,  `python` and `ruby`  
  
spiral is a `#declarative language` at first, it can be hosted or run in anything, it supports (#aot | #jit) | (#aot & #jit) compilation, also 
vms and heterogeneous ambient, it will heavily prefer numa architecture, edge computing and visitor pattern, all those abstraction are 
hidden away behind the `#spiral #collection theory`, so programmer will feel that they are just doing scripting but in reality
they will create code able to run in any context using the least amount of data transfer between nodes.

in the initial phase the compiler use the v8 engine 
  
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
  
### functors behave like types and categories, also functor are morphisms between categories  
  
when a functor is created in spiral it creates a 
* type (predicated/challenge)
* a category (concept similar to a `class` or `trait`) 
* and a mapping between categories (functor)  
* a wrapper or container that will wrap the codomain of the functor to its own category
  
  
```javascript  
functor dollar(x: number) => x  
/// this will create the category  dollar
/// the type dollar
/// and a mapping from number to dollar
/// a wrapper () which category is dollar and will contain the codomian after is returned by the functor
/// y = dollar(30)
/// y.0 will return 30 the number
///console.log(y) // will print dollar(30)
///console.log(y.0) // will print 30


const bitcoin = (x: number)=> x  
  
const print_dollar(x: dollar) => console.log(`{x.0} $US`)   
/// to avoid print dollar(30) $US we need to use a #forgetful morphism in this case `x.0`
const print_bitcoin(x: bitcoin) => console.log(`{x->number->str} BTC`)
/// will also can archive this using x->number->str, this will force a morhism to number then morph to str
  
dollar(50).print_dollar() /// ok: 50 $USD
dollar(50).print_bitcoin() /// Error: print_bitcoin is not defined for dollar  
50.print_bitcoin()  /// ok:  50 BTC , 50 is a number and have a #auto morphism to bitcoin
let x = 50;
x.print_bitcoin()  /// ok
x.print_dollar() /// Error!!! when x was used on print_bitcoin, print_bitcoin propagate the bitcoin type to x
x.0.print_dollar() /// ok  #forgetful morphism as `member access` will forget where they belong and they can be casted again as dollar

(x-> number).print_dollar() 

/// notice that () is necessary to call the print_* functors, but in the `hi` functor it was not
/// there is a reason why, hi is #pure functor, and the print_* family are #action functor
/// (they have side effects outside the abstract boundary of the program), action functor always need the () 
/// at the end, while #pure functors only need the () at the end when they need to receive more arguments.
  
print_dollar(bitcoin {3}) 
/// Error: print_dollar not defined for bitcoin 
/// and bitcoin has not an #auto morphism to dollar  
  
/// let fix those errros!!  
#iso functor convert(: bitcoin): dolar =>  ::domain * 10'000 
/// the whole domain is accecible using  ::domain path
/// the #iso decorator here will automatically implement the convertion from dolar to bitcoin.    
 
/// or less hacky without ::domain #iso functor convert(currency: bitcoin): dolar =>  currency * 10'000  
/// also  
/// #iso functor convert(currency: bitcoin) =>  dolar(currency * 10'000)  
  
console.log(bitcoin {3} -> usd) // 30'000  
bitcoin(3).print_dollar() // 30'000 $USD  
dollar(90'000).print_bitcoin() // 9 $USD   
  
```  
  
### types are propagated  
  
instead of being inferred, types propagate themselves, is responsibility of the types to check that the code is well defined the compiler just give the basic tools to types to do their job. 

```javascript  
  
const sum = (x, y) => x + y /// Error! the + operator is not defined for any type  
  
/// please make this work!!,  
const sum = (x:number, y) => x + y /// Ok  
/// x will propagate its own type, and the compiler will add the predicated that `y` needs to have /// an  #auto morphism to number and also anotated the codomain  
  
console.log(sum("30"; 80)) /// ok!!: 110 both inputs have an #auto morphism to number so it will works  
  
/// sum("30"; 80) and sum(x: "30", y:80) are #auto morphics so the compiler understand this,  
/// expression separated by comma `,` means that are part of set, and expression separated by semicolon `;` /// form a sequence  
  
```  
  
types are functors, categories are types, types are categories and functor are categories and by the way values also are types, categories and functors  
  
const x = 60, in reality is `functor x() => 60`  
  
  
## deep dive into the spiral   
  
the best documentation right now is the source code.


# spiral end goals  
  
* be able to create video-games with complex magic system that will allow to its player create custom magics,  
 and transfer that knowledge to other players, I am big fan of the overlord novel, I want a video game like this  
   
* compile and simulate chemical reactions  
  
* be able to compile for first time an eukaryotic cell  
  
* check if we live or not in a simulation.
