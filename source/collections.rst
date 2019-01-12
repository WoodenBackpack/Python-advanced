Collecions
**********

  "This module implements specialized container datatypes providing alternatives to Python’s general purpose built-in containers, dict, list, set, and tuple."

==========
NamedTuple
==========

  .. code-block:: python

    import collections
    Player = collections.namedtuple(typename="Player", field_names=("name", "age"))
    p1 = Player("Adam", 30)
    p2 = Player("Jacek", 20)
    print(p1, p2)


  * namedtuple is immutable

    .. code-block:: python

      # p1.name = "Michal" ERROR, can't set attribute

  * default arguments
  
    .. code-block:: python
   
      # Python 3.7+
      Player = collections.namedtuple("Player", ("name", "age"), defaults=("Unnamed", 0))
      p1 = Player()
      print(p1)

      # Before Python 3.7
      Player = collections.namedtuple("Player", ("name", "age"))
      Player.__new__.__defaults__ = ("Unnamed", 0)
      p1 = Player()

      * other methods and varaibles:
      
      .. code-block:: python
      namedtuple._fields
      namedtuple._fields_defaults
      namedtuple._make(iterable)
      namedtuple._replace(**kwargs)
      namedtuple._asdict()

========
ChainMap
========

    "A ChainMap groups multiple dicts or other mappings together to create a single, updateable view. If no maps are specified, a single empty dictionary is provided so that a new chain always has at least one mapping."
    
    .. code-block:: python

      import collections
      games = {"Serious Sam" : 20, "Diablo II" : 21, "Heroes III" : 2}
      software = {"Photoshop CS6" : 2, "Ableton" : 3}
      boardGames = {"Catan": 12, "Civilization": 3}

      store = collections.ChainMap(games, software, boardGames)
      print(store.get("Diablo II"))


    ChainMap allow us to store items from different sources but can also keep a priority because get(key) or [key] operator return a first value with that key 

    .. code-block:: python

      import collections
      import os
      default = {"valgrind": False, "gen-suppressions" : True}
      envVariables = {"valgrind": True, "quiet" : False}
      userArgs = {"valgrind": True, "quiet" : True}

      binaryName = "main"
      args = collections.ChainMap(userArgs, envVariables, default)
      if (args.get("valgrind")):
        command = "valgrind " + (args.get("quiet") and "-q ") + (args.get("gen-suppresstions") and "--gen-suppressions=all ") + binaryName
      os.system(command)

=======
Counter
=======

    "A Counter is a dict subclass for counting hashable objects. It is a collection where elements are stored as dictionary keys and their counts are stored as dictionary values. Counts are allowed to be any integer value including zero or negative counts. The Counter class is similar to bags or multisets in other languages."

    .. code-block:: python
    
      l = [1,1,1,1,4,5,6,2,3,4]
      st = "aaababbaccababa"
      listC = collections.Counter(l)
      stringC = collections.Counter(st)
      print(listC)
      print(stringC)
   
    get n most common elements

    .. code-block:: python

      l = [1,1,2,2,3,4,5,6,7]
      c = collections.Counter(l)
      print(l.most_common(1))
      print(l.most_common(3))

    as you can see if there are more elements with the same amount elements will be returned in "first-added" order

    .. code-block:: python

      st = "abbac"
      c = collections.Counter(st)
      print(c.most_common(1)) # "(a,2)"


    subtract method changes counter of element or elements by 1:
    
    .. code-block:: python

      st = "abbca"
      c = collections.Counter(st)
      c.subtract("a")
      print(c["a"]) # 1


============
Ordered dict
============

    OrderedDict remembers order that items were interted to dictionary but since Python3.6 reimplementation of dictionaries all dictionaries are ordered and provides better way to store keys than OrdedKeys, anyway only cPython implementation ensure that.

    .. code-block:: python

      c = collections.OrderedDict((x, y) for (x, y) in zip("abcd", range(4)))
      print(c)
      c["b"] = 40
      print(c)
      del c["b"]
      c["b"] = 50
      print(c)


======================
User[String/List/Dict]
======================

    There are also some classes defined that provides wrapers for strings, lists and dictionaries that were used as a interface with some implementation but since it is possible to subclass directrly from dict/list/string those classes were partially supplanted.
