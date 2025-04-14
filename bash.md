  #To watch / show function or other in bash 
		type [function]
		#Example ---> type youfunction
		#Show all function decleared in bash ---> typeset -f

	#Run browser vis ssh
		export DISPLAY=:0	
		firefox &

	#Show execute process (trasert mode)
			set -x
		#Turn off
			set +x

	#Get only the file name without path
		basename

		"A && B" Run B if A succeeded
		"A || B" Run B if A failed
		"A &" Run A in background.

	#To know wich command belong to wich packege (watch)
		dpkg -S 'which watch'

	#Return to previous directory/folder ---> cd -

    	#Do 1+1
       		ttt=$((1+1));echo $ttt

    	#Current time/date Plus one hour
       		echo -n $(($(date +'%l')+1));echo -n $(date +' :%m %p')' '; echo -n $(date +'%A %d');

    	#How to check if a string contains a substring in Bash
		string='My long string'
		if [[ $string == *"My long"* ]]; then
  		echo "It's there!"
		fi
    	#or
		string='My long string';if test "$string" = *"My long"*;then echo "It's there!";fi

    	#Check if a File or Directory Exists (https://linuxize.com/post/bash-check-if-file-exists/)
		if [[ -f "$FILE" ]]; then
     			echo "$FILE exist"
 		fi

    	#Replace (http://qaru.site/questions/4981/how-to-use-double-or-single-brackets-parentheses-curly-braces)
		sed -i -e 's|old_text|new_text|g' path_to_file

    	#Replace with echo
		$ var="abcde"; echo ${var/de/12}
		abc12

	#Test redirect
		curl -I http://converterlab.net/

	#qbackground works
	#run script in background ---> nohup command &>/dev/null &

	#watch jobs in background ---> jobs
	#Moving Background Processes to the Foreground ---> fg %number-job
	#Moveng the processes to background ---> CTRL-Z
	#Start pocesses in the foreground ---> bg 
	#Kill background processes ---> watch number (jobs) then ---> kill %number-job 

	#cursor navigation in terminal	
		^A: go to the beginning of line
		^E: go to the end of line
		Alt-B: skip one word backward
		Alt-F: skip one word forward
		Esc-B: skip one word backward (on MacOS)
		Esc-F: skip one word forward (on MacOS)
		^U: delete to the beginning of line
		^K: delete to the end of line
		Alt-D: delete to the end of word
		^J: like Enter
		^H: delete symbol like backspace
		^W: delete word
		^U: delete line
		^\: more powerfull than ^C


	#qlogs all logs here (/etc/rsyslog.conf)
	#log rotation here /etc/logrotate.d/*
		https://www.digitalocean.com/community/tutorials/how-to-manage-logfiles-with-logrotate-on-ubuntu-16-04

	#qtime 	     ---> date
	#change time ---> date MMDDhhmmCCYY.ss 
	(MM - month, hh - hours, mm - minutes, CCYY - year, ss - second)
