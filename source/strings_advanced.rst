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
  
  * dictionary based formatting

    .. code-block:: python
      
      greeting = """
      Hello %(name)s!
      It\'s good to see You
      Your age is %(age)s:
      """
      guest = {"age": 20, "name": "Jacek"}
      print(greeting % guest)

      

    
