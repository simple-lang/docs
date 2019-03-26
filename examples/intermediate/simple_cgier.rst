=================
simple_cgier.sim
=================




------
Code
------

.. code-block:: simple
    call simple.io.File
    call simple.util.Console
    import simple.core
    
    block main()
    	header()
    	paramsLen = lengthOf(cmdparams) - 1
    	if paramsLen < 0 { help() }
    	for a = 0 to paramsLen {
    		param = cmdparams[a]
    		if param == "--help" or param == "--h" {
    			help()
    			
    		elif param == "--wamp"
    			superWamper("C:/wamp/")
    			
    		elif param == "--wamp64"
    			superWamper("C:/wamp64/")
    			
    		}
    	}
    	
    block superWamper(file)
    	stdout.print("Configure your wamp server : preparing...")
    	if isWindows() {
    		if __dir_exists(file) == 1 {
    			stdout.printf("%sConfigure your wamp server : checking your httpd.conf", strCopy(" ", 40))
    			file = new Directory("C:/wamp64/bin/apache/")
    			file = file.getDirectories()[0]
    			path = file.AbsolutePath
    			file = new File(path + "/conf/httpd.conf")
    			if file.exists() {
    				stdout.printf("%sConfigure your wamp server : httpd.conf found", strCopy(" ", 40))
    				stdout.printf("%sConfigure your wamp server : backing up your httpd.conf", strCopy(" ", 40))
    				uniqueNum = __rand()
    				content = file.readAllString()
    				__write_file(getTmpDirectory()+"httpd.conf"+uniqueNum, content)
    				stdout.printf("%sConfigure your wamp server : configuring simple for cgi", strCopy(" ", 40))
    				if strSubStr(content, " +ExecCGI") == -1 {
    					stdout.printf("%sConfigure your wamp server : writing +ExecCGI", strCopy(" ", 56))
    					content = replaceStr(content, "+Indexes ", "+Indexes +ExecCGI ")
    				}
    				if strSubStr(content, "AddHandler cgi-script .sim") == -1 {
    					stdout.printf("%sConfigure your wamp server : writing AddHandler", strCopy(" ", 56))
    					content = replaceStr(content, "AddHandler cgi-script .cgi", "AddHandler cgi-script .cgi" + nl + "	AddHandler cgi-script .sim"+nl)
    				}
    				if strSubStr(content, "index.sim") == -1 {
    					stdout.printf("%sConfigure your wamp server : writing DirectoryIndex", strCopy(" ", 56))
    					content = replaceStr(content, "index.php ", "index.sim index.php ")
    				}
    				file.write(content)
    				stdout.printf("%sConfigure your wamp server (done)", strCopy(" ", 56))
    				stdout.println("
httpd.conf backed up at : " + getTmpDirectory()+"httpd.conf"+uniqueNum)
    				stdout.println("restart your wamp server to apply changes")
    			else
    				stderr.printf("%sConfigure your wamp server : httpd.conf not found (failed)", strCopy(" ", 40))
    			}
    			
    		else
    			stderr.printf("%sConfigure your wamp server : Wamp Not installed (failed)", strCopy(" ", 40))
    		}
    	else
    		stderr.printf("%sConfigure your wamp server : Not Windows OS (failed)", strCopy(" ", 40))
    	}
    	
    block header() 
    	header = "simple_cgier.sim 0.0.1 (Febuary 22 2019, 03:33 PM)"
    	stdout.println(header)
    	
    block compareVersions(string versions...)
    	@versions
    	
    block help()
    	helpText = 'simple ./simple_cgier.sim [OPTIONS]
    	
    [OPTIONS] : option passed to the program.
    
    The OPTIONS include: 
     --wamp		configure WAMP Server
     --wamp64	configure WAMP x64 Server
     --xamp		configure XAMP Server
     
     --ask		ask for server path if not detected
    '
    	stdout.println(helpText)
    	
    block getTmpDirectory()
    	tempDirectory = getTempDirectory()
    	tDirectory = new Directory(tempDirectory+"/.simple_env/")
    	if !tDirectory.exists() {
    		tDirectory.create()
    	}
    	return tDirectory.AbsolutePath + SystemSlash()


