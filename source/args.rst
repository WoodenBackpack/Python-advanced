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


==============
ArgumentParser
==============

ArgumentParser provides way to handle arguments passed to script

.. code-block:: python

  import argparse

  parser = argparse.ArgumentParser()
  add_argument('-q', help="Run script in quiet mode")
  parser.add_argument('-w', help="Write output to file")
  args = parser.parse_args()
    
.. code-block:: console

  python3 ./script.py --help

  #usage: script.py [-h] [-q Q] [-w W]

  # optional arguments:
  #   -h, --help  show this help message and exit
  #   -q Q        Run script in quiet mode
  #   -w W        Write output to file


* Description and epilog
  
  Lets add some description to help

.. code-block:: python

  praser = argparse.ArgumentParser(
      formatter_class = argparse.RawDescriptionHelpFormatter,
      description="""
      ====================
      Handle script output
      ====================
      """, 
      epilog="*** Use thes script carefully! ***")

  parser.add_argument('-q', help="Run script in quiet mode")
  parser.add_argument('-w', help="Write output to file")
  args = parser.parse_args()
   
.. code-block:: console

  python3 ./script.py --help

  # usage: testPython.py [-h] [-q Q] [-w W]
  # 
  # ====================
  # Handle script output
  # ====================
  # 
  # optional arguments:
  #   -h, --help  show this help message and exit
  #   -q Q        Run script in quiet mode
  #   -w W        Write output to file
  # 
  # *** Use thes script carefully! ***

As you can see we've used a formatter_class, available argparse formatters are:
  * class argparse.RawDescriptionHelpFormatter
  * class argparse.RawTextHelpFormatter
  * class argparse.ArgumentDefaultsHelpFormatter
  * class argparse.MetavarTypeHelpFormatter

* Handling arguments

.. code-block:: python

  parser = argparse.ArgumentParser()
  parser.add_argument('-q', 
    dest="store_bool_value", 
    help="Run script in quiet mode", 
    action="store_true")
  parser.add_argument('-m', 
    dest="store_int", 
    help="Max output length", 
    action="store", 
    type=int)
  parser.add_argument('-lh', 
    dest="store_const", 
    help="Add [$USER] to every log", 
    action="store_const", 
    const="[$USER]")
  parser.add_argument('-w', 
    help="Write output to file", 
    dest="store_true_default", 
    action="store_true", 
    default=False)
  parser.add_argument('--files', 
    nargs="+", 
    help="Add log from another files to output", 
    dest="store_multiple", 
    action="store", 
    default=[])
  parser.add_argument('-u', 
    dest="append", 
    help="Users that can read logs", 
    action="append", 
    default=[])
  parser.add_argument('-j', 
    dest="append_jenkins_user", 
    help="Allow Jenkins user to read logs", 
    action="append_const", 
    default=[], 
    const="juser")
  args = parser.parse_args()
  print(args.store_bool_value)
  print(args.store_int)
  print(args.store_const)
  print(args.store_true_default)
  print(args.store_multiple)
  print(args.append)
  print(args.append_jenkins_user)

.. code-block:: console

  python3.6 ./testPython.py -lh -m 20 -q --files out.txt log.txt -u piotr -u user -u jack -j
  # True
  # 20
  # [$USER]
  # False
  # ['out.txt', 'log.txt']
  # ['piotr', 'user', 'jack']
  # ['juser']

