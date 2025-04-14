#Install Ubuntu
wsl --install

#Run srcipt as admin
runas /user:immo-nrw\$($env:USERNAME -replace '^[^.]+' , 'admin') "C:\Rollen\RoleManager.exe"

#Shows all disks (analog for mount in Linux)
Get-PSDrive -PSProvider FileSystem
\\fs1.immo-nrw.net\Trans$\#VERKNUEPFUNGEN\Rollen\RoleManager.


