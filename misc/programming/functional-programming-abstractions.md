% Functional Programming Abstractions

_Specifically in Haskell, with occasional references to Scala._

## Semigroup
Semigroup is a class of types in which two values of a type can be associatively combined in some way so as to produce another value of that type. For example, addition or multiplication can combine two numbers into another number, or concatenation can combine two lists into another list. It is possible for a given type to have more than one valid semigroup instance (for example, numeric types could have additive or multiplicative instances).
Of course, in practise only a single instance can be defined for a given type (in Haskell, alternate applicative instances would need to be defined on a `newtype` wrapper instead).

### Implemented Functions
#### Combination
In Haskell, the infix operator `<>` represents the "combination" function that maps two values into another value of the same type:

	(<>) :: Semigroup a => a -> a -> a

### Semigroup Laws
#### Associativity
Combination must be associative, that is:

	x <> (y <> z) == (x <> y) <> z

(This restriction is why addition and multiplication are semigroup instances for numeric types, but subtraction and division are not.)

## Monoids 
A monoid is a special case of a semigroup where each type instance also has some identity or unit value that can be combined with some other value to return that other value. For example, the identity value for numeric addition would be 0, while for numeric multiplication it would be 1, or for list concatenation it would be the empty list.

### Implemented Functions
#### Combination
Inherited directly from Semigroup. In Haskell the Monoid combination function is called `mappend` and is simply:
	
	mappend :: Monoid a => a -> a -> a
	mappend = (<>)
	
#### Identity
In Haskell the identity value for a given monoidal type is given by `mempty`:

	mempty :: Monoid a => a

### Monoid Laws
#### Associativity
Inherited directly from Semigroup. As above:

	x `mappend` (y `mappend` z) == (x `mappend` y) `mappend` z

#### Existence of an identity value
The identity value must in fact be an identity value, that is:

	x `mappend` mempty == x
	mempty `mappend` x == x

## Functors

A functor is some structure containing values that can be mapped over with a function.

### Implemented Functions
#### Mapping
In Haskell, every functor typeclass implementation defines a function `fmap` to map a function over the functor's value(s):

    fmap :: Functor f => (a -> b) -> f a -> f b

Haskell also has an infix form of this function called `<$>`.

### Functor Laws
#### Identity Mapping
Mapping the identity function over a functor returns the same functor.

    fmap id myfunctor == id myfunctor

#### Composed Function Mapping
Mapping a composition of two functions over a functor should return the same result as sequentially mapping each function over the functor.

    fmap (f . g) myfunctor == fmap f (fmap g myfunctor)

## Applicative

An applicative combines the concepts behind both functors and monoids. In Haskell, the Applicative typeclass is a special case of the Functor typeclass (but not the Monoid typeclass). Like monoids, and unlike functors, more than one applicative instance may be possible for a given type. 

### Implemented Functions

#### Minimal instance creation

Puts a value into a minimal applicative structure. In Haskell the function is called `pure`.

	pure :: Applicative f => a -> f a

#### Application

Similar to functor mapping, but the function being applied is also contained in an applicative, requiring the definition of monoid-like combination rules. In Haskell this operation is represented by the `<*>` infix function.

	(<*>) :: Applicative f => f (a -> b) -> f a -> f b

### Applicative Laws

#### Identity Application
Applying an applicative containing the identity function to some other applicative just returns that other applicative:

	pure id <*> myapplicative == myapplicative

#### Composed Function Application
Applying an applicatively-composed function to some value should return the same result as applying the functions successively.

	pure (.) <*> applf <*> applg <*> applx == applf <*> (applg <*> applx)

#### Homomorphism
Applying a function to a value when both are in minimal applicative structures should yield a new value in an applicative structure that is no different than the result of applying the function to the value independent of any structure.

	pure f <*> pure x == pure (f x)
	
#### Interchange
Applying a function-containing applicative to a value in a minimal applicative structure should give the same result as applying a minimal applicative structure (containing a function that takes a function and applies the original value) to the original function-containing applicative.

	applf <*> pure x == pure ($ x) <*> applf

## Monads

A monad encapsulates one or more values in a specific computational context that can be referenced or propogated through subsequent computations. In Haskell, the Monad typeclass is a special case of the Applicative typeclass.

### Implemented Functions 
#### Bind
Takes some monadic context, extracts the value(s), and applies those values to a function that returns some other value in a monadic context:

    (>>=) :: m x -> (x -> m y) -> m y

In Haskell, `bind` is represented as the infix `>>=` operator, while in Scala it is defined on monadic objects as the method `flatMap`, making more explicit the fact that it can be thought of as `(flatten (fmap f m)`, where `flatten` is some function taking a nested monadic value and reducing it to a single monadic context.

    flatten :: m (m x) -> m x

#### Return
Puts some value in a minimal monadic context. 

    return :: x -> m x

While the function is called `return` in Haskell, in Scala it's named `unit`.

#### Monadic function composition (implicit)
This function (`<=<` in Haskell) doesn't need to be explicity declared as it can be derived directly from bind as:

    (<=<) :: (b -> m c) -> (a -> m b) -> (a -> m c)
    f <=< g = (\x -> g x >>= f)

It's the equivalent of composing functions (`.`) for normal function application, that is 

    f (g x) == (f . g) x

is to function application as 

    x >>= g >>= f == x >>= (f <=< g)

is to monadic function composition.

Defining this here makes monadic associativity (below) easier to represent.

### Monad Laws
Bind and return need to be defined such that each of the following hold:

#### Associativity
While monadic function composition doesn't need to be commutative, it does need to be associative.

    f <=< (g <=< h) == (f <=< g) <=< h

#### Left Identity

Binding a value `x` in a minimal monadic context to a function `f` is the same as directly applying the value to the function.

    return x >>= f == f x

#### Right Identity

Binding the return function to a monadic value just returns the original monadic value.

    m >>= return == m

# Helpful Resources

[Learn You A Haskell](http://learnyouahaskell.com/chapters) is a great introduction to most of this and more. Some of the specific code is a little out of date relative to more recent versions of Haskell but the ideas are all there.

[Haskell Programming from First Principles](http://haskellbook.com/) is an even better resource that builds up these ideas in a very nicely structured sequence. It's a slow burn (monads are past page 700!) and intended to be read sequentially to learn new concepts while reinforcing old ones. Not exactly a concise reference text, but excellent for rigorous self-guided learning.
