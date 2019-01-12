Functions and scripts arguments
*******************************

=====================================
Positional and keywork-only arguments
=====================================

  .. code-block:: python

    def foo(a, b, c):
      print(a, b, c)


  .. code-block:: python
   
    def foo(a, b, c, srt = False):
      if srt : print(a, b, c)
      else: print(sorted((a,b,c)))

    foo(3, 5, 2)             # 3, 5, 2
    foo(3, 5, 2, srt = True) # 2, 3, 5
    foo(3, 5, 2, True)       # 2, 3, 5

  How to avoid passing arguments implicity like in last function call?

  .. code-block:: python 
    
    def foo(a, b, c, *, srt = False):
      if srt : print(a, b, c)
      else: print(sorted((a,b,c)))

    foo(3, 5, 2)             # 3, 5, 2
    foo(3, 5, 2, srt = True) # 2, 3, 5
    foo(3, 5, 2, True)       # error

  The "*" sign as a argument means that after this every argument should be passes explicitly as a named argument, but what if there is no default value and user must to provide that argument?

  .. code-block:: python 
    
    def foo(a, b, c, *, srt):
      if srt : print(a, b, c)
      else: print(sorted((a,b,c)))

    foo(3, 5, 2)              # TypeError: foo() missing 1 required keyword-only argument: 'srt'
    foo(3, 5, 2, srt = True)  # 2, 3, 5
    foo(3, 5, 2, srt = False) # 3, 5, 2
    foo(3, 5, 2, True)        # TypeError: foo() takes 3 positional arguments but 4 were given

=====================
\*args and \*\*kwargs
=====================

  * What if we don't know how many arguments should be passed to our function?

  .. code-block:: python

    def foo(a, b, c, *args):
      print(a, b, c)
      print(" ".join(args))

    foo(1,2,3)                        # 1, 2, 3
    foo(1,2,3, "-b")                  # 1, 2, 3
                                      # -b
    foo(1,2,3, "-b", "-c" , "-quiet") # 1, 2, 3
                                      # -b -c -quiet
  
  * What if we want to user name every argument he will pass

  .. code-block:: python

    def foo(**kwargs):
      print(kwargs)
      for (name, value) in kwargs.items():
        print(name + " = " + str(value))
    
    foo(name = "adam", age = 20)
    # {"name": "adam", "age": 20} kwargs is dictionary
    # name = adam
    # age = 20


  .. code-block:: python

    class Person:
      def __init__(self, name, age):
        self.name = name
        self.age = age
    
    class Suspect(Person):
      def __init__(self, name, age, **additionalInformations):
        super().__init__(name, age)
        [self.__setattr__(name, value) for (name, value) in additionalInformations.items()]
      def __str__(self):
        return "; ".join([(itemName + " = " + str(itemValue)) for (itemName, itemValue) in self.__dict__.items()])
    
    s = Suspect("Adam", 50, eyes="blue", scars="no")
    print(s)
    # name = Adam; age = 50; eyes = blue; scars = no


