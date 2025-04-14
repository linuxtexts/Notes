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
