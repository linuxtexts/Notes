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

 	#Mount remoute disk
  	net use Z: \\fs1.immo-nrw.net\LCS$ /user:domain\username password
	  
	.......................................................................................................


