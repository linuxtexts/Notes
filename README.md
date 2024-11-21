https://habr.com/ru/articles/347106/
https://habr.com/ru/articles/658879/ - vds


_____ qWindows ________________________________________________________________________________________________

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

______ Telegram Bot ____________________________________________________________________________________________

	import subprocess
	from telegram import Update, InlineKeyboardButton, InlineKeyboardMarkup
	from telegram.ext import Updater, CommandHandler, CallbackQueryHandler, CallbackContext
	
	# Function to execute the bash script
	def execute_bash_script():
	    script_path = "/user/local/admin/new_key.sh"  # Specify the path to your script
	    try:
	        result = subprocess.run([script_path], capture_output=True, text=True)
	        return result.stdout  # Return the output of the script
	    except Exception as e:
	        return f"Error executing script: {str(e)}"
	
	# Handler for the /start command
	def start(update: Update, context: CallbackContext) -> None:
	    user = update.effective_user
	    # Welcome message
	    update.message.reply_text(f'Hello, {user.first_name}! Click the button to get a key.')
	
	    # Create a button
	    keyboard = [[InlineKeyboardButton("Get key", callback_data='get_key')]]
	    reply_markup = InlineKeyboardMarkup(keyboard)
	
	    # Send message with the button
	    update.message.reply_text("Click the button below to request a key:", reply_markup=reply_markup)
	
	# Button click handler
	def button_handler(update: Update, context: CallbackContext) -> None:
	    query = update.callback_query
	    query.answer()
	
	    # Check which button the user clicked
	    if query.data == 'get_key':
	        # Execute the script and get the result
	        script_output = execute_bash_script()
	
	        # Send the result back to the user
	        query.edit_message_text(text=f"Your key: {script_output}")
	
	# Main function
	def main() -> None:
	    # Replace 'YOUR_TOKEN' with your bot's token
	    updater = Updater("YOUR_TOKEN")
	
	    # Register handlers
	    updater.dispatcher.add_handler(CommandHandler("start", start))
	    updater.dispatcher.add_handler(CallbackQueryHandler(button_handler))
	
	    # Start the bot
	    updater.start_polling()
	
	    # Run the bot until manually stopped
	    updater.idle()
	
	if __name__ == '__main__':
	    main()

___qcurl___________________________________________________________________________________

        qcurlftpfs
                yum install curlftpfs -y

        Copy file with Curl
                curl -o do-bots.txt  https://www.digitalocean.com/robots.txt

        #send curl through proxy socks5
                curl --socks5 <proxy_host:proxy_port> <URL>


-------------- script to send text message to telegram -----------------------------

        #!/bin/bash

        # Variables
        BOT_TOKEN="sdfsdfssadfadsfafdadf"   # Your bot token obtained from @BotFather
        CHAT_ID="227909746"       # Your Chat ID
        MESSAGE="Warning Nginx is down"  # Message text

        # Telegram API URL
        URL="https://api.telegram.org/bot$BOT_TOKEN/sendMessage"

        # Sending the message via curl and tor socks5 proxy
        curl -s -X POST --socks5 127.0.0.1:9050 $URL -d chat_id=$CHAT_ID -d text="$MESSAGE"

        # Output result
        if [ $? -eq 0 ]; then
          echo "Message sent successfully"
        else
          echo "Failed to send message"
        fi

-------------------- Check nginx status at remoute server ------------------------------------------------

	#!/bin/bash

	# Specify the IP address of the remote server
	REMOTE_SERVER="121.66.33.11"
	
	# SSH port for connection
	SSH_PORT="22333"
	
	# Path to the log file to check on the remote server
	LOG_FILE="/var/log/check_nginx_script.log"
	
	# Command to check the file on the remote server
	CHECK_COMMAND="grep 'Failed to reload Nginx' $LOG_FILE"
	
	# Infinite loop to run the script every 3 hours
	while true; do
	    # Attempt to connect to the remote server via the specified port and execute the command
	    ssh -q -o ConnectTimeout=10 -o ProxyCommand="nc -X 5 -x 127.0.0.1:9050 %h %p" -i $dedic1_cert -p $SSH_PORT root$REMOTE_SERVER "$CHECK_COMMAND" > /dev/null 2>&1
	    SSH_EXIT_CODE=$?
	
	    # Check if the SSH connection was successful
	    if [ $SSH_EXIT_CODE -eq 0 ]; then
	        # Check if the 'Failed to reload Nginx' line is present in the log file
	        if ssh -q -p $SSH_PORT $REMOTE_SERVER "$CHECK_COMMAND"; then
	            echo "Warning: check nginx at server $REMOTE_SERVER"
	        else
	            echo "Nginx is working correctly on server $REMOTE_SERVER"
	        fi
	    else
	        # Output a message if the SSH connection failed
	        echo "SSH connection failed to server $REMOTE_SERVER"
	    fi
	
	    # Wait for 3 hours (10800 seconds) before running the check again
	    sleep 10800
	done



-------------------- Password encrypt script -------------------------------------------------------------

	#!/bin/bash

	PASSWORD_FILE="passwords.enc"
	PUBLIC_KEY="my_rsa_key.pub"  # Путь к вашему публичному ключу
	PRIVATE_KEY="my_rsa_key"     # Путь к вашему приватному ключу
	
	# Функция для создания файла, если он не существует
	create_encrypted_file() {
	    echo -n "" > "$PASSWORD_FILE"
	}
	
	# Функция для шифрования с использованием публичного ключа
	encrypt_file() {
	    echo "$1" | openssl rsautl -encrypt -inkey "$PUBLIC_KEY" -pubin -out "$PASSWORD_FILE"
	}
	
	# Функция для дешифрования с использованием приватного ключа
	decrypt_to_variable() {
	    local decrypted_content=$(openssl rsautl -decrypt -inkey "$PRIVATE_KEY" -in "$PASSWORD_FILE" 2>/dev/null)
	    echo "$decrypted_content"
	}
	
	# Добавление пароля с описанием
	add_password() {
	    read -p "Enter the name of the account: " account_name
	    read -sp "Enter the password: " password
	    echo ""  # Для перехода на новую строку
	    read -p "Enter a description: " description
	
	    # Дешифруем содержимое и добавляем новый пароль
	    local existing_content=$(decrypt_to_variable)
	    if [ $? -ne 0 ]; then
	        existing_content=""  # Если расшифровка не удалась, начинаем с пустого
	    fi
	    
	    # Формируем новое содержимое
	    local new_content="$existing_content$account_name:$password:$description"$'\n'
	    encrypt_file "$new_content"  # Шифруем новое содержимое
	    echo "Password and description added and file encrypted."
	}
	
	# Извлечение пароля
	get_password() {
	    local account_name="$1"
	    local decrypted_content=$(decrypt_to_variable)
	    
	    local password=$(echo "$decrypted_content" | grep "^$account_name:" | cut -d':' -f2)
	    
	    if [ -n "$password" ]; then
	        echo -n "$password" | pbcopy  # Копирование в буфер обмена для macOS
	        echo "Password for '$account_name' copied to clipboard."
	    else
	        echo "Password for '$account_name' not found."
	    fi
	
	    # Обнуляем переменные для безопасности
	    unset decrypted_content
	    unset password
	}
	
	# Извлечение описания пароля
	get_description() {
	    local account_name="$1"
	    local decrypted_content=$(decrypt_to_variable)
	    
	    local description=$(echo "$decrypted_content" | grep "^$account_name:" | cut -d':' -f3)
	    
	    if [ -n "$description" ]; then
	        echo "Description for '$account_name': $description"
	    else
	        echo "Description for '$account_name' not found."
	    fi
	
	    # Обнуляем переменные для безопасности
	    unset decrypted_content
	    unset description
	}
	
	# Функция для вывода всех учетных записей и описаний (без паролей)
	show_all_except_passwords() {
	    local decrypted_content=$(decrypt_to_variable)
	    
	    if [ -z "$decrypted_content" ]; then
	        echo "No entries found."
	        return
	    fi
	
	    # Выводим только имя учетной записи и описание
	    echo "$decrypted_content" | while IFS=: read -r account_name _ description; do
	        echo "Account: $account_name, Description: $description"
	    done
	
	    # Обнуляем переменные для безопасности
	    unset decrypted_content
	}
	
	# Проверка существования зашифрованного файла
	if [ ! -f "$PASSWORD_FILE" ]; then
	    create_encrypted_file
	fi
	
	# Основной блок
	case "$1" in
	    add)
	        add_password
	        ;;
	    get_pass)
	        if [ -z "$2" ]; then
	            echo "Usage: ./pass_manager.sh get_pass <account_name>"
	            exit 1
	        fi
	        get_password "$2"
	        ;;
	    desc)
	        if [ -z "$2" ]; then
	            echo "Usage: ./pass_manager.sh desc <account_name>"
	            exit 1
	        fi
	        get_description "$2"
	        ;;
	    show_all)
	        show_all_except_passwords
	        ;;
	    *)
	        echo "Usage: ./pass_manager.sh {add|get_pass|desc|show_all}"
	        ;;
	esac



	./pass_manager.sh add
 	./pass_manager.sh get_pass googleaccount
  	./pass_manager.sh desc googleaccount





