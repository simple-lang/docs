
.. index:: 
	single: simple.util.Console

====================
simple.util.Console
====================

The Console module consists functions and constants to interact with the standard streams. 
The stream in this modules are:

* stdin 
  The standard input stream **stdin** is used to accept input from the command line when 
  working in a system environment. In web context such as CGI the **stdin** holds all the 
  content from POST, PUTS Methods with the exception of GET which is appended to the URL

* stdout
  The standard output stream **stdout** is used to display data in the console and act 
  as the document body of a webpage in a web environment.
	
* stderr 
  The standard output stream **stderr** is used to display error data in the console. Data 
  written to the stderr can be redirected to the same display as `final stdout`_.

:copyright: 2018-2019, Azeez Adewale
:license: MIT License Copyright (c) 2018 simple
:author: Azeez Adewale <azeezadewale98@gmail.com>
:date: 5 Febuary 2017
:time: 09 : 29 PM
:filename: Console.sim


================
Table Of Content
================
============================ ========================================================================================================================================================================================================================
 Fields/Blocks/Classes        Summary                                                                                                                                                                                                                
============================ ========================================================================================================================================================================================================================
 `Called Modules`_            import `simple.util.ConsoleColor`_ which hold all the type of color for the foreground and backgroundof the console.                                                                                                   
 `module simple.util`_                                                                                                                                                                                                                               
 `final ConsoleColor`_        initialized the ConsoleColor variable to enable usage of it Color Fields e.g ConsoleColor.Black                                                                                                                        
 `final stdout`_              The standard output stream is the default destination of output for console programs. It is usually directed by default to the text console (generally, on the screen) which can be the terminal.                      
 `final stderr`_              The standard error stream is the default destination for error messages and other diagnostic warnings. Like `final stdout`_ , it is usually also directed by default to the text console (generally, on the screen).   
 `final stdin`_               The standard input stream is the default source of data for applications. It is usually directed by default to the keyboard.                                                                                           
 `block clearConsole()`_      Clear the console or terminal that the input output data is currently interacting with. This block is equivalent to using the **system** function to clear the console, with the platform specific command.            
============================ ========================================================================================================================================================================================================================


.. index:: 
	pair: Console.sim; Called Modules

===============
Called Modules
===============
**Source**: `Called Modules Source`_.
    
    import `simple.util.ConsoleColor`_ which hold all the type of color for the foreground and background
    of the console.


.. index:: 
	pair: Console.sim; module simple.util

===================
module simple.util
===================
**Source**: `module simple.util Source`_.
    
    


.. index:: 
	pair: Console.sim; final ConsoleColor

===================
final ConsoleColor
===================
**Source**: `final ConsoleColor Source`_.
    
    initialized the ConsoleColor variable to enable usage of it Color Fields e.g ConsoleColor.Black


.. index:: 
	pair: Console.sim; final stdout

=============
final stdout
=============
**Source**: `final stdout Source`_.
    
    The standard output stream is the default destination of output for console programs. It is usually directed 
    by default to the text console (generally, on the screen) which can be the terminal. 
    
    Although it is commonly  assumed that the default destination for stdout is going to be the screen, this may 
    not be the case even in regular console systems, since stdout can generally be redirected on most operating 
    systems at the time of invoking the application. For example, many systems, among them DOS/Windows and most 
    UNIX shells, support the following command syntax :
    
    ::
    
      theprogram > output.txt
    
    to redirect the output of **theprogram** to the file **output.txt** instead of the console.
    
    .. note::
      The stdout stream cannot be closed in order to prevent error while trying to write to it later, instead 
      of closing the stream the stdout stream will be flushed instead
    


.. index:: 
	pair: Console.sim; final stderr

=============
final stderr
=============
**Source**: `final stderr Source`_.
    
    The standard error stream is the default destination for error messages and other diagnostic warnings. 
    Like `final stdout`_ , it is usually also directed by default to the text console (generally, on the screen).
    
    Although in many cases both `final stdout`_ and stderr are associated with the same output device 
    (like the console), applications may differentiate between what is sent to `final stdout`_ and what to 
    stderr for the case that one of them is redirected. For example, it is frequent to redirect the regular 
    output of a console program (`final stdout`_) to a file while expecting the error messages to keep appearing 
    in the console.
    
    .. note::
      The stderr stream cannot be closed in order to prevent error while trying to write to it later, 
      instead of closing the stream the stderr stream will be flushed instead


.. index:: 
	pair: Console.sim; final stdin

============
final stdin
============
**Source**: `final stdin Source`_.
    
    The standard input stream is the default source of data for applications. It is usually directed by default to the 
    keyboard.
    
    it is commonly assumed that the source of data for stdin is going to be a keyboard, this may not be the case even 
    in regular console systems, since stdin can generally be redirected on most operating systems at the time of 
    invoking the application. For example, many systems, among them DOS/Windows and most UNIX shells, support the 
    following command syntax :
    
    ::
    
      theprogram < output.txt
    
    to use the content of the file **output.txt** as the primary source of data for **theprogram** instead of the console 
    keyboard.
    
    .. note::
      The stdin stream cannot be closed


.. index:: 
	pair: Console.sim; block clearConsole()

=====================
block clearConsole()
=====================
**Source**: `block clearConsole() Source`_.
    
    Clear the console or terminal that the input output data is currently interacting with. This block is 
    equivalent to using the **system** function to clear the console, with the platform specific command. 
    The following command are issued base on the platform 
    
    * Windows 	system("cls")
    * Others 	system("clear")
    
    This is proven to work in a command line environment but should be avoided in a GUI application as 
    a console or terminal might pop up to execute the **system** command.



-------

.

.. _simple.util.ConsoleColor: ./ConsoleColor.html

.

.. _Called Modules Source: https://github.com/simple-lang/simple/tree/master/modules/simple/util/Console.sim#L29
.. _module simple.util Source: https://github.com/simple-lang/simple/tree/master/modules/simple/util/Console.sim#L40
.. _final ConsoleColor Source: https://github.com/simple-lang/simple/tree/master/modules/simple/util/Console.sim#L46
.. _final stdout Source: https://github.com/simple-lang/simple/tree/master/modules/simple/util/Console.sim#L68
.. _final stderr Source: https://github.com/simple-lang/simple/tree/master/modules/simple/util/Console.sim#L86
.. _final stdin Source: https://github.com/simple-lang/simple/tree/master/modules/simple/util/Console.sim#L109
.. _block clearConsole() Source: https://github.com/simple-lang/simple/tree/master/modules/simple/util/Console.sim#L125

