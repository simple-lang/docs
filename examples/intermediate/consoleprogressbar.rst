=======================
consoleprogressbar.sim
=======================

An example file to demonstrate an interactive command line using the standard output 
stream by showing progress bar base on the percentage

:copyright: 2018-2019, Azeez Adewale
:license: MIT License Copyright (c) 2018 simple
:author: Azeez Adewale <azeezadewale98@gmail.com>
:date: 08 July 2018
:filename: consoleprogressbar.sim



------
Code
------

.. code-block:: simple

    
    call simple.util.Console
    
    PBSTR = "||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||"
    PBWIDTH = 60
    
    block main()
    	display "Enter Your Limit : " read limit
    	@"A Progress That count from 0 to "+limit+" in different styles "
    	printProgress(limit,"=")
    	
    	@""
    	for a = 0 to 200 {
    		printProgress_1((a * 100/200))
    	}
    
    block printProgress(length,indicator)
    	progress = 0
    	while progress < length {
    		barWidth = 100
    		
    		stdout.print("[")
    		pos = (progress * barWidth ) / length
    		for i = 0 to barWidth {
    			if i < pos stdout.print(indicator)
    			elif i == pos stdout.print(">")
    			else stdout.print(" ") end
    		}
    		stdout.print("] "+pos+" %% of "+progress+"\r") 
    		stdout.flush()
    		
    		progress ++
    	}
    
    block printProgress_1(percentage)
    	val = percentage * 100
    	lpad = percentage * PBWIDTH
    	rpad = PBWIDTH - lpad 
    	stdout.vprintf(["\r#{1}%#{2} [#{3}#{4}]", val, lpad, PBSTR, rpad, ""])
    	stdout.flush()