----------------------Script download und upload files to remout server using ssh--------------------------
	
 	#!/bin/bash

	# Function to display the menu
	show_menu() {
	    echo "1) Upload a file"
	    echo "2) Download a file"
	    echo "3) Exit"
	}
	
	# Function for file upload
	upload_file() {
	    read -p "Enter the path of the file to upload: " local_file
	    read -p "Enter the destination path on the server: " remote_path
	    read -p "Use password or certificate for authentication? (p/c): " auth_method
	    read -p "Enter your SOCKS5 proxy (host:port): " proxy
	
	    # Handle authentication method
	    if [[ "$auth_method" == "p" ]]; then
	        read -sp "Enter your password: " password
	        sshpass -p "$password" rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -o StrictHostKeyChecking=no" "$local_file" "$user@$ip:$remote_path"
	    elif [[ "$auth_method" == "c" ]]; then
	        read -p "Enter the path to your private key: " private_key
	        rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -i $private_key -o StrictHostKeyChecking=no" "$local_file" "$user@$ip:$remote_path"
	    else
	        echo "Invalid authentication method."
	    fi
	}
	
	# Function for file download
	download_file() {
	    read -p "Enter the path of the file to download from the server: " remote_file
	    read -p "Enter the local path to save the file: " local_path
	    read -p "Use password or certificate for authentication? (p/c): " auth_method
	    read -p "Enter your SOCKS5 proxy (host:port): " proxy
	
	    # Handle authentication method
	    if [[ "$auth_method" == "p" ]]; then
	        read -sp "Enter your password: " password
	        sshpass -p "$password" rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -o StrictHostKeyChecking=no" "$user@$ip:$remote_file" "$local_path"
	    elif [[ "$auth_method" == "c" ]]; then
	        read -p "Enter the path to your private key: " private_key
	        rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -i $private_key -o StrictHostKeyChecking=no" "$user@$ip:$remote_file" "$local_path"
	    else
	        echo "Invalid authentication method."
	    fi
	}
	
	# Read server IP and user information
	read -p "Enter the server IP address: " ip
	read -p "Enter your username: " user
	
	# Main loop
	while true; do
	    show_menu
	    read -p "Choose an option: " option
	
	    case $option in
	        1) upload_file ;;
	        2) download_file ;;
	        3) echo "Exiting..."; exit 0 ;;
	        *) echo "Invalid option. Please try again." ;;
	    esac
	done


-------------------------------------------- version 2 -----------------------------------------------------------

	#!/bin/bash

	#Function to display the menu
	show_menu() {
	    echo "1) Upload a file"
	    echo "2) Download a file"
	    echo "3) Exit"
	}
	
	#Function for file upload
	upload_file() {
	    read -p "Enter the path of the file to upload: " local_file
	    read -p "Enter the destination path on the server: " remote_path
	    read -p "Use password or certificate for authentication? (p/c): " auth_method
	    read -p "Enter your SOCKS5 proxy (host:port): " proxy
	
	    # Handle authentication method
	    if [[ "$auth_method" == "p" ]]; then
	        read -sp "Enter your password: " password
	        rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -o StrictHostKeyChecking=no" "$local_file" "$user@$ip:$remote_path"
	    elif [[ "$auth_method" == "c" ]]; then
	        read -p "Enter the path to your private key: " private_key
	        rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -i $private_key -o StrictHostKeyChecking=no" "$local_file" "$user@$ip:$remote_path"
	    else
	        echo "Invalid authentication method."
	    fi
	}
	
	#Function for file download
	download_file() {
	    read -p "Enter the path of the file to download from the server: " remote_file
	    read -p "Enter the local path to save the file: " local_path
	    read -p "Use password or certificate for authentication? (p/c): " auth_method
	    read -p "Enter your SOCKS5 proxy (host:port): " proxy
	
	    # Handle authentication method
	    if [[ "$auth_method" == "p" ]]; then
	        read -sp "Enter your password: " password
	        rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -o StrictHostKeyChecking=no" "$user@$ip:$remote_file" "$local_path"
	    elif [[ "$auth_method" == "c" ]]; then
	        read -p "Enter the path to your private key: " private_key
	        rsync -avz -e "ssh -o ProxyCommand=nc -X 5 -x $proxy %h %p -i $private_key -o StrictHostKeyChecking=no" "$user@$ip:$remote_file" "$local_path"
	    else
	        echo "Invalid authentication method."
	    fi
	}
	
	#Read server IP and user information
	read -p "Enter the server IP address: " ip
	read -p "Enter your username: " user
	
	#Main loop
	while true; do
	    show_menu
	    read -p "Choose an option: " option
	
	    case $option in
	        1) upload_file ;;
	        2) download_file ;;
	        3) echo "Exiting..."; exit 0 ;;
	        *) echo "Invalid option. Please try again." ;;
	    esac
	done









----------------------alias--------------------------------------------------------------------------------

	netstat -tn 2>/dev/null | awk '$4 ~ /:443$/ && $6 == "ESTABLISHED" {print $5}' | cut -d':' -f1 | sort | uniq -c | sort -nr
 	

---------------------Certbot autoupdate certificates-------------------------------------------------------

	#!/bin/bash
	
	# Path to the log file
	LOG_FILE="/var/log/certbot_renewal.log"
	
	# Current date for logging
	current_date=$(date '+%Y-%m-%d %H:%M:%S')
	
	# Log the start of the script
	echo "[$current_date] Starting certificate check" >> $LOG_FILE
	
	# Flag indicating if renewal is needed
	renew_needed=false
	
	# Get a list of all certificates and their expiration dates
	cert_info=$(certbot certificates)
	
	# Check each certificate's expiration date
	while read -r line; do
	    if [[ $line == "Certificate Name:"* ]]; then
	        # Save the domain name
	        domain=$(echo $line | awk '{print $3}')
	    elif [[ $line == "Expiry Date:"* ]]; then
	        # Get the expiration date and days left until expiration
	        expiry_date=$(echo $line | awk '{print $3 " " $4 " " $5}')
	        days_left=$(echo $line | grep -oP '(?<=\().*(?= days)')
	
	        # Check if the certificate expires in less than 30 days
	        if [[ $days_left -lt 30 ]]; then
	            echo "[$current_date] Certificate for domain $domain expires in $days_left days. Attempting renewal..." >> $LOG_FILE
	            renew_needed=true
	        fi
	    fi
	done <<< "$cert_info"
	
	# If at least one certificate expires in less than 30 days, run renewal
	if [ "$renew_needed" = true ]; then
	    certbot renew >> $LOG_FILE 2>&1
	    renew_status=$?
	
	    if [ $renew_status -eq 0 ]; then
	        echo "[$current_date] Certificates successfully renewed." >> $LOG_FILE
	    else
	        echo "[$current_date] Error renewing certificates." >> $LOG_FILE
	    fi
	else
	    echo "[$current_date] All certificates are valid for more than 30 days." >> $LOG_FILE
	fi
	
	# Log the end of the script
	echo "[$current_date] Certificate check completed" >> $LOG_FILE


 	crontab -e
  	0 0 * * * /path/to/check_and_renew_certs.sh



