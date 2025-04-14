	.......................................................................................................
	#Error by add to Autopilot
 	#Press - Sift+F10
	#Typ - powerchell
 	#then type in terminal:
	Install-Script -name get-autopilotDiagnosticscommunity
	J
	J
	J
	Set-ExecutionPolicy Bypass
	Get-AutopilotDiagnosticsCommunity.ps1

	.......................................................................................................
 	#Zertifikate Problem
	Ich habe unter V:\VERKNUEPFUNGEN\Certs unsere internen Root und Sub CA Zertifikate abgelegt.
 	Die Benutzer können certmgr.msc aufrufen, und dann unter Vertrauenswürdige Stammzertifizierungstellen -> Zertifikate die
  	beiden Dateien mit "root" im Name importieren, und dann unter Zwischenzertifizierungsstellen -> Zertifikate die beiden 
   	Zertifikate mit "sub" im Namen importieren.
	.......................................................................................................


	  #Install Ubuntu
	  wsl --install
	  
	  #Run srcipt as admin
	  runas /user:immo-nrw\$($env:USERNAME -replace '^[^.]+' , 'admin') "C:\Rollen\RoleManager.exe"
	  
	  #Shows all disks (analog for mount in Linux)
	  Get-PSDrive -PSProvider FileSystem
	  \\fs1.immo-nrw.net\Trans$\#VERKNUEPFUNGEN\Rollen\RoleManager.




