% Functional Programming Abstractions

_Specifically in Haskell, with occasional references to Scala._

## Functors

A functor is a data structure that can be mapped over.

### Implemented Functions 
#### `fmap`
In Haskell, every functor typeclass implementation defines a function `fmap` to map a function over the functor's value(s):

    fmap :: f x -> (x -> y) -> f y

### Functor Laws
#### Identity Mapping
Mapping the identity function over a functor returns the same functor.

    fmap id F = id F 

#### Composed Function Mapping
Mapping a composition of two functions over a functor should return the same result as sequentially mapping each function over the functor.

    fmap (f . g) F = fmap f (fmap g F)

## Applicative Functors 
### Implemented Functions 
### Applicative Functor Laws


## Semigroup
Semigroup is a class of types in which two values of a type can be associatively combined in some way so as to produce another value of that type. For example, addition or multiplication can combine two numbers into another number, or concatenation can combine two lists into another list.

### Implemented Functions
#### Combination
In Haskell, the infix operator `<>` represents the "combination" function that maps two values into another value of the same type:

	(<>) :: a -> a -> a

### Semigroup Laws
#### Associativity
Combination must be associative, that is:

	a <> (b <> c) == (a <> b) <> c

This restriction is why addition and multiplication are semigroup instances for numbers but subtraction and division are not.

## Monoids 
A monoid is a special case of a semigroup where each type instance also has some identity or unit value that can be combined with some other value to return that other value. For example, the identity value for numeric addition would be 0, while for numeric multiplication it would be 1, or for list concatenation it would be the empty list.

### Implemented Functions
#### Combination
Inherited directly from Semigroup. In Haskell the Monoid combination function is called `mappend` and is simply:
	
	mappend :: a -> a -> a
	mappend = (<>)
	
#### Identity
In Haskell the identity value for a given monoidal type is given by `empty`:

	mempty :: a

### Monoid Laws
#### Associativity
Inherited directly from Semigroup. As above:

	a `mappend` (b `mappend` c) == (a `mappend` b) `mappend` c

#### Existence of an identity value
The identity value must in fact be an identity value, that is:

	a `mappend` mempty == a
	mempty `mappend` a == a

## Monads 

A monad encapsulates one or more values in a specific computational context that can be referenced or propogated through subsequent computations.

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

It's the equivalent of composing functions for normal function application, that is 

    f . g = a
    f (g x) == a x

is to function application as 

    f <=< g = a
    x >>= g >>= f == x >>= a

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

[Learn You A Haskell](http://learnyouahaskell.com/chapters) is a great introduction to all of this and more. Some of the specific code is a little out of date relative to more recent versions of Haskell but the ideas are all there.