#Check certbot certificates script__one time per Day________________________________________________________________________
	
	# Log file
	LOG_FILE="/var/log/certbot_check.log"
	
	# Current date
	DATE=$(date +"%Y-%m-%d %H:%M:%S")
	
	# Get the list of certificates and their expiration dates
	CERTS=$(certbot certificates 2>/dev/null)
	
	# Set the threshold to 30 days
	LIMIT=30
	
	# Flag for updating certificates
	UPDATE_NEEDED=false
	
	# Process each certificate
	echo "----- Checking certificates: $DATE -----" >> "$LOG_FILE"
	
	while read -r line; do
	    # Find lines containing "Expiry Date"
	    if [[ $line == *"Expiry Date:"* ]]; then
	        DOMAIN=$(echo "$line" | sed -n 's/.*Domains: \(.*\)/\1/p')
	        EXPIRY_DATE=$(echo "$line" | sed -n 's/.*Expiry Date: \(.*\)/\1/p')
	
	        # Convert the certificate's expiration date to UNIX timestamp
	        EXPIRY_TIMESTAMP=$(date -d "$EXPIRY_DATE" +%s)
	        CURRENT_TIMESTAMP=$(date +%s)
	
	        # Calculate the number of days left until the certificate expires
	        DAYS_LEFT=$(( (EXPIRY_TIMESTAMP - CURRENT_TIMESTAMP) / 86400 ))
	
	        # Log certificate information
	        echo "Certificate for domain: $DOMAIN, Days left: $DAYS_LEFT" >> "$LOG_FILE"
	
	        # Check if the certificate expires in less than 30 days
	        if [[ $DAYS_LEFT -le $LIMIT ]]; then
	            echo "Certificate for domain $DOMAIN needs renewal (days left: $DAYS_LEFT)." >> "$LOG_FILE"
	            UPDATE_NEEDED=true
	        fi
	    fi
	done <<< "$CERTS"
	
	# Check if certificates need to be updated
	if [[ $UPDATE_NEEDED = true ]]; then
	    echo "Certificate renewal is required." >> "$LOG_FILE"
	    /path/to/update_certificates.sh
	else
	    echo "All certificates are valid." >> "$LOG_FILE"
	fi
	
	echo "----- End of check -----" >> "$LOG_FILE"

	crontab -e
	0 0 * * * /path/to/cert_check.sh

______________________________________________________________________________________________________________________
#!/bin/bash

	# Проверка статуса Nginx
	if ! systemctl is-active --quiet nginx; then
	    echo "Nginx не работает. Перезагрузка..."
	    systemctl restart nginx
	    if systemctl is-active --quiet nginx; then
	        echo "Nginx успешно перезагружен."
	    else
	        echo "Не удалось перезагрузить Nginx."
	    fi
	else
	    echo "Nginx работает нормально."
	fi
	
	
	crontab -e
	0 * * * * /path/to/your/script.sh

___qsc___________________________Spreadsheet Calculator_____________________________________________________________

        https://github.com/n-t-roff/sc
        https://www.maketecheasier.com/linux-command-line-spreadsheets/
        https://www.linuxjournal.com/article/10699
        http://n-t-roff.github.io/sc.1.html

        git clone https://github.com/n-t-roff/sc.git
        cd sc && ./configure && make && make install

        #help - ?
        #save file      - P
        #edit num cell  - e
        #set format cell - f
        #set color      - ^TC
        #mark cell      - ma or ms or md ...
        #pase           - ca or cs or cd ...

        gB13 — go to cell B13.
        ir, ic — insert row, insert column.
        ma (mb, mc and so on) — “mark” cell as a (or b, or c and so on).
        ca (cb, cc and so on) — copy contents previously marked with ma.
        Ctrl-f, Ctrl-b — page up or down (also pgup, pgdown).
        dr, yr, pr — delete row, yank row, put row.
        dc, yc, pc — delete column, yank, put column.
        dd, yd, pd — delete, yank, put a cell.
        = — enter a numeric value (25 or F13-D14) or formula (@sum(A2:A145)).
        < — insert left-justified text.
        \ — insert centered text.
        > — insert right-justified text.
        x — remove cell.
        W<filename.asc> — write plain-text file.
        P<filename.sc> — write an .sc file.
        G<filename.sc> — read (“get”) an .sc file.
        Zr, Zc — zap (hide) row or column.



___qsyncthing___________________________________________________________________________________________________

	brew install syncthing
	http://127.0.0.1:8384/
	#Action -> Settings -> Connection -> disable "Enable Relaying"
 
___qiTerm_______________________________________________________________________________________________________

	Preferences -> Advanced -> search "restor" and turnoff all
	split pane vertical ---> cmd+d
	split pane horisontal ---> Shift+cmd+d
	kill, unsplit pane ---> cmd+w
	list pane ---> cmd+[ and cmd+]
	Bind Key -> Preferences -> Key -> Plus (+) -> Run Coprocess -> Run Commane and Keyboard Shortcuts

___qmpv_________________________________________________________________________________________________________

	brew uninstall ffmpeg
	brew install mpv
	#record video from youtube with mpv ---> mpv --record-file=video.mkv https://www.youtube.com/watch?v=…
	#play video firm youtube with mpv ---> mpv --screenshot-directory=~/Documents/save/1/img/ --autofit=40% --ytdl-format=22 --ontop https://www.youtube.com/watch?v=…
	#install mpv on centos ---> https://snapcraft.io/install/mpv/centos
	#install mpv-video-cutter   

	................................................. ~/.config/mpv/mpv.conf................................
 
	screenshot-directory="~/Documents/save/img"
	screenshot-format=png
	screenshot-template="%F:%P"
	volume=0
	save-position-on-quit
	profile=pseudo-gui
	........................................................................................................
	#set to ~/.config/mpv/input.conf instead LEFT and RIGHT
		l no-osd seek  1 exact
		h no-osd seek  -1 exact

___Terminal_____________________________________________________________________________________________________

	Preferencec -> Window -> Restore text when reopening windows (Turn it off)

        e — edit a numeric value.
        E — edit a string value.

___qcron_____________________________________________________________________________________________________

	#make files
        touch /root/check_nginx.sh && touch /root/check_and_renew_certs.sh && touch /var/log/check_nginx_script.log && touch /var/log/check_and_renew_certs.log
        chmod 777 /root/check_nginx.sh && chmod 777 /root/check_and_renew_certs.sh
	
 	0 * * * * /root/check_nginx.sh >> /var/log/check_nginx_script.log
        0 0 * * * /root/check_and_renew_certs.sh >> /var/log/check_and_renew_certs.log
 	________________________________________________________________
        #corntab scripts ---> check_nginx.sh
        ________________________________________________________________
        #!/bin/bash
        # Check Nginx
        if ! systemctl is-active --quiet nginx; then
            echo "Nginx down. Restarting nginx..."
            systemctl restart nginx
            if systemctl is-active --quiet nginx; then
                echo "Nginx successfully restarted."
            else
                echo "Failed to reload Nginx."
            fi
        else
            echo "Nginx is working fine."
        fi


        ________________________________________________________________
        #crontab scripts ---> check_and_renew_certs.sh
        ________________________________________________________________
        #!/bin/bash
        # Function to check the number of days until the certificate expires
        check_certificate() {
            domain=$1
            expiry_date=$(sudo certbot certificates --domain "$domain" 2>/dev/null | grep "Expiry Date:" | awk '{print $3, $4, $5}')
            expiry_epoch=$(date -d "$expiry_date" +%s)
            current_epoch=$(date +%s)
            days_left=$(( (expiry_epoch - current_epoch) / 86400 ))

            echo "Domen: $domain, days until expiration: $days_left"

            if [ "$days_left" -lt 30 ]; then
                echo "Certificat for domain $domain expires in more than 30 days. Starting the update..."
                sudo certbot renew --cert-name "$domain"
            else
                echo "The certificate for the domain $domain expires in more than 30 days."
            fi
        }

        # We get a list of all domains for which there are certificates
        domains=$(sudo certbot certificates | grep "Domains:" | awk '{print $2}')

        # We check every domain
        for domain in $domains; do
            check_certificate "$domain"
        done
