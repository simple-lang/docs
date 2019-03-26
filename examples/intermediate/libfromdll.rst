===============
libfromdll.sim
===============

Copyright (c) 2018 Azeez Adewale <azeezadewale98@gmail.com> 
MIT License Copyright (c) 2018 simple-lang


* @Filename - libfromdll.sim
* @Author - Azeez Adewale
* @Date - 07 July 2018
* @Time - 01:22 AM


Generate .lib file from a .dll on windows os using the 
Visual Studio Command Line tools


------
Code
------

.. code-block:: simple
    
    
    call simple.debugging.Throw
    call simple.io.Directory
    call simple.util.Console
    import simple.core
    
    block main()
    	if !isWindows() {
    		display "This script is for the Windows Platform only " + nl
    		__exit__
    	}
    	keepTemp = false
    	defPath = null
    	libPath = null
    	batPath = ""
    	dllFile = ""
    	vcvarsall = ""
    	arch="x86"
    	programFiles = null
    	exportedSymbols = ""
    	if lengthOf(cmdparams) < 2 {
    		help()
    	}	
    	
    	for param in cmdparams {
    		if param == "-h" or param == "--h" or param == "--help" {
    			help()
    			
    		elif param == "-k" or param == "--keep"
    			keepTemp = true
    			
    		elif strStartsWith(param, "-d=") or strStartsWith(param, "--dll=")
    			dllFile = removeStr(param, "-d=")
    			dllFile = removeStr(param, "--dll=")
    			
    		elif strStartsWith(param, "-a=") or strStartsWith(param, "--arch=")
    			arch = removeStr(param, "-a=")
    			arch = removeStr(param, "--arch=")
    			if arch == "x64" { arch = "x86_amd64" }
    			
    		elif strStartsWith(param, "-l=") or strStartsWith(param, "--lib=")
    			libPath = removeStr(param, "-l=")
    			libPath = removeStr(param, "--lib=")
    			if !strEndsWith(libPath, ".lib") {
    				throw("Invalid .lib name, " + libPath)
    			}
    			
    		else
    			#throw("invalid parameter '" + param + "' add --help to view flags")
    		}
    		
    	}
    	if dllFile == "" {
    		throw("No Windows Shared Library (DLL) Supplied")
    		
    	elif !strEndsWith(dllFile, "dll") 
    		throw("Invalid Windows Shared Library (DLL), " + dllFile)
    		
    	}
    	
    	if isNull(batPath) {
    		batPath = strReplace(dllFile, ".dll", ".bat")
    	}
    	if isNull(defPath) {
    		defPath = strReplace(dllFile, ".dll", ".def")
    	}
    	if isNull(libPath) {
    		libPath = strReplace(dllFile, ".dll", ".lib")
    	}
    	if isWindows64() {
    		programFiles = new Directory("C:\Program Files (x86))
    	else
    		programFiles = new Directory("C:\Program Files)
    	}
    	stdout.print("looking for vcvarsall.bat : ...")
    	folders = programFiles.getDirectories()
    	for folder in folders {
    		stdout.printf("%slooking for vcvarsall.bat : " + folder.Name, strCopy(" ", 120))
    		if strContains(folder.AbsolutePath, "Visual Studio") {
    			if __file_exists(folder.AbsolutePath+"\VC\cvarsall.bat") and !strContains(folder.AbsolutePath, "11") {
    				vcvarsall = folder.AbsolutePath+"\VCcvarsall.bat"
    				stdout.printf("%slooking for vcvarsall.bat : " + folder.Name + " (found)
", strCopy(" ", 120))
    				break
    			}
    		}
    	}
    	if vcvarsall == "" {
    		stdout.printf("%slooking for vcvarsall.bat : not found
", strCopy(" ", 120))
    		stdout.println("Cannot find vcvarsall.bat, ensure you have at least one Visual Studio installed along with the C/C++ SDK")
    		__exit__
    	}
    	stdout.print("preparing to export functions : ...")
    	__write_file(batPath, 'call "' + vcvarsall + '" ' + arch + nl + 
    							'dumpbin /EXPORTS "' + dllFile + '" ')
    	system(batPath+' > '+defPath)
    	exportedSymbols = __read_file(defPath)
    	startIndex = strSubStr(exportedSymbols,"ordinal hint RVA      name") + 26
    	exportedSymbols = strRight(exportedSymbols, lengthOf(exportedSymbols) - startIndex + 1)
    	endIndex = strSubStr(exportedSymbols,"  Summary")
    	exportedSymbols = strLeft(exportedSymbols, endIndex)
    	__write_file(defPath, exportedSymbols)
    	exportedSymbols = strSplit(exportedSymbols,nl)
    	finalSymbols = "EXPORTS" + crlf
    	for line in exportedSymbols {
    		if strContains(line,"=") {
    			line = strSplit(line, "=")[0]
    		}
    		exportFunc = strSplit(line, " ")
    		len = lengthOf(exportFunc)
    		if len > 2 {
    			functionV = removeStr(exportFunc[len-1], cr)
    			finalSymbols += functionV + crlf
    			stdout.printf("%sexporting function : " + exportFunc[len-2] + ":" +functionV, strCopy(" ", 120))
    		}
    	}
    	stdout.printf("%sexporting functions : (done)
", strCopy(" ", 120))
    	stdout.print("preparing to generate .lib : ...")
    	if arch == "x86_amd64" { arch = "x64" }
    	__write_file(defPath, finalSymbols)
    	__write_file(batPath, 'call "' + vcvarsall + '" ' + arch + nl + 
    							'lib.exe /def:"' + defPath + '" /OUT:"' + libPath + '" /MACHINE:' + arch)
    	system(batPath + " > nul")
    	if __file_exists(libPath) {
    		stdout.printf("%s.lib generation successful
", strCopy(" ", 120))
    	else
    		stdout.printf("%s.lib generation failed
", strCopy(" ", 120))
    	}
    	
    	if !keepTemp {
    		stdout.print("removing temporary directories : ")
    		system('del "'+defPath+'"')
    		system('del "'+batPath+'"')
    		system('del "'+strReplace(libPath,".lib",".exp")+'"')
    		stdout.print("(done)")
    	}
    	stdout.println()
    	
    	
    block help()
    	help = 
    "usage:  libfromdll [FLAGS]
    
    [FLAGS] : option passed to the program.
    
    The FLAGS include: 
     -h --help			view this help message
     -k --keep			keep the temporary file used for the processes
     -d=<path> --dll=<path>		the dll to export the functions from
     -l=<path> --lib=<path>		the name + path for the generated .lib file
     -a=<arch> --arch=<arch>	specify the lib architecture e.g x86
    "
    	stdout.println(help)
    	__exit__
    	


