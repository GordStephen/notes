# Scala Concepts and Nomenclature

## Classes

Classes are "static templates that can be instantiated into many independent entities at runtime" [[1](http://scala-exercises.47deg.com/koans#classes)]. They combine data / record structures (internal state) and local methods that can manipulate that state (as well as doing other typical function things).

Declaring a class constructor argument as a `val` automatically defines a getter to externally examine the value (e.g. `println(someClassInstance.someParam)`), while declaring it as a `var` also allows its value to be changed externally via assignment (e.g. `someClassInstance.someParam = "someValue"`). If neither is declared the parameter is not directly accessible outside of the class.

### Extension / Inheritance

A class can extend a single class or any number of traits with the `extends` and `with` keywords.

```scala
class SomeClass extends SomeTrait
class SomeClass2 extends SomeOtherClass with SomeTrait with SomeOtherTrait
```

If multiple traits define the same methods or data, the order in which classes or traits are listed determines which methods are ultimately inherited (the latest-occurring definition is the one which is kept).

## Abstract and Case Classes

Case classes are concrete, serializable class instantiations which automatically make their constructor parameters externally accessible, allowing for pattern matching. Unlike regular classes, they do not require `new` keyword to create.

```scala
class SomeClass(a: Int, b:Int)
abstract class SomeAbstractClass
case class SomeCaseClass1(a: Int, b: Int) extends SomeAbstractClass
case class SomeCaseClass2(a: Int, b: Int) extends SomeAbstractClass

x = new SomeClass(1, 2)
y = SomeCaseClass1(1, 2)
```

## Objects

Scala objects are singletons - they are unique in a given program, and any references to an object by definition refer to the exact same entity.

## Companion Objects

A Scala object with the same name as a class is known as a companion object. It has internal data and/or methods that are shared among all instances of its corresponding class.

## Traits

Traits are abstract, constructor-free, incompletely-defined classes that can provide either abstract or concrete method definitions to be inherited by other classes. Concrete method definitions can reference abstract ones.

Traits can also contain concrete data that can be mixed in to classes. Unlike data in objects, trait data is independent (not shared) in each instantiation that uses the trait.

## Partial Functions

Partial functions (not to be confused with partially _applied_ functions, which relate to currying) are a class representing functions that are only defined for an arbitrary subset of the input domain suggested by the type signature. To fulfill this role, instantiations require the definition of `isDefinedAt` and `apply` methods. Case statements with guards automatically provide such definitions (the guard describes `isDefinedAt` while the result value describes `apply`).

## Extractors and `unapply`

An `unapply` function can be defined on a class that allows for effectively reversing the class constructor by returning the constructor arguments that parametrize the specific class instantiation. This can then be applied (implicitly) during pattern matching. Classes that support this method are called extractors. Case classes provide this functionality automatically.