___qIPTABLES_________________________________________________________________________________________________

	https://notessysadmin.com/minimum-rules-for-iptables
	https://toster.ru/q/14066
	http://farmal.in/2011/07/borba-s-ddos-atakami-sredstvami-iptables-i-sysctl/

	#show all rules!!!
                iptables -n -L -v --line-numbers

        #redirect (forwarding) port form 80 to 8080 on local PC
                iptables -t nat -A OUTPUT -o lo -p tcp --dport 80 -j REDIRECT --to-port 8080

	#INSTALL IPTABLES at NEW SERVER
		1. Install IPTABLES
			systemctl disable firewalld
			systemctl mask firewalld
			yum install iptables-services -y
			systemctl enable iptables
			systemctl enable ip6tables

	........................................................................ /etc/sysconfig/iptables

	*filter
	:INPUT ACCEPT [0:0]
	:FORWARD ACCEPT [0:0]
	:OUTPUT ACCEPT [214:43782]
	-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
	-A INPUT -i lo -j ACCEPT
	-A INPUT -j REJECT --reject-with icmp-port-unreachable
	COMMIT

	........................................................................ /etc/sysconfig/ip6tables

	*filter
	:INPUT ACCEPT [0:0]
	:FORWARD ACCEPT [0:0]
	:OUTPUT ACCEPT [214:43782]
	-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
	-A INPUT -p tcp -m tcp --dport 22 -j ACCEPT
	-A INPUT -i lo -j ACCEPT
	-A INPUT -j REJECT --reject-with icmp6-adm-prohibited
	COMMIT

	............................................................................................. END

		2. Save rules (it save rules to /etc/sysconfig/iptables) ---> service iptables save
		3. Restart ---> service iptables restart

	#Save iptable rules to file       ---> iptables-save > /etc/iptables.rules (">" resave to the file, ">>" save to the end of the file)
	#Restore iptables rules from file ---> iptables-restore < /etc/iptables.rules
	#show all rules and usage traffic ---> iptables -L or iptables -n -L -v --line-numbers

	#DELETE RULE    ---> iptables -D INPUT 4
	#close port 25  ---> iptables -A INPUT -p tcp --destination-port 25 -j DROP
	#open port 25   ---> iptables -A INPUT -p tcp --destination-port 25 -j ACCEPT
	#BLOCK IP       ---> iptables -I INPUT -s 115.214.193.224 -j DROP

	#Drop traffic STATISTIC to zerio ---> iptables -Z INPUT
	#Disable ip6 ipv6 service ip6tables stopchkconfig ip6tables off -->/etc/sysconfig/iptables systemctl stop firewalld
	# Default iptable rules
		iptables -P INPUT DROP
		iptables -P FORWARD DROP
		iptables -P OUTPUT ACCEPT

	# Allow input connection from localhost       ---> iptables -A INPUT -i lo -j ACCEPT
	# Allow already installed inbound connections ---> iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

	# HTTP port ---> iptables -A INPUT -p TCP --dport 80 -j ACCEPT

	# Show IPtables REAL TIME ---> watch -n 2 -d iptables -nv20L

	# Set limit max connection from one IP adress (max 100 connection from IP)
		iptables -t filter -I INPUT -p tcp --syn --dport 80 -m connlimit --connlimit-above 100 --connlimit-mask 32 -j DROP
		(https://unix.stackexchange.com/questions/139285/limit-max-connections-per-ip-address-and-new-connections-per-second-with-iptable)

___qDDOS______________________________________________________________________________________________________________________

	http://mycrimea.su/partners/web/access/ 

	#Number of connection to 80 port ---> netstat -na | grep :80 | wc -l
	#The same in the SYN status 	 ---> netstat -na | grep :80 | grep syn
	#Show online 			 ---> netstat -na | grep :80 | grep EST | wc -l

	#Popular User-Agents
		cat /var/www/admin/data/logs/sitename.net.access.log | awk -F\" '{print $6}' | sort | uniq -c | sort -n
		netstat -ntu | awk ' $5 ~ /^[0-9]/ {print $5}' | cut -d: -f1 | sort | uniq -c | sort -n
		netstat -nt | awk -F":" '{print $2}' | sort | uniq -c | sort -n (show with port)

		alalis last 500 string of log file -->tail -n 500 /var/www/admin/data/logs/sitename.net.access.log | cut -d' ' -f1 | sort | uniq -c | sort -gr

		tail -n 2000000 /var/www/admin/data/logs/sitename.net.access.log | cut -d' ' -f1 | sort | uniq -c | sort -n
					
		___qlogtop____________________________________________________________________________________________________

		#What does logtop shows?
		#line1 - Just number
		#line2 - amount of query from ip
		#line3 - amount of query from ip in one second
		#line4 - ip adress
					
		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $1; fflush();'} | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | cut -d ' ' -f 1 | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $7; fflush();'} | logtop

		---------------------------------------------------------------------------------------------------------------

		awk '{print $4}' /var/www/httpd-logs/text.access.log | cut -d: -f1 | uniq -c

		#Show refferer to site (use acess log file)
			grep "200 " /home/admin/httpd-logs/site.com.access.log | cut -d '"' -f 4 | sort | uniq -c | sort -rn | grep -v "site.com" | less

	
	#Pratektion from flud by nginx

	#In http section
	
	map $http_user_agent $bad_useragent {                                                                                                                    
        default 0;                                                                                                                                               
        ~*ia_archiver 1;                                                                                                                                         
        ~*Curl 1;                                                                                                                                                
        ~*libwww 1;                                                                                                                                              
        ~*BLEXBot 1;                                                                                                                                             
        ~*SBooksNet 1;                                                                                                                                           
        ~*MJ12bot 1;                                                                                                                                             
        ~*Java 1;                                                                                                                                                
        ~*NTENTbot 1;                                                                                                                                            
        ~*GetIntent 1;                                                                                                                                           
        ~*SemrushBot 1;                                                                                                                                          
        ~*HybridBot 1;                                                                                                                                           
        ~*AhrefsBot 1;                                                                                                                                           
        ~*SeznamBot 1;                                                                                                                                           
        ~*DeuSu 1;                                                                                                                                               
        ~*GrapeshotCrawler 1;                                                                                                                                    
        ~*SentiBot 1;                                                                                                                                            
        ~*default 1;                                                                                                                                             
        ~*Virusdie 1;                                                                                                                                            
        ~*WordPress 1;                                                                                                                                           
        ~*WhatsApp 1;                                                                                                                                            
        ~*SeopultContentAnalyzer 1;                                                                                                                              
        ~*WinHTTP 1;                                                                                                                                             
        ~*MauiBot 1;                                                                                                                                             
        ~*weborama 1;                                                                                                                                            
        ~*Python 1;                                                                                                                                              
        ~*Go-http-client 1;                                                                                                                                      
        ~*VelenPublicWebCrawler 1;                                                                                                                               
        }
      	                                                                                                                                                         
      	#in server section                                                                                                                                       
      	                                                                                                                                                         
      	if ($bad_useragent) {                                                                                                                                    
      		return 444;                                                                                                                                              
      	}

  ___qDDOS______________________________________________________________________________________________________________

	http://mycrimea.su/partners/web/access/ 

	#Number of connection to 80 port ---> netstat -na | grep :80 | wc -l
	#The same in the SYN status 	 ---> netstat -na | grep :80 | grep syn
	#Show online 			 ---> netstat -na | grep :80 | grep EST | wc -l

	#Popular User-Agents
		cat /var/www/admin/data/logs/sitename.net.access.log | awk -F\" '{print $6}' | sort | uniq -c | sort -n
		netstat -ntu | awk ' $5 ~ /^[0-9]/ {print $5}' | cut -d: -f1 | sort | uniq -c | sort -n
		netstat -nt | awk -F":" '{print $2}' | sort | uniq -c | sort -n (show with port)

		alalis last 500 string of log file -->tail -n 500 /var/www/admin/data/logs/sitename.net.access.log | cut -d' ' -f1 | sort | uniq -c | sort -gr

		tail -n 2000000 /var/www/admin/data/logs/sitename.net.access.log | cut -d' ' -f1 | sort | uniq -c | sort -n
					
		___qlogtop_____________________________________________________________________________________________

		#What does logtop shows?
		#line1 - Just number
		#line2 - amount of query from ip
		#line3 - amount of query from ip in one second
		#line4 - ip adress
					
		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $1; fflush();'} | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | cut -d ' ' -f 1 | logtop

		tail -f /var/www/admin/data/logs/sitename.net.access.log | awk {'print $7; fflush();'} | logtop

		--------------------------------------------------------------------------------------------------------

		awk '{print $4}' /var/www/httpd-logs/text.access.log | cut -d: -f1 | uniq -c

		#Show refferer to site (use acess log file)
			grep "200 " /home/admin/httpd-logs/site.com.access.log | cut -d '"' -f 4 | sort | uniq -c | sort -rn | grep -v "site.com" | less

	
	#Pratektion from flud by nginx

	#In http section
	
	map $http_user_agent $bad_useragent {                                                                                                                    
        default 0;                                                                                                                                               
        ~*ia_archiver 1;                                                                                                                                         
        ~*Curl 1;                                                                                                                                                
        ~*libwww 1;                                                                                                                                              
        ~*BLEXBot 1;                                                                                                                                             
        ~*SBooksNet 1;                                                                                                                                           
        ~*MJ12bot 1;                                                                                                                                             
        ~*Java 1;                                                                                                                                                
        ~*NTENTbot 1;                                                                                                                                            
        ~*GetIntent 1;                                                                                                                                           
        ~*SemrushBot 1;                                                                                                                                          
        ~*HybridBot 1;                                                                                                                                           
        ~*AhrefsBot 1;                                                                                                                                           
        ~*SeznamBot 1;                                                                                                                                           
        ~*DeuSu 1;                                                                                                                                               
        ~*GrapeshotCrawler 1;                                                                                                                                    
        ~*SentiBot 1;                                                                                                                                            
        ~*default 1;                                                                                                                                             
        ~*Virusdie 1;                                                                                                                                            
        ~*WordPress 1;                                                                                                                                           
        ~*WhatsApp 1;                                                                                                                                            
        ~*SeopultContentAnalyzer 1;                                                                                                                              
        ~*WinHTTP 1;                                                                                                                                             
        ~*MauiBot 1;                                                                                                                                             
        ~*weborama 1;                                                                                                                                            
        ~*Python 1;                                                                                                                                              
        ~*Go-http-client 1;                                                                                                                                      
        ~*VelenPublicWebCrawler 1;                                                                                                                               
        }
      	                                                                                                                                                         
      	#in server section                                                                                                                                       
      	                                                                                                                                                         
      	if ($bad_useragent) {                                                                                                                                    
      		return 444;                                                                                                                                              
      	}

