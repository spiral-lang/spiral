# spiral  
  
`state`: (#design phase) & (#unstable #experimental stage) 
 
`description`:  spiral is a functional language  inspired by the category theory and  oriented to create dynamical system, types are built from the bottom up, using small set of logic  that can be combine together to create the traditional concept of classes.  is built with the objective to reduce the amount of work necessary to produce modern software today, it comes with nice features like auto implementation of tests.

one of the  final objectives of spiral is give to the developer all the tools needed to build any complex system without use other   programming or query languages, `no sql` or `graphql`, spiral is built to handle the most complex type systems, even real time graph engines, you can build anything, from an operative system to  you normal spa application.  also concepts like pointers, asycn/await are abstracted away from from your of day to day coding problems. 

when programming spiral you use `functors or function objects` instead of `functions`.  they are just data and can be manipulated by the programmer, query, transformed, transpiled or compile to whatever they want. the process is very natural and not additional hack is needed in orden to perform those transformation, just understand  `categories`, `boundaries` , `algebra` and `morphism`. 

in theory you work as programmer is not just design those  functors but also compile or transpile  them to functions/programs or full application, based in the specific requirements of your target machine ,  the core library give you the tools to do it in most cases.

```javascript 
//declare a functor
functor x(){}
//to make things simpler also can be declared like this
fn x(){}
```
 the spiral spec is very small due that everything can be expressed with functors.  the syntax is similar to es5/es6

# Boundaries ,  Categories and Identity

`boundaries`, `categories` and `identity`  are concept that emerge in spiral, they have a lot in commons and most of the time you can see them like the same things 

a boundary is a logic structure that follow some rule of construction, this rule of construction give to the boundary its identity, so any mutation that violate this rule automatically will return error, and the only way to get ride of the rule of construction is destroying the boundary and create a new one with other rules.

eventually those concepts diverge, but for the moment to make things simpler, suppose that are just the same
 
##  Creating your first category  and boundary builder 

boundaries constructor are declared using recursive functors  and assertions.  lets create a small currency app, every currency  uses its own boundaries with a set of rules.

```javascript
//start with bitcoin
fn bitcoin(x){
    //make sure that x is  rational number
    rational(number(x));
    //assert the limit to any bitcoin transaction
    assert(x.divisor <= 0.00000001.divisor, `to many digits, its violate the bitcoin minimum transaction`); 
    
    // the magic then happens here, this recursive call will be simulated by the compiler
    // and by induction in most cases will determine that the value in the input is the 
    // same in the output and categorize  `fn bitcoin` as a boundary that forms
    // a dynamical system with constant value, 
    // it also support other types of dynamical system (they will be explained later)
    return bitcoin(x)
}

//let create a bitcoin
let x = bitcoin(30.00) //ok
bitcoin(0.00000000000000000001) // error `to many digits`
x = bitcoin(`asasas`) // error `asasas` can not be transformed into a rational number
x = bitcoin(`543`) // ok

functor currency(x){
	assert(x.name in ["BTC", "USD"]);
	rational(number(x.value));
	if(x.name == 'BTC'){
          assert(x.divisor <= 0.00000001.divisor, 
                `to many digits, its violate the bitcoin minimum transaction`); 
    }
	return currency(x);
}
let c = currency({name: 'BTC', value: 30 })
c.name = 'asasas' // erro name should be in ["BTC", "USD"]

```
the above code have created

 1. the bitcoin category
 2. the builder for bitcoin boundary  ,  a concept similar to the class constructor, in fact boundaries can be mapped to classes
 3. the bitcoin identity, now is possible to use bitcoin in the identity function of the object, example:  `bitcoin/1`, `bitcoin/3`, etc in a database. 
 4.  the set of predicated or rule/type that define which valid state can hold a object inside the bitcoin boundary.

the continuous rule is then applied every time that a mutation is done and will raise and assertion if the mutation  moves the object to an invalid state.

as explained above assertion create types, so the compiler will help you and make sure that you never raise any assertions before compile you code. 

assertion that cannot be eliminated in compilation time will be move to the boundary of the application.   network, os and other io events. they can be propagated  even to the  forms in the front-end, you will be in total control of how those assertion appears to your final user to avoid leak  how your code is implemented.

one nice feature of `assertion/type  propagation` is that you can move them to the boundary of your back-end, (the computer that receive all the inbound communication) and eliminate all the bound check in the machines located in the deepest levels of your cluster at compile time. !!


## adding method to the boundaries

by using assertion the compiler will pickup the concept of `methods` , it only requires that the assertion is done  to the first argument of the functor to be associated with a specific category/boundary. 
```javascript 
fn format(x){
    bitcoin(x); //assert that is a bitcoin
    return `{x} BTC`
}
console.log(bitcoin(90).format()) // 90 BTC
console.log(bitcoin(90).format) // 90 BTC
// also if the functor have not additional arguments and is pure the () at the end is not necessary
```

# Algebras

algebras are a important part of spiral, and it `how most of the optimization happens!!!`. an algebra for category `c` is the set of all the `(binary|unary)(pure(endo(functors, c)))` 

### what do it means `(binary | unary)(pure(endo(functors)))`  ?

lets create those boundaries 

```javascript 
fn endo(f, c){
	//assert that f is a functor
	functor(f);
	if(cat){
	  //assert that c is a category
	  category(c);
	  /// every input should be in the category c 
	  f.domain.map((x)=> assert(x.category == c));
	  // codomian should be in category c
	  assert(f.codomain.category == c)
	  // the last part is most complicated but every functor invocation in the body of f
	  // also should be an endo-functor on c,
	  f.fn_calls.map((call)=> endo(call, c));
	  return endo(f, f.codoman.category);
	}else{
		return endo(f, f.codoman.category);
	}
};
fn bynary(f){
	//assert that f is a functor
	functor(f);
	assert(f.domain.length == 2); 
	return bynary(f)
};

fn unary(f){
	//assert that f is a functor
	functor(f);
	assert(f.domain.length == 1); 
	return unary(f)
};

//`pure` boundary will make sure recursively that the any functor call is also pure,
// this mean that inside the functor no mutation is performed on the domain
// this is more complicate to put in few lines of codes,
// the functor at deepest levels like `set` and `get` 
// need to be marked as pure/impure and the rest can be archived by induction
fn pure(f){
	 functor(f);
	 if(f.pure == true){
	    return pure(f);
	 }else{
	    f.fn_calls.map((call)=> pure(call));
	     return pure(f);
	 }
};

fn impure(f){
   //a match take multiples functors and only one functors should be executed without error
   //if more that one functor is executed it will raise an ambiguity error
    match f {
      (x){ 
        assert(pure(x));
        throw `a functor can not be pure and impure at the same time`;
      },
      (x)=> return impure(x);
    }
};

//the key point of this kind of recursion is that any object that enter to the recursion become stable,
//in both cases (`bitcoin`, `endo`, etc.) are using pure functors, so by induction automatically is 
//recognized as stable. not all the boundaries need to be stables, and is probably be the most
//exciting part of spiral

endo.auto  = true;
bynary.auto  = true;
unary.auto  = true;
pure.auto  = true;
impure.auto  = true;

/** auto categories
* auto categories allow meta-programming, 
* just add the auto property to the functor used to define a  category
* and the compiler will recognized and attach the category to every object that match the rule 
* 
* due that auto-categories are not part of the rules of construction
* an object can make mutation outside the boundary of the auto-category attached to it,
* and lose them on  the process
* this can be exploited in ui interface 
*/ 

fn with_binary_operator(f){
	 binary(pure(endo(functor(f))));
	 assert(f.binary_operator in {`+`, `-`, `*`, `%`, `|`});
	 return with_binary_operator(f);
};
with_binary_operator.auto = true;
//additional properties need to be assert in previous auto(categories) to be used
with_binary_operator.asdweew = `sasas` // error I can`t find an auto category that match this object
with_binary_operator.binary_operator = `-`;  //error 
//`with_binary_operator`  is not a binary((pure(endo(functor))
```

now you have the tools to understand this `(binary | unary)(pure(endo(functors)))`,  the `|` is an algebra between functors called `choice` that  allow horizontally composition of  functors and  the most important operator in the whole spiral concept, most of the complex problem  in programming can be reduced to choices, `matches` and `conditionals` use `choice` 

```javascript

fn choice(x, y){
	functor(f);
	functor(f);
	return (domain)=>{
		//wrap possible assertion into a result object 
		let left = result(x(domain));
		let right = result(y(domain));
		if (left.isOk & right.isOk){
			throw AmbiguityError();
		}else if(left.isOk){
				return left.unwrap();
		}else if(right.isOk){
				return right.unwrap();
		}else{
			throw {left.error(), right.error()}
		}
	}
}
choice.binary_operator = "|"

//the definition of match

fn match({input, fns }){
	return fns.reduce(
	initial:()=>{ throw `nothing matched` },
	(accumulator, currentValue )=>choice(accumulator, currentValue)
	)(input);
}

// if and else statement also are reduced by the choice operator, 

// multiples fn  with the same name defined in the same file also are reduced by choice

fn format(x){
	number(x)
	return `number: x`
}
fn format(x){
	str(x)
	return `str: x`
}

//both functions are internally reduced by choice

const format =  50; 
// is translate to 
fn format(){
	return 50
}
//boom!! some magic happens here choice will complain about that format always return ok(50) 
// and raise and  ambiguity error,  this is easy to spot at compiled time
```

choice implementation looks ugly but assertion are also  boundaries,  that happens to have algebras so most of them will be eliminated in your code.

example `assert(x==3) & assert(x>4)` automatically will raise and ambiguity error at compiled time

example `assert(x>3) & assert(x>100)`, `assert(x>3)` will be eliminated by `assert(x>100)`




### declaring an algebra for bitcoin

```javascript
fn sum(x, y){
	bitcoin(rational(number(x)));
	bitcoin(rational(number(y)));
	return bitcoin(x->number + y)
	//the -> will force a morphism to number where + operator is implemented,
	// morphism are explained later, the whole sum implementation as method of bitcoin 
	// is not really necessary when you understand the category of auto(pure(morphism))
}
sum.binary_operator = `+`;

// to determine that sum is a correct algebra, the compiler will create a test to determine
// in which region of the domain sum is a endo-functor, 
// this can be done by induction most of the time,  


console.log((bitcoin(30) + bitcoin(90)).format) // `120 USD`
```

the compiler will test the  `commutative`, `associative` and `distributive` laws  in a background job,  while you code, it will use those law to automatically optimized your code, most  optimization  happen due to  algebras

there will be an api to implement compiler background jobs in the future that will be mostly used to `auto(test)`. 


### algebras only happen with object in the exact same category

```javascript 
bitcoin(30) + 90 //error impossible to use the binary operator `+`/sum
// between bitcoin an  a natural(number)
```

but this can be overcome with morphism, especially the category of  `pure(auto(morphism))`

## dollar

```javascript
fn dollar(x){
    rational(number(x));
    assert(x.divisor <= 0.001.divisor, `to many digits, its violate the minimum dollar transaction amount`);
    return dollar(x);
};

fn format(x){
    dollar(x); 
    return `{x} USD`
}
fn sum(x, y){
	dollar(rational(number(x)));
	dollar(rational(number(x)));
	return dollar(x->number + y)
}
sum.binary_operator = `+`;

//dollar(30) + bitcoin(30) // error the `+` is not implemented for dollar and bitcoin
```
in fact binary operator only can happen between object in the exact same category , then how can we create bitcoin + dollar?,  the answer lied into morphism.

## morphisms

morphism are transformation of object from category A to category B,  almost everything functor that take an object from A to B  can be considered a morphism.

```javascript
fn bitcoin(x){
   dollar(x); 
   return bitcoin(x->number / 70000 );	
}

fn dollar(x){
   bitcoin(x); 
   return dollar(x->number * 70000 );	
}
// at this point due auto categories the compiler will know that those thing are morphism,
// even better the second function is not really necessary due that division is the inverse of the 
// multiplication, so auto-cats will classify the whole thing as an iso(morphism), 

// and also as auto(morphism), just like auto(cats), auto(morphism) happens automatically, 
// which means that those  transformation will happen in the code without you need to acknowledge them. 
// there is obviously some rules for auto(morphism), 
// most important is purity and data need to be read 
// sequentially without memory allocation, for example algorithms that use things like sort 
// will difficult be part of the category of auto(morphism) 


let x = bitcoin(dollar(70000) + bitcoin(30)) // bitcoin(100)
let y = dollar(0)
y = bitcoin(30) + dollar(1200) + bitcoin(1);// ok because `y` have been already declared as dollar
 
dollar(30) + bitcoin(30) // error? the compiler will complain here 
//there is an ambiguity and it does not know if the result should be in dollar or bitcoin.

bitcoin(dollar(30)) + bitcoin(30) // ok
bitcoin(dollar(30) + bitcoin(30)) // ok
dollar(30).bitcoin + bitcoin(30) // ok

```

```javascript
/// it is also possible to name morphism
fn to_bitcoin(x){
   dollar(x); 
   return bitcoin(x->number / 70000 );	
}

//but now the compiler will complain, an ambiguity have been create
// and now is necessary to disambiguate the code 
let x = bitcoin(dollar(70000).to_bitcoin  + bitcoin(30)) 
//or
let x = bitcoin(dollar(70000).bitcoin  + bitcoin(30)) 
//or
let x = bitcoin(bitcoin(dollar(70000))  + bitcoin(30)) 
```

## the -> operator

the `->` force a morphism,  it looks the shortest path between two categories  , it can be used when `auto(morphism)` does not work, it also can be use two compare two objects in a specific category

> the only rule right now is that `->` can be use in anything, except  forgetful(functor) that are functors that drop part of the structure of the input object, in those case the `.` operator is better.  also is not possible to use -> on morphism  that need more information to be done`

```javascript 
	let x = bitcoin(1);
	let y = dollar(7000);
	assert(x == y) //ok: there is not need to disambiguate, the eq operator does not care
	
	// we can use -> here because the input object is number and is not losing 
	// its structure when is transformed into a rational number
	assert(x -> rational(number) == y) // this will compare y and x in the
	//rational number category which mean it will fail because 1!= 7000 
```


##  Two boundaries in the same category should be isomorphic

there is infinite ways to morph something between categories but the the rule of thumb lied into iso-morphism, which mean that there should be  pure transformation from `b1` to `b2` and from `b2` to `b1` in which not information is loss, if two object are in the exact same category or your plan to use algebras between them

the guarantee correctness on the program logic

```javascript
//map a row in your database to bitcoin
fn bitcoin(r){
	your_database(row(r));
	assert(r.currency == `BTC`)
	rational(number(r.value));
	return bitcoin(r)
}


let remote_bitcoin = bitcoin(your_database(row({currency:`BTC`, value: 300})));
let local_bitcoin = bitcoin(1700);

local_bitcoin + remote_bitcoin// error the local_bitcoin and  remote_bitcoin are in different categories, 

//fix this
fn bitcoin(r){
    //there is not need to put row again, also those functor can be called in any order 
    // bitcoin(your_database(row)) == row(bitcoin(your_database)) == bitcoin(row(your_database))
	bitcoin(your_database(r));
	return bitcoin(rational(r.value)))
}

fn row(x){
	bitcoin(rational(x));
	return bitcoin(row({currency: `BTC`, value: x->number}))
}

local_bitcoin + remote_bitcoin // will return ok bitcoin(2000)
local_bitcoin.currency // will do ok `BTC`

let email  =  `john@sales.yourcompany.com`
email.username // john
email.host // sales.yourcompany.com
email.department // sales , wtf!!, the magic of auto(categories) and auto(morphism)
// if a ambiguity surge in the ambient then you will need to disambiguate
email.yourcompany.department 
```

## not all assertions are necessary

``` javascript 
fn total(x, y, z){
	return x + y + z; 
}
```

the compiler will try to add them automatically, here will assume that  x, y and z  are numbers.  as soon how there is a ambiguity error in the scope  (file, package) etc, for example the binary operator `+` was used in other category, 
the compiler will complain and let you know that you need to make an assertion  `(the power of choice)`
