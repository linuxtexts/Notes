___qsc___________________________Spreadsheet Calculator_____________________________________________________________
--------------------------------------------------------------------------------------------------------------------
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