___qTEST SERVER__________________________________________________________________________________________________________

	#TEST(iftop) loading HDD realtime ("yum install sysstat") (https://pingvinoff.net/2009/01/06/komanda-iostat/)
                iostat -xmd 1
                iostat 5 3
		#on mac os use ---> iostat -w1 disk0

		# rrqm/s 	обобщенное количество запросов на чтение в секунду;
		# wrqm/s 	обобщенное количество запросов на запись в секунду;
		# r/s 	количество запросов на чтение в секунду;
		# w/s 	количество запросов на запись в секунду;
		# rMB/s 	количество МБ при чтении с диска в секунду;
		# wMB/s 	количество МБ при записи на диск в секунду;
		# avgrq-sz 	средний размер (в секторах) запросов к диску;
		# avgqu-sz 	средний размер очереди запросов к диску;
		# await 	среднее время (милисекунды) на обработку запросов к диску (включает в себя время, потраченное в очереди на обработку и время на обработку запроса);
		# r_await 	среднее время (милисекунды) на обработку запросов чтения к диску (включает в себя время, потраченное в очереди на обработку и время на обработку запроса);
		# w_await 	среднее время (милисекунды) на обработку запросов запи ик диску (включает в себя время, потраченное в очереди на обработку и время на обработку запроса);
		# svctm 	среднее время (милисекунды) I/O запросов (не образайте внимания на неё — будет удалена в ближайшем будущем);
		# %util 	% CPU, затраченный на передачу I/O запросов к диску («пропускная способность» диска);

		# %util if it 90-100% hdd speed not enough

	#REAL TIME Bandwich Usages (http://palexa.pp.ua/blog/linux/setevye-resheniya/utilita-iftop-kontrol-trafika-v-rezhime-realnogo-vremeni-online.html)
                iftop -i [eth interface]
                iftop -i eth0 -f "dst port domain" (DNS traffic)
                iftop -i eth0 -f "icmp" (ICMP traff)
                iftop -i eth0 -f "dst port http" (HTTP traff)
                iftop -i eth0 -f "dst port 22" (Traff going to 22 port
                iftop -i eth0 -f "dst 8.8.8.8" (Traff going to ip 8.8.8.8)

	#Test RAID 	---> cat /proc/mdstat
	#Test speed bandwidth traff speed ---> speedtest-cli-master
	#Test free space ---> df -h
	#Show CPU temperature ---> sensors
	#Temperature HDD     ---> hddtemp /dev/sda


___qhistory_________________________________________________________________________________________________________________

	#Show history ---> history
	#Search in history ---> history | grep word
	#Reply command number 3 ---> !number (!3)


___qps_________________________________________________________

	#Show all process with comfortable view
		ps -ejH
	#Show all process
		ps -elf

___qnetstat____SS analog netstat_______________________________

	#Show all connection
                netstat -tunup
        #Show all IP adress connection at 443 port
                netstat -tn 2>/dev/null | awk '$4 ~ /:443$/ && $6 == "ESTABLISHED" {print $5}' | cut -d':' -f1 | sort | uniq -c | sort -n
                #you can limitet max connection from one IP address with IPTABLES (example 20)
                        iptables -t filter -I INPUT -p tcp --syn --dport 443 -m connlimit --connlimit-above 20 --connlimit-mask 32 -j DROP

        #Show all rules
                iptables -n -L -v --line-numbers
        #Show what process on wich port works
                netstat -tulpan | grep -i 'listen'
        #Show a wich porgram listen a port 25
                lsof -i :25

___qss_________________________________________________________

	#Show all connection
		ss -tunup
	
___qcp ________________________________________________________

	#Copy folder 		    ---> cp -a /source/. /dest/
	#Copy files (limited spee)d ---> rsync -avu --bwlimit=20000 --progress /source/files/ /destination/files/ (speed limit 20000kb)
		#--chmod=Du=rwx,Dg=rx,Do=rx,Fu=rw,Fg=r,Fo=r
		#Du = Directory Owner (Read, write, execute)
		#Dg = Directory Group (Read, execute)
		#Do = Directory Users (all) (Read, execute)
		#Fu = File Owner (Read, write)
		#Fg = File Group (Read)
		#Fo = File Users (all) (Read)

	#Copy file by using scp
		scp -P 443 ~/Downloads/Homer-Conferencing.sh root@8.9.9.9:/root
	#Copy file by using scp via proxy
		scp -P2277 "/usr/bin/ssh -o ProxyCommand=nc -X 5 -x 127.0.0.1:9050 %h %p" ~/Downloads/file.txt admin@192.168.0.1:/home/admin/www/1main/qwe/1s/source/
___qtouch_______________________________________________________

	#Change "Data Creation" and "Last Modified of file"
		touch -a -m  -t 201001011212 temp.txt
		# -a = accessed
		# -m = modified
		# -t  = timestamp - use [[CC]YY]MMDDhhmm[.ss] time format


___qrsync_______________________________________________________

	#https://www.linuxtechi.com/rsync-command-examples-linux/
	#Transfer file from remout server to local server
		rsync -v -e ssh remoteuser@X.X.X.X:/home/remoteuser/transfer/testfile.txt /home/localuser/
		rsync -avu --bwlimit=5000 --chmod=Fu=rwx,Fg=r,Fo=r --progress /from_folder/ ~/destination_folder/
	#rsync for resume download use "--partial"
		rsync -azvvP /home/path/folder1/ /home/path/folder2

	#rsync for cloud storage
		rsync -azvvpu --delete -e "ssh -p443" ~/.A8fhd4ksDk root@ip-address:/home/
		#--ignore-existing - copy only new file

	#rsync exclude directorie
		rsync -avzP --bwlimit=5000 --exclude {'file1.txt','dir1/*','dir2'} /source-directory /destination-directory
		#example
			rsync -avzP --exclude {'/.save/', '/save/'} ~/ /Volumes/hdd/john/
		#rsync without recurcive ---> -d

		#example exclude ONE directorie
			rsync -avzP --exclude '/save/' ~/ /Volumes/hdd/john/

	#rsync transfer file to server via socks5 proxy
		rsync --progress -avrz -e 'ssh -p2244 -o ProxyCommand="nc -X 5 -x 127.0.0.1:5060 %h %p"' /Users/john/Downloads/test.mp4  admin@remoute-server:/name-folder/filename

	___qhosts__/etc/hosts_______________________________________________
	127.0.0.1 analytics.google.com
	127.0.0.1 www.google-analytics.com
	127.0.0.1 google-analytics.com
	127.0.0.1 ssl.google-analytics.com


___qExif___________________________________________________________

	#Install ---> brew install exiftool
	#Show all date for file ---> exiftool -time:all -a -G0:1 -s file.txt
	#Change exif set CreateDate = CreationDate (So the main is set CreateDate, CreationDate and FileModifyDate the date you'l need)
		exiftool "-CreateDate<CreationDate" "-FileModifyDate<CreateDate" file.mp4

	#Example, if Photo has a wrong time sequence
		1. See (exiftool -time:all -a -G0:1 -s IMG_0531.mov) if CreationDate has right time then Step2 change "CreateDate" and "FileModifyDate"
		2. exiftool "-CreateDate<CreationDate" "-FileModifyDate<CreationDate" file.mpr

	#Manual set date
		exiftool -AllDates="2013:08:27 08:50:14" ~/Camera3/Camera/IMG_8458_1.mov

___qSMART__________________________________________________________

	#install smart on centos ---> yum install smartmontools
	#Show SMART HDD smart ---> smartctl -A /dev/sda 
	#Show SMART HDD smart ---> smartctl -a disk3s2  
	#The main parametr 5 - "Reallocated_Sector"
	second 197 - "Current_Pending_Sector"

	#insatall smart on macos ---> brew install smartmontools
	#run -> smartctl -a disk0 (and watch "Percentage Used" if it more than 30% soom mac will die)


___qMySQL_________________________________________________________________________________________________________

	Step 1: Install MariaDB (if not already installed)
		sudo apt update
		sudo apt install mariadb-server
	 	sudo systemctl start mariadb
		sudo systemctl enable mariadb
  
 	Step 2: Secure the MariaDB installation
 		sudo mysql_secure_installation
   
   	Step 3: Log in to the MariaDB shell
    		sudo mysql -u root -p
      
      	Step 4: Create a database
		CREATE DATABASE your_database_name;
  	Step 5: Create a user
   		CREATE USER 'your_user_name'@'localhost' IDENTIFIED BY 'your_password';
     	Step 6: Grant privileges
      
      		GRANT ALL PRIVILEGES ON your_database_name.* TO 'your_user_name'@'localhost';
		FLUSH PRIVILEGES;
  		EXIT;
    
    	Step 8: Verify the user and database
     		mysql -u your_user_name -p
       		SHOW DATABASES;
	 
  	Step 9: Import the Database
   		mysql -u your_user_name -p your_database_name < /path/to/your_file.sql
			

	#Connect to BD and make Query
		mysql -u username -puserpass dbname -e "UPDATE mytable SET mycolumn = 'myvalue' WHERE id='myid'";
		mysql --user=username --password=passwordtext basename
		mysql --user=username --password=passwordtext basename -e "SELECT * FROM main WHERE id='1'";
		mysql --user=username --password=passwordtext basename -e "SELECT * FROM user WHERE password='aaaaabbbbb'";
		
	#Show tables of database
		#Enter in mysql
		use name_of_database;
		show tables;

	#Show columns from [table name];
		show columns from table;
	
	select post_text from posts where post_text like "%http:%" and id<100;
	update posts SET post_text = REPLACE(post_text, 'http://name', 'https://name') WHERE post_text LIKE '%http://name%';
	
	#Replace
	UPDATE main SET other_cont_en = REPLACE(other_cont_en, ' width="800" height="600"', '') WHERE INSTR(other_cont_en, '%width="800"%') > 0;
	SELECT * FROM `main` WHERE `other_cont_en` like '% width="800" height="600"%'

	#Make dump-file(backup base) with base MySQL  
		mysqldump --single-transaction -u user -p DBNAME > backup.sql

	#Recover from dump-file to MySQL
		mysql basename -u username -p < base.sql
	-------------------------------------------------------------------------------
	/usr/bin/mysqladmin -u root password 'new-password'
	mysql -u root -p
	mysql> CREATE DATABASE name_bae;
	mysqladmin -u root -p'oldpassword' password newpass
	-------------------------------------------------------------------------------
	#Install qMariaDB
		yum remove mysql mysql-server
		mv /var/lib/mysql /var/lib/mysql_old_backup
		yum install mariadb-server -y
		yum install mariadb -y
		chmod -R 777 /var/lib/mysql/
		systemctl enable mariadb.service
		service mariadb start
		/usr/bin/mysql_secure_installation

		#Enter current password for root (enter for none): Enter
		#Set root password? [Y/n]: Y
		#New password: <your-password>
		#Re-enter new password: <your-password>
		#Remove anonymous users? [Y/n]: Y
		#Disallow root login remotely? [Y/n]: Y
		#Remove test database and access to it? [Y/n]: Y
		#Reload privilege tables now? [Y/n]: Y

	-------------------------------------------------------------------------------
	#Reset password for MySQL
		service mariadb stop
		mysqld_safe --skip-grant-tables &
		mysql -u root
		mysql> use mysql;
		mysql> update user set password=PASSWORD("NEW-ROOT-PASSWORD") where User='root';
		mysql> flush privileges;
		mysql> quit
		service mariadb start
		mysql -u root -p (check)
	-------------------------------------------------------------------------------
	#Create user and database
		mysql -u root -p
		GRANT ALL PRIVILEGES ON *.* TO 'newuser'@'localhost' IDENTIFIED BY '5w7PJcCp';
	#then login as new user ---> mysql -u newuser -p
	#and create database for user ---> CREATE DATABASE `newuser`;
	-------------------------------------------------------------------------------
	#check and fix mysql errors and problem ---> mysql_upgrade -uroot -p
	#chemk if service mariadb active ---> systemctl is-active mariadb

	#drop database
		mysql -u root -p
		show databases;
		drop database name; (if is there "-" use ---> drop database `name`;)


	#ERROR mariadb cant start after Reset password for MySQL
	#use ---> ps aux | grep -i mysql
	#and kill all mysql processes and start MariaDB again	


___qWireGuard_______________________________________________________________________________________________________

	#Install Centos7
	#https://www.wireguard.com/install/#red-hat-enterprise-linux-7-centos-7-module-tools
	#https://www.cyberciti.biz/faq/ubuntu-20-04-set-up-wireguard-vpn-server/

	sudo yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
	sudo curl -o /etc/yum.repos.d/jdoss-wireguard-epel-7.repo https://copr.fedorainfracloud.org/coprs/jdoss/wireguard/repo/epel-7/jdoss-wireguard-epel-7.repo
	sudo yum install wireguard-dkms wireguard-tools


	#install server then install desctop ubuntu (link install any ubuntu desctop - https://www.makeuseof.com/install-desktop-environment-gui-ubuntu-server/)
		sudo apt install ubuntu-desktop


	#Install wireguard (https://www.the-digital-life.com/wireguard-installation-and-configuration/)
	# https://www.youtube.com/watch?v=bVKNSf1p1d0

	1. Install WireGuard on Server and Client
		sudo apt update && sudo apt install wireguard
	2. Generate keys on Server and Client
		wg genkey | tee privatekey | wg pubkey > publickey
	3. Configure server (/etc/wireguard/wg0.conf)
		----------------------------------------------------------------------------------------------------------------
		[Interface]
		PrivateKey=<server-private-key>
		Address=<server-ip-address>/<subnet>
		SaveConfig=true
		PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o <public-interface> -j MASQUERADE;
		PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o <public-interface> -j MASQUERADE;
		ListenPort = 51820
		----------------------------------------------------------------------------------------------------------------

		---------------Example------------------------------------------------------------------------------------------
		[Interface]
		Address = 10.0.0.1/8
		PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE;
		PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE;
		ListenPort = 51820
		PrivateKey = cMe2MDoXJ8be09bYhd0azj/PPJEPGi1riHEpnYP8lVI=

		[Peer]
		PublicKey = J+RoaunI4NENqLrRMsc3ofz/cfW/KYa3etpRfFHhAmY=
		AllowedIPs = 10.0.0.2/32
		----------------------------------------------------------------------------------------------------------------
	4. Start wg on Server
			wg-quick up wg0
		#or for Mac OS
			wg-quick up ~/.config/wireguard/wg0.conf

	5. Configure client (/etc/wireguard/wg0.conf)
		----------------------------------------------------------------------------------------------------------------
		[Interface]
		PrivateKey = <client-private-key>
		Address = <client-ip-address>/<subnet>

		[Peer]
		PublicKey = <server-public-key>
		Endpoint = <server-public-ip-address>:51820
		AllowedIPs = 0.0.0.0/0
		----------------------------------------------------------------------------------------------------------------
		[Interface]
		Address = 10.0.0.2/8
		PrivateKey = eIurbkxVvISdOuiGnr4fGkjofbvueoC/qeNGy45l61A=

		[Peer]
		PublicKey = fUDlm68ko0wxGBNA7cWj1V1sBoH5bQl8suCioYRVjzU=
		AllowedIPs = 0.0.0.0/0
		Endpoint = 94.130.176.20:51820
		PersistentKeepalive = 30
		----------------------------------------------------------------------------------------------------------------

	6. Start wg on Client
			wg-quick up wg0
		#or for Mac OS
			wg-quick down ~/.config/wireguard/wg0.conf
	
	7. Configure on server allow ip forward in cat /proc/sys/net/ipv4/ip_forward set "1"
		sudo sysctl -w net.ipv4.ip_forward=1
		sudo sysctl -p

	---------------------------------------------------------------------------------------------------------------------
	#Client for Mac - https://medium.com/@headquartershq/setting-up-wireguard-on-a-mac-8a121bfe9d86


___qVNC on remoute server__________________________________________________________________________________________

	#install vnc server
		https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-20-04
	#make tunel for vnc viewer on local pc
		ssh -i ~/.ssh/id_rsa -L59000:localhost:5901 root@ip-address -Nf

	#kill VNC
		vncserver -kill :1

	#start VNC on localhost
		vncserver -localhost


___qbase64__________________________________________________________________________________________________________

	#encode to base64 ---> echo password | base64
	#decode from base64 ---> echo 'password' | base64 --decode

___qsshfs___________________________________________________________________________________________________________

	#Install sshfs on Centos 7
		yum install epel-release
		yum install fuse-sshfs
	#Mount
		sshfs foobar@remote.server:/home/foobar /mnt

___qnetwork____________________________NETWORK______________________________________________________________________

	#Change qhostname - https://www.tecmint.com/set-change-hostname-in-centos-7/
	#network (NFS - network file system)
		watch 'netstat -na | grep -v 8080 | grep tcp | sort|grep EST'

	#check all connection status
		netstat -tan | awk '{print $6}' | sort | uniq -c
  
	#Show all open ports
		netstat -nap | grep LISTEN
		or
		netstat -lntup
  
	#Show route table in mac os
		netstat -rn

	#Chenge default route
		route delete default
		route add default 192.168.0.1
	or
		route change default -interface $INTF
		route change 192.168.0.0/16 -interface $INTF

	#Configure Network config (Red Hat)
		#https://www.cyberciti.biz/faq/howto-setting-rhel7-centos-7-static-ip-configuration/
		/etc/sysconfig/network-scripts/ifcfg-eth0
		IPADDR=192.168.1.200
		NETMASK=255.255.255.0
		GATEWAY=192.168.1.1
		DNS1=1.0.0.1
		#or use ---> nmtui

	#DISABLE ipv6 in Red Hat
		sysctl -w net.ipv6.conf.all.disable_ipv6=1
		sysctl -w net.ipv6.conf.default.disable_ipv6=1

	#Disable ipv6 (Ubuntu)
		sudo sh -c 'echo 1 > /proc/sys/net/ipv6/conf/eth0/disable_ipv6'

	#Disable ipv6 (Android)
		echo 0 > /proc/sys/net/ipv6/conf/wlan0/accept_ra
		echo 1 > /proc/sys/net/ipv6/conf/wlan0/disable_ipv6

	#Conect WI-FI via terminal ---> http://rus-linux.net/MyLDP/consol/wifi-from-command-line.html
	
	#Set DNS 		   ---> /etc/resolv.conf

		netstat -nlpt (t - TCP pocket, p - show program using connection, l - Show only listening sockets)

	#REAL TIME network monitor ---> watch lsof -i
	#REAL TIME network monitor ---> watch 'netstat -nap | grep -v 8080 | grep tcp | sort'

	#Show all route        ---> netstat -rn
	#Show all route        ---> route -n
	#Show default geatways ---> ip route show 

	#Show specifications of WI-FI, ethernet -->ethtool -i [wlan0]

	#To show what SERVICE using internet -->lsof -P -i -n | cut -f 1 -d " " | uniq
	#or netstat -nap | grep -v '127.0.0.1' | grep tcp | grep -v nginx | grep -v ':80'

___qtcpdump__________________________________________________________________________________________________________

	#Whatch all dns querys with tcpdump
	sudo tcpdump -i en0 port 53

___qwine_____________________________________________________________________________________________________________

	#install
	brew cask install wine-stable

___qbrew____________________________________________________________________________________________________________

	#brew install from source example
	brew install --build-from-source ffmpeg

___qwindows make usb with windows setup__________________________________________________

	diskutil eraseDisk MS-DOS "WIN10" MBR /dev/disk2
	diskutil eraseDisk MS-DOS "WIN10" GPT /dev/disk2
	#or
	diskutil eraseDisk ExFAT "WIN10" GPT /dev/disk2

	hdiutil mount ~/Downloads/Win10_1903_V1_English_x64.iso
	cp -rp /Volumes/CCCOMA_X64FRE_EN-US_DV9/* /Volumes/WIN10/	

	#remove hebirnate
		sudo pmset -a sleep 0; sudo pmset -a hibernatemode 0; sudo pmset -a disablesleep 1;
	#enable 
		sudo pmset -b disablesleep 0;sudo pmset -b sleep 5;

___qUbuntu_________________________________________________________________________________________

	#Replace Alt and command (the interrupt command will replaced from Ctrl+c to Shift+Ctrl+c)

	............................................................................................. vim ~/.Xmodmap
	clear control
	clear mod4

	keycode 105 =
	keycode 206 =

	keycode 133 = Control_L NoSymbol Control_L
	keycode 134 = Control_R NoSymbol Control_R
	keycode 37 = Super_L NoSymbol Super_L

	add control = Control_L
	add control = Control_R
	add mod4 = Super_L
	............................................................................................................
	xmodmap ~/.Xmodmap

	#Disable Double Tap (need install synclient) https://askubuntu.com/questions/939065/disable-double-tap-but-keep-one-tap-to-click-enabled-on-touchpad
	synclient MaxTapMove=2

	#run script start.sh at boot
		echo "script body" > ~/start.sh
		cp ~/start.sh /etc/init.d/start.sh
		sudo update-rc.d start.sh defaults

	---------------------------------------------------------
	install Hardware Sensor Indicator
	---------------------------------------------------------
	qi3 windows tail manager (HOTKEYS - https://i3wm.org/docs/refcard.html)
	tail manager yabai - https://github.com/koekeishiya/yabai/wiki/Disabling-System-Integrity-Protection
	sudo apt install i3
	/usr/bin/i3-config-wizard

___ qbash ____________________________________________________________________________________

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

___qsort_________________________________________________

	sort -k2 -n -t '['

	-k sort by column number
	-n sort by numbers (from 1 to 9)
	-t custom separator

___qcut__________________________________________________

	cut -d ">" -f4
	-d delete separator
	-f the number of columb that shows after

___quniq_________________________________________________

	uniq -c 
	-c count uniq element

___qawk__________________________________________________

	#about qawk - https://www.shellhacks.com/awk-print-column-change-field-separator-linux-bash/
		$1 - take first field
		$(NF) - take the last field
		$(NF-1) - take the last -1 field

	#print column 1 and 2 with " - " separator, and prints column also seperated with space
		awk -F ' ' '{print $1" - "$2}'

	#take only the last field
		awk -F ' ' '{print $NF}' 

	#take the last - 1 field
		awk -F ' ' '{print $(NF-1)}' 

___qln________qsymlink___________________________________

	#Make symlinked ---> (sudo ln -s /symlink  /folder)
	#Show the path for ln symbollind ---> readlink -f symlinkName

___qLocale____________________________________________________________________                         

	https://www.shellhacks.com/linux-define-locale-language-settings/

	#list of all locales
		localedef --list-archive

	#Installed locales
		locale -a | grep hi_IN.utf8

	#set language for current session
		LANG=en_US.utf8 or LANG=ru_RU.utf8

	#In Centos add/create to /etc/environment
		LANG=en_US.UTF-8
		LC_ALL=en_US.UTF-8

___qSCREEN____________________________________________________________________________________________________________

	#config file /etc/screenrc

	#run command at every windows (execute from screen with :[command]) ---> at "#" stuff "source ~/Documents/save/1/alias"
        #execute command for window 0 ---> screen -S SessionName -p0 -X stuff "command"$(echo -ne '\015')
		screen -S ses -p [0-4] -X stuff "ls -la"$(echo -ne '\015')
		screen -S ses -X at "#" stuff "c"$(echo -ne '\015')

	#Set password for screen session ---> ^A and type ":password"
	#kill session 	 --->  screen -X -S [session # you want to kill] quit
	#or 		 ---> ^D then "\"
	#kill all screen ---> pkill screen

	#connect to session ---> screen -x [screen session]

	#New screen session ---> screen -S [new session name]

	#Rename screen session ---> :sessionname [name]
		#or ---> screen -S old_session_name -X sessionname new_session_name

	#Move windows to another number ---> :number 7 (in move current windows to '7' and wndows '7' to current windows)

	#Execute command to every windows ---> :at "#" stuff "ls -l^M"

	#WATCH HELP ^A then ?	
	#To split vertically 		  ---> ^A |.
	#To split horizontally 		  ---> ^A S
	#To unsplit 			  ---> ^A Q
	#Resize				  :resize 20
	#Split				  :split
	#Unsplit current window		  :remove
	#To switch from one to the other  ---> ^A then Tab (or ^I)
	#To close the pane that has focus ---> ^A X
	#Set windows title 		  ---> :title NewWindowsTitle
	#Lock session			  ---> ^A x
	#Kill window 			  ---> ^A k or (Ctrl-a :kill)
	#monitor window for silence	  ---> C-a _
	#redraw window 			  ---> C-a C-l
	#enable logging in the screen session ---> C-a H 
	#change to window by number or name ---> C-a ' <number or title>

	http://www.softpanorama.org/Utilities/Screen/screenrc_examples.shtml
	.................................................................................................START......vim ~/.screenrc
	#terminfo * F9=\E[20~Q
	#bindkey -k F9 next
	#
	#escape ``
	escape ^Kk
	#escape Ctrl+\
	#escape ^\\

	bind = resize =
	bind + resize +2
	bind - resize -2
	bind _ resize max

	bindkey -k k1 :"pwd"
	bind '"\e[24~":"foobar"'
	set password none


	#screen -t net #rtorrent
	#screen -t 1 #irssi
	#screen -t 2
	#screen -t 3
	#screen -t 4
	altscreen on
	term screen-256color
	bind ',' prev
	bind '.' next
	#
	#change the hardstatus settings to give an window list at the bottom of the
	#screen, with the time and date and with the current window highlighted
	hardstatus alwayslastline
	hardstatus string '%{= kG}[ %{G}%H %{g}][%= %{= kw}%?%-Lw%?%{r}(%{W}%n*%f%t%?(%u)%?%{r})%{w}%?%+Lw%?%?%= %{g}][%{B} %m-%d %{W}%c %{g}]'
	.......................................................................................................................END.

	............................................................................................................screenrc centos
	escape ^Jj
	bind = resize =
	bind + resize +1
	bind - resize -1
	bind _ resize max

	caption always "%{= kw}%-w%{= gW}%n %t%{-}%+w %-= activity - %Y-%m-%d %C:%s"
	..............................................................................................................
	#Next windows 		---> ^A then [n]. (works for n∈{0,1…9})
	#Switch between terminals using list ---> ^A+a then " (useful when more than 10 terminals)
	#Send ^A to the underlying terminal ^A then a.

___qarh (q7zip,tar.gz)_qtar__qzip______________________________________________________________________________________

	#Unarhiv 7zip file with password (q7z,qz7)
	7z -pPASSWORD x file.zip

	#pack   ---> tar -zcvf output_file.tar.gz /foder_what_to_encrypt
	       |   | gzip vds1
	#unpack ---> tar zxvf hydra-4.1-src.tar.gz

	unrar e file.rar
___qDD________________________________________________________________________________________________________________

	https://habr.com/ru/post/117050/

	#Clone hdd from sda to sdb ---> sudo dd if=/dev/sda of=/dev/sdb bs=1M conv=noerror,sync status=progress
		#noerror - ignore errors
		#sync: сообщает утилите dd о необходимости заполнения блоков нулями в случае возникновения ошибок чтения

	#Delete ALL data on HDD ---> dd if=/dev/zero of=/dev/sda bs=4k
				or ---> dd if=/dev/zero of=/dev/Pd3 bs=1M count=999999999999

	#Delete folder forever ---> for filename in /pathtofolder/*; do dd if=/dev/zero of="$filename" bs=1k count=200024; done

