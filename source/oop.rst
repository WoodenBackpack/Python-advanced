OOP
***

Class definition:

::

  class MyNewClass:
    pass

.. note::

  pass keyword is doing nothing it allows to leave a inplementation without body without it above code will not compile

To create a object of MyNewClass type:

::

  var = MyNewClass

Lets add initializer to our new class

::

  class MyNewClass:
    def __init__(self):
      print("Created MyNewClass instance")
  # then:
  var = MyNewClass()
  >>>Created MyNewClass instance

If you are programming in C++ or Java "self" can be confusing for you. "self" refers to a instance of a class like "this" and it necessary to have access to class members:

::

  class MyNewClass:
    def __init__(self):
      self.x = 20 # it will create class member named x

  # to access x type:
  var = MyNewClass()
  print(var.x)
  >>>20

Lets take a case when we'll declare class member outside the __init__ initializer:

::

  class MyNewClass:
    l = []
    def __init__(self):
      self.t = 5
      self.l.append(2)
      print(self.l)
  
  for x in range(0,5):
    var = MyNewClass()

  >>>[2]
  [2, 2]
  [2, 2, 2]
  [2, 2, 2, 2]
  [2, 2, 2, 2, 2]

So as You can see we've created static variable! It happened because a "l" variable is owned by class itself but "t" is owned by instance, remember each variable, class, instance in Python is a object (derives from "object" class) so there is a "MyNewClass.class" object and every instance is a copy of the class with values belonging to a class and an instance.

There is another confusing Python behaviour: You can't to overload a functions:

::

  class MyNewClass:
    def __init__(self):
      print("empty ctor")
    def __init__(self, arg):
      print("ctor with arg: {arg}")

  var = MyNewClass() # Error 
  var = MyNewClass(123) # object created

It behaves like this because __init__ is overwriten by the last __init__ definition.
In python every variable is the reference to a object so by a mistake you can override a every object and lost way to access it for ever:

::

  class msg:
    def __init__(self):
      self.value = "Msg def value"

  msg = 12
  var = msg() # Oops You got no longer access to msg class! ERROR

Inheritance

::

  class Base:
    def __init__(self):
      self.baseMember = "base member"
      print("Base class initializer")

  class Derived(Base):
    def __init__(self):
      self.derivedMember = "derived member"
      print("Derived class initializer")

Its another python difference from C++/Java-like languages cause lets take a look:

::

  c = Derived()
  >>>Derived class initializer
  # Look Base class initializer wasnt called
  print(c.baseMember) # ERROR
  

So... you have to call base class initializer in Derived class init:

::

  class Derived(Base):
    def __init__(self):
      Base.__init__(self)
      self.derivedMember
      print("Dervied class initializer")

  c = Derived()
  >>>Base class initializer
  Derived class initializer
  
  print(c.baseMember)
  >>>base member  # Now its ok!

So how looks multiple inheritance when we want to call initializer of every parent class:

::

  class A:
    def __init__(self):
      print("A")
  class B:
    def __init__(self):
      print("B")
  class C:
    def __init__(self):
      print("C")

  class D(A, B, C):
    def __init__(self):
      A.__init__(self)
      B.__init__(self)
      C.__init__(self)
      print("D")

  d = D()
  >>>A
  B
  C
  D

 
