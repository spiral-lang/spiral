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
/// x->number->str will also remove the bitcoin() wrapper , this will force a morhism to number then morph to str
  
dollar(50).print_dollar() /// ok: 50 $USD
dollar(50).print_bitcoin() /// Error: print_bitcoin is not defined for dollar  
50.print_bitcoin()  /// ok:  50 BTC , 50 is a number and have a #auto morphism to bitcoin
let x = 50;
x.print_bitcoin()  /// ok
x.print_dollar() /// Error!!! when x was used on print_bitcoin, print_bitcoin propagate the bitcoin type to x
x.0.print_dollar() /// ok  #forgetful morphism as `member access` will return a codomain that has forget 
where it belongs, in this case because the codmain is a number it  can be casted again as dollar

(x-> number).print_dollar() /// `->` is another #forgetful functor , it mean force the morphism 

/// notice that () is necessary to call the print_* functors, but in the `hi` functor it was not.
/// there is a reason why, hi is a #pure functor, and the print_* family are #action functor
/// (they have side effects outside the abstract boundary of the program), action functor always need the () 
/// at the end, while #pure functors only need the () at the end when they need to receive more arguments.
  
print_dollar(bitcoin {3}) 
/// Error: print_dollar not defined for bitcoin 
/// and bitcoin has not an #auto morphism to dollar  
  
/// let fix those errros!!  
/// the convert function will support bitcon and will automatically apply the #forgetful functor ->
#iso functor convert(: bitcoin->number): dolar =>  ::domain * 10'000 

/// the whole domain is accecible using  ::domain path
/// the #iso decorator here will automatically implement the convertion from dolar to bitcoin.    
 
/// or less hacky without ::domain = #iso functor convert(currency: bitcoin): dolar =>  currency.0 * 10'000  
/// also  
/// #iso functor convert(currency: bitcoin) =>  dolar(currency.0 * 10'000)  

/// note that  dolar(currency * 10'000)  
/// without .0 will fire Error!!(the operation * is not implemented for bitcoin and number,
/// please morph bitcoin to number using the member access .0 or ->number)
  
console.log(bitcoin {3} -> dollar) // dollar(30'000)  
bitcoin(3).print_dollar() // 30'000 $USD  
dollar(90'000).print_bitcoin() // 9 $USD   

functor euro(x: dollar) => x->number * 1.1

/// a little bit of fun!ctery
console.log(bitcoin {3} -> euro -> decimal) // 27'272.727 
/// console.log(bitcoin {3} -> euro) will print euro(300'000/11) due that  number in decimal form will lose information,
/// and it will not clasify as #iso morphism 

/// why! why! wtf! i have not told the program how to do that. 
/// you did, but there is a little of magic here, the so-called #auto functors or #auto categories or #auto morphism 
/// they all are the different face of the same phenomena, 
/// in this case an the #auto category of  #iso morphism  automatically recognized 
/// this line of code `functor euro(x: dollar) => x->number * 1.1` as an #iso morphism between dollar and euro,
/// due  that the operation `*` has an inverse operator `/` that does not lose information in the category of `#rational numbers`
/// and remember that you specified that there is an #iso morphism between dollar and bitcoin,
/// so the compiler just #inferred the iso morphism between euro and bitcoin 
  
```  
  
### types are propagated  
  
instead of being inferred, types propagate themselves,
is responsibility of the types to check that the code is well defined the compiler just give the basic tools to types to do their job. 

at the moment the basic definition of type is 

```javascript
#auto functor type (predicated, consequence: decorator, error) => {predicated, consequence, error}
// predicated is a check if pass then is assumed that the value is of the type #pure functor (*)=>bool
// consequence is a decorator that will decorated value with new morphismz
// an error that will be fire in case that the check does not pass

```

types are like agents /workers /actors, they want to propagate greedily and cover most of what they can in
spiral graph (a fractal with all the morphism of the program) I like also to call it `dark matter`

types should know how to merge/eliminate each to another

is will be very easy to create basic and high level types but the documentation about this is still `wip`


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
  
types are functors, categories are types, types are categories and functor are categories and by the way values also are types, categories and functors

  
  
const x = 60, in reality is `functor x() => 60`  
let x = 60, in reality is `functor x()=> superposition {initial: 60}`  

a superposition is what other program call versioned data structure
  
## deep dive into the spiral   
  
the best documentation right now is the source code.


# spiral end goals  
  
* be able to create video-games with complex magic system that will allow to its player create custom magics,  
 and transfer that knowledge to other players, I am big fan of the `overlord light novel`, and I want a video game like this  
   
* compile and simulate chemical reactions  
  
* be able to compile and run for first time an fully eukaryotic cell  
  
* check if we live or not in a simulation.
