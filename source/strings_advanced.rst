Advanced strings formating
**************************

===============
Representations
===============

  * hex/bin/oct:
      
    .. code-block:: python

      number = 21
      print(f"string represented as a hex: {number:x}")
      print("same but using hex function " + hex(number))
      print(f"string represented as a bit: {number:b}")
      print("same but using bin:  " + bin(number))
      print(f"string represented as a octal: {number:o}")
      print("same but using oct : " + oct(number))

  Otherwise operations:

    .. code-block:: python

      binary = "100100"
      hexadecimal = "FF"
      octadecimal "82"
      print(int(binary, 2))
      print(int(hexadecimal, 16))
      print(int(octadecimal, 8))
  
  * dictionary based formatting

    .. code-block:: python
      
      greeting = """
      Hello %(name)s!
      It\'s good to see You
      Your age is %(age)s:
      """
      guest = {"age": 20, "name": "Jacek"}
      print(greeting % guest)

   * Print class object using vars() method

   .. code-block:: python

     class Person:
       def __init__(self, name, age):
         self.age = age
         self.name = name
         self.surName = "undefined"
     
     a = Person("Jacek", 20)
     greeting = """
     Hey %(name)s
     your age is %(age)s
     """
     
     print(greeting % vars(a))
