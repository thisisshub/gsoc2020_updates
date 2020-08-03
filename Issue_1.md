
These tags encountered in the `TagsCheck.py` are either partially broken or totally broken
**Disclaimer:**
- [ ] Broken
- [x] Fixed 

- Partially broken checks
    - [ ]  non-coherent-filename
    - **Definition:**
    
       *The file which contains the package should be named* `<NAME>-<VERSION>-<RELEASE>.<ARCH>.rpm`     
		
    -  **Where is it found?**
		
        This error is encountered in every package in test/binary/
        
       
     - **How to reproduce this error?**
			   
         - Make a simple specfile and ensure the package is committed to the naming style 
          `<NAME>-<VERSION>-<RELEASE>.<ARCH>.rpm`
			   
         - `rpmbuild -bb <or -bs> package.spec`
			   
         -  run `rpmlint package.rpm` 
      
	-  **Expected Result**:
		  
        - No stdout
	
	-	**Reality**:
			
        - `package.x86_64: W: non-coherent-filename rpmbuild/RPMS/x86_64 package-0-1.x86_64.rpm`   
 
 - [ ]  summary-on-multiple-lines
	 
   - **Definition:**
	    
        Your summary must fit on one line.     
	
		-  **Where is it found?**
    
			This warning should be encountered in packages that have a multi line summary. But you can still not find the warning in the stdout.

     - **How to reproduce this error?**
         - Make a simple spec file with multiple line summary, using vim you can type in insert mode ```Ctrl-v + <Enter Key> or CR``` which would show a highlighted `^M` character (which acts like a enter key).
        			            
         - or try making a `Summary: This is a \n summary` tag line since the code suggests in the `check_summary()` method 
         
          `if '\n' in summary:
              self.output.add_info('E', pkg, 'summary-on-multiple-lines', lang)`
              
         
         - `rpmbuild -bb <or -bs> package.spec`
			   
         - run `rpmlint package.rpm` 
         
   - **Expected Result**:
		
         package.x86_64: W: summary-on-multiple-lines rpmbuild/RPMS/x86_64 package-0-1.x86_64.rpm  
	
	-	**Reality**:
		
          -	No warning in stdout 

 - [ ]  invalid-license
 
	 - **Definition:**

         (definition not available in the  TagsCheck.toml. Below is a user defined definition)
	       The license should be valid license which conforms to the SPDX shortname format
		
    -  **Where is it found?**
			
      This warning should be encountered in packages that have a invalid license. But you can still find the warning in the stdout of every valid license containing package.
	   
     - **How to reproduce this error?**
			   
         - Make a simple spec file with `License: MIT` or any other valid license
			   
         - `rpmbuild -bb <or -bs> package.spec`
			   
         -  run `rpmlint package.rpm` 
   
   - **Expected Result**:
		   
       -   No warning in stdout
	
  -	**Reality**: 
			
	    package.x86_64: W: invalid-license MIT

 - [ ]  tag-not-utf8
	 
   - **Definition:**
	       
         The character encoding of the value of this tag is not UTF-8.
	
		-  **Where is it found?**
			
        This warning should be encountered in packages that have a valid license. But you cannot find the warning in the stdout of the concerned package.
	
	   - **How to reproduce this error?**
			
      - For example in vim, goto insert mode and press `Ctrl-v` and type `0x00` which would make `^@` null sign since, `NUL == 0x00 == 0 == Ctrl + @ == ^@` (more info [here]([https://stackoverflow.com/questions/71323/how-to-replace-a-character-by-a-newline-in-vim](https://stackoverflow.com/questions/71323/how-to-replace-a-character-by-a-newline-in-vim))) in vim. Type this character in any tag value. Hence,  
	
			  Make a simple spec file with say, 
				
        ````
				#1
				Summary: Something <non-utf8-character> warning
	    
			  #2
			  %description
				This is a summary with <non-utf8-character>

			  #3
			  %changelog
			  Someone <non-utf8-character> Something    
        ````  
	  
    - **Expected Result**:

		   - `package.x86_64: E: tag-not-utf8 Summary Something <non-utf8-character> warning ` 

		   - ` package.x86_64: E: tag-not-utf8 description This is a summary with <non-utf8-character>`

		   - `package.x86_64: E: tag-not-utf8 changelog Someone <non-utf8-character> Something`
		   
	-	**Reality**: 
		
		- `/path/to/package.spec does not appear to be a specfile.`
		
		-	No warning in stdout
