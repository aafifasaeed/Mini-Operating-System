#!/bin/bash


#function f_new
f_new(){
if zenity --entry \   #zenity displays a dialog box
--title="Add new profile" \
--text="Enter name of new profile:" --width 309 --height 240 \
--entry-text "NewProfile">>USER.txt     #entered text is added into file user
#If the user clicks the OK button, the zenity command returns a success status (0), and the text is appended to the file. 
  then echo $?          #
  else echo "No name entered"
fi

}
f_f(){

OLDIFS="$IFS"
IFS='-'
FILES=$(zenity --file-selection --multiple --separator='-' --title "Pick a file")
IFS="$OLDIFS"

for file in "${FILES[@]}"
do
    if [[ $file == /* && -f $file ]]
    then
        echo $file
    fi
done
}
f_open(){
zOpenPath="$(zenity --file-selection)"
if [ "$zOpenPath" != "" ]
 then
  zData=$(cat "$zOpenPath")
  zNewData=$(echo -n "$zData" | zenity --text-info --editable --width 650 --height 400)
  zSavePath=$(echo -n "$(zenity --file-selection --filename="$zOpenPath" --save --confirm-overwrite)")
  echo -n "$zNewData" &gt; "$zSavePath"
fi

}

f_file(){
a="<span background='Orange'> <span font='15' color='black'>\nFile Mangement System\n</span></span>"
while true; do
  menu_launch="$(zenity  --width 400 --height 500 --cancel-label="Go-Back" --ok-label="Execute" --title="File Mangement" --text="${a}" --list --column "Choose Options:" "New File" "open"  "Current Position" "Show All Files" "list the content of a directory" "list all the content of a directory including the hidden files" "create a new directory" "Delete an existing directory" "creates a new File" "Delete a File" "Forcefully Delete File" "Copies the content of File File1 into File File2" "Copies the content of directory dir1 into directory dir2" "Rename files and directories" "print the content of a File" "print the first 10 lines of a file." "print the Last 10 lines of a file." "Count All File" "Find File" "Find Specific Word" "File Compress" "File Decompress" "Exit")"
         if [ "$menu_launch" = "Exit" ]; then
                   f_menu
          echo done
          exit
         elif [ "$menu_launch" = "New File" ]; then
		    echo $(gedit)
                    exit
	 elif [ "$menu_launch" = "open" ]; then
		    f_open
                    exit
	 elif [ "$menu_launch" = "Current Position" ]; then
		a=$(pwd |zenity --text-info --editable --width 650 --height 400)
                 echo $a
         elif [ "$menu_launch" = "Show All Files" ]; then
		a=$(file * |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "list the content of a directory" ]; then
		a=$(ls |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "list all the content of a directory including the hidden files" ]; then
		a=$(ls -la |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "create a new directory" ]; then
                        b=$(zenity --entry text="Hello")
		a=$(mkdir $b  |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Delete an existing directory" ]; then
                       b=$(zenity --entry text="Hello")
		a=$(rmdir $b |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "creates a new file" ]; then
                         b=$(zenity --entry text="Hello")
		a=$(touch $b |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Delete a File" ]; then
                     b=$(zenity --entry text="Hello")
		a=$(rm $b |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Forcefully Delete File" ]; then
                       b=$(zenity --entry text="Hello")
		a=$(rm -f $b |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Copies the content of File File1 into File File2" ]; then
                       b=$(zenity --entry text="Hello")
                       c=$(zenity --entry text="Hello")
		a=$(cp $b $c |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Rename files and directories" ]; then
                       b=$(zenity --entry text="Hello")
                       c=$(zenity --entry text="Hello")
		a=$(mv -r $b $c |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "print the content of a file" ]; then
                       b=$(zenity --entry text="Hello")
		a=$(cat $b  |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "print the first 10 lines of a file." ]; then
                       b=$(zenity --entry text="Hello")
		a=$(head $b  |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "print the Last 10 lines of a file." ]; then
                       b=$(zenity --entry text="Hello")
		a=$(tail $b  |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Count All File" ]; then
                       b=$(zenity --entry text="Hello")
		a=$(wc *$b  |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Find File" ]; then
                       b=$(zenity --entry --title="Which File You want To Find Enter Extension" text="Hello")
		a=$(find *$b  |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          
          elif [ "$menu_launch" = "File Compress" ]; then
                     
                b=$(zenity --entry --title="Enter File With Extension" text="Hello")
		a=$(gzip $b  |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "File Decompress" ]; then
                       
                       b=$(zenity --entry --title="Enter Extension" text="Hello")
		a=$(gzip -d $b.gz |zenity --text-info --editable --width 650 --height 400)
                 echo $a

         else
          f_menu
         fi
done



}
f_user(){
a=$(zenity --forms --title="Add User" \
	--text="Enter information about your User." \
	--separator="," \
	--add-entry="User Name" \
	--add-password="Password" \
	--add-password="Confirm Password" >>USE.txt)
if [ $a = 0 ]; then
zenity --error
fi

case $? in
    0)
        echo "Friend added.";;
    1)
        echo "No friend added."
	;;
    -1)
        echo "An unexpected error has occurred."
	;;
esac


}


f_system_info(){
a=$(lshw -short|zenity --text-info --width 650 --height 400)
                 echo $a

}

f_pack(){
a="<span background='Orange'><span font='30' color='black'>\nPackage Management\n</span></span>"
while true; do
  menu_launch="$(zenity  --width 400 --height 500 --cancel-label="Go-Back" --ok-label="Execute" --title="Packages" --text="${a}" --list --column "Choose Options:" "nmap" "nmap remove" "update"  "dpkg l" "dpkg -l | grep apache2" "dpkg -L ufw" "unattended-upgrades" "install apticron" "Yum Package" "Dnf Package" "List of Dnf Repositories" "List All Available Packages" "Find Package Name with brief Description" "List All Snap Packages" "List All Flatpak Packages" "Packages Installed Recently" "Stats of Cache" "Install or Upgrade Specific Packages" "Remove Package Completely" "Download Only Source Code of Package" "EXIT")"
         if [ "$menu_launch" = "EXIT" ]; then
                   f_menu
          echo done
          exit
         elif [ "$menu_launch" = "nmap" ]; then
		   a=$(sudo apt install nmap|zenity --text-info --width 650 --height 400)
                 echo $a
         elif [ "$menu_launch" = "nmap remove" ]; then
		   a=$(sudo apt remove nmap|zenity --text-info --width 650 --height 400)
                 echo $a
	 elif [ "$menu_launch" = "update" ]; then
		 a=$(sudo apt update|zenity --text-info --editable  --width 650 --height 400)
                 echo $a
	 elif [ "$menu_launch" = "dpkg l" ]; then
		a=$(dpkg -l|zenity --text-info --width 650 --height 400)
                 echo $a
         elif [ "$menu_launch" = "dpkg -l | grep apache2" ]; then
                 a=$(dpkg -l | grep apache2 |zenity --text-info --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "dpkg -L ufw" ]; then
		a=$(dpkg -L ufw|zenity --text-info --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "unattended-upgrades" ]; then
		a=$(sudo apt install unattended-upgrades|zenity --text-info --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "install apticron" ]; then
		a=$(sudo apt install apticron|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Yum Package" ]; then
		a=$(sudo apt install yum|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Dnf Package" ]; then
		a=$(yum install dnf|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "List of Dnf Repositories" ]; then
		a=$(sudo dnf repolist|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "List All Available Packages" ]; then
		a=$(apt-cache pkgnames|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Find Package Name with brief Description" ]; then
		pn=$(zenity --entry text="Hello")
		a=$(apt-cache $pn|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "List All Snap Packages" ]; then
		a=$(snap list|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "List All Flatpak Packages" ]; then
		a=$(flatpak list|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Packages Installed Recently" ]; then
		a=$(grep " install " /var/log/dpkg.log|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Stats of Cache" ]; then
		a=$(apt-cache stats|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Install or Upgrade Specific Packages" ]; then
		spn=$(zenity --entry text="Hello")		
		a=$(sudo apt-get install $spn |zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Remove Package Completely" ]; then
		p1=$(zenity --entry text="Hello")		
		a=$(sudo apt-get purge $p1|zenity --text --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Download Only Source Code of Package" ]; then
		p1=$(zenity --entry text="Hello")
		a=$(sudo apt-get --download-only source $p2|zenity --text --width 650 --height 400)
                 echo $a
         else
         f_menu
         fi
done

}


f_utilities(){
a="<span background='Blue'><span font='30' color='White'>\nUtilities\n</span></span>"
while true; do
  menu_launch="$(zenity  --width 400 --height 500 --cancel-label="Go-Back" --ok-label="Execute" --title="Utilities" --text="${a}" --list --column "Choose Options:" "Display informantion of cmd" "Pattern of cmd" "Info of pseeudo cmd" "Short dump of information" "Summary of the memory usage" "Group" "History" "Kill" "Man" "Passupdate" "Running processes" "Force shutdown" "Watch" "Top -Ss" "Shh cmd" "EXIT")"
         if [ "$menu_launch" = "EXIT" ]; then
                   f_menu
          echo done
          exit
         elif [ "$menu_launch" = "Display informantion of cmd" ]; then
		    a=$(help -d |zenity --text-info --editable --width 650 --height 400)
                 echo $a
         elif [ "$menu_launch" = "Pattern of cmd" ]; then
		    a=$(help -s |zenity --text-info --editable --width 650 --height 400)
                 echo $a
	  elif [ "$menu_launch" = "Info of pseeudo cmd" ]; then
		a=$(help -m |zenity --text-info --editable --width 650 --height 400)
                 echo $a
         elif [ "$menu_launch" = "Short dump of information" ]; then
		a=$(finger |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Summary of the memory usage" ]; then
		a=$(free -h |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Group" ]; then
		a=$(groups |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "History" ]; then
		a=$(!188 |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Kill" ]; then
		a=$(ps |zenity --text-info --editable --width 650 --height 400)
                 echo $a
	  elif [ "$menu_launch" = "Man" ]; then
		a=$(Man chown |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Pass update" ]; then
		a=$(sudo passwd |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Force shutdown" ]; then
		a=$(shutdown now |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Shh cmd" ]; then
		a=$(shh |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Top -Ss" ]; then
		a=$(top -Ss |zenity --text-info --editable --width 650 --height 400)
                 echo $a
          elif [ "$menu_launch" = "Watch" ]; then
		a=$(watch |zenity --text-info --editable --width 650 --height 400)
                 echo $a
         else
          f_menu
         fi
done	
}

f_storage(){
a="<span background='Blue'><span font='30' color='Black'>\nStorage\n</span></span>"
while true; do
  menu_launch="$(zenity  --width 400 --height 500 --cancel-label="Go-Back" --ok-label="Execute" --title="Storage" --text="${a}" --list --column "Choose Options:" "Memory Usage" "Memory Usage Statistics" "Memory CPU PerProcess"  "Deep PerProcess Detail" "Check Storage Byte" "Check Storage KiloByte" "Check Storage MegaByte" "Check Storage GigaByte" "Check Storage TeraByte" "High and Low Memory Stats" "Exit")"
         if [ "$menu_launch" = "Exit" ]; then
                   f_menu
          echo done
          exit

         elif [ "$menu_launch" = "Memory Usage" ]; then
		a=$(free |zenity --text-info --editable --width 650 --height 400)
		echo $a
		exit

	 elif [ "$menu_launch" = "Memory Usage Statistics" ]; then
		a=$(vmstat |zenity --text-info --editable --width 650 --height 400)
		echo $a
		

	 elif [ "$menu_launch" = "PerProcess Detail" ]; then
		a=$(top |zenity --text-info --editable --width 650 --height 400)
		echo $a
                

	 elif [ "$menu_launch" = "Deep Perprocess Detail" ]; then
		a=$(htop |zenity --text-info --editable --width 650 --height 400)		echo $a
		

	 elif [ "$menu_launch" = "Check Storage Byte" ]; then
		a=$(free -b |zenity --text-info --editable --width 650 --height 400)		echo $a
		
		

         elif [ "$menu_launch" = "Check Storage KiloByte" ]; then
		a=$(free -k |zenity --text-info --editable --width 650 --height 400)
		echo $a
                

         elif [ "$menu_launch" = "Check Storage MegaByte" ]; then
		a=$(free -m |zenity --text-info --editable --width 650 --height 400)
                echo $a

         elif [ "$menu_launch" = "Check Storage GigaByte" ]; then
		a=$(free -g |zenity --text-info --editable --width 650 --height 400)
                echo $a
	elif [ "$menu_launch" = "Check Storage TeraByte" ]; then
		a=$(free --tera |zenity --text-info --editable --width 650 --height 400)
                echo $a
	
	elif [ "$menu_launch" = "High and Low Memory Stats" ]; then
		a=$(free -l |zenity --text-info --editable --width 650 --height 400)
                echo $a
         else
          f_menu
         fi
done


}

f_browser(){
zenity --info --text="Fire fox has being Opened"
firefox -p
}

f_process(){
a="<span background='Orange'><span font='30' color='black'>\nProcess\n</span></span>"
while true; do
  menu_launch="$(zenity  --width 400 --height 500 --cancel-label="Go-Back" --ok-label="Execute" --title="Process" --text="${a}" --list --column "Choose Options:" "Running Processes" "Visualize Processes" "All-User Processes" "Priority Process" "File System Information" "Hidden File Information" "Run/Stop in Foreground" "Send Process to Background" "Status of Process" "Returns Process ID" "Kill Process" "Display Active Process" "Display All Process" "Full Format Listing" "Process Owned" "Display Process by User Real ID" "All Process running as Root" "List Process by Group" "List Process by Effective Group" "Print Process Tree" "To List All Specifiers" "View Pid PPID UserName And Command" "Exit")"
         if [ "$menu_launch" = "Exit" ]; then
                   f_menu
          echo done
          exit

         elif [ "$menu_launch" = "Running Processes" ]; then
		a=$(ps |zenity --text-info --editable --width 650 --height 400)
                echo $a

	 elif [ "$menu_launch" = "Visualize Processes" ]; then
		a=$(pstree -g |zenity --text-info --editable --width 650 --height 400)
                echo $a
		

	 elif [ "$menu_launch" = "All-User Processes" ]; then
		a=$(ps aux |zenity --text-info --editable --width 650 --height 400)
                echo $a

	 elif [ "$menu_launch" = "Priority Process" ]; then
		a=$(nice |zenity --text-info --editable --width 650 --height 400)
                echo $a
	elif [ "$menu_launch" = "File System Information" ]; then
		a=$(df |zenity --text-info --editable --width 650 --height 400)
                echo $a

elif [ "$menu_launch" = "Hidden File Information" ]; then
		a=$(df -h |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Run/Stop in Foreground" ]; then
		a=$(fg |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Send Process to Background" ]; then
		a=$(bg |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Status of Process" ]; then
		a=$(ps PID |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Returns Process ID" ]; then
		a=$(pidof |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Kill Process" ]; then
		a=$(kill PID |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Display Active Process" ]; then
		a=$(ps -A |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Display All Process" ]; then
		a=$(ps au |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Full Format Listing" ]; then
		a=$(ps -ef |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Process Owned" ]; then
		a=$(ps -x |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Display Process by User Real ID" ]; then
		a=$(ps -fu |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "All Process running as Root" ]; then
		a=$(ps -U root -u root  |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "List Process by Group" ]; then
		a=$(ps -fG apache |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "List Process by Effective Group" ]; then
		a=$(ps -fg apache |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "Print Process Tree" ]; then
		a=$(ps -e --forest |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "To List All Specifiers" ]; then
		a=$(ps L |zenity --text-info --editable --width 650 --height 400)
                echo $a
elif [ "$menu_launch" = "View Pid PPID UserName And Command" ]; then
		a=$(ps -eo pid,ppid,user,cmd |zenity --text-info --editable --width 650 --height 400)
                echo $a
 	else
		f_menu

        	 fi
done

}




f_calendar(){
	zenity --info --text="Calendar has being Opened"
	zenity --calendar
}

f_NewTerminal(){
	zenity --info --text="New Terminal has being Opened"
	gnome-terminal
}


f_network(){

a="<span background='Orange'><span font='30' color='black'>\nNetwork\n</span></span>"
while true; do
  menu_launch="$(zenity  --width 400 --height 500 --cancel-label="Go-Back" --ok-label="Execute" --title="Network" --text="${a}" --list --column "Choose Options:" "PC Name" "IP" "Ping"  "WLAN IP" "NSLOOKUP" "NetStat" "Active Conn" "Exit")"
         if [ "$menu_launch" = "Exit" ]; then
                   f_menu
          echo done
          exit

         elif [ "$menu_launch" = "PC Name" ]; then
		a=$(hostname |zenity --text-info --editable --width 650 --height 100)
		echo $a
		

	 elif [ "$menu_launch" = "IP" ]; then
		a=$(hostname -I |zenity --text-info --editable --width 650 --height 100)
		echo $a
		

	 elif [ "$menu_launch" = "Ping" ]; then
		a=$(zenity --entry --title="IP Address" --text="Enter the IP Address")
		echo $a
	    	b=$(ping $a |zenity --text-info --editable --width 650 --height 400)
                echo $b
                

	 elif [ "$menu_launch" = "WLAN IP" ]; then
		a=$(pwd |zenity --text-info --editable --width 650 --height 400)
                echo $a

         elif [ "$menu_launch" = "NSLOOKUP" ]; then
		a=$(zenity --entry --title="IP Address" --text="Enter the IP Address")
		echo $a
	    	b=$(nslookup $a |zenity --text-info --editable --width 650 --height 400)
                echo $b
                

         elif [ "$menu_launch" = "NetStat" ]; then
		a=$(netstat -r |zenity --text-info --editable --width 650 --height 400)
                echo $a

         elif [ "$menu_launch" = "Active Conn" ]; then
		a=$(ss |zenity --text-info --editable --width 650 --height 400)
                echo $a
         else
          f_menu
         fi
done
}
f_datetime(){
#!/bin/bash

# Time, Date and Timezone configuration utility for Linux Lite
# Fork of Date and Time Setting Tool Copyright 2009,2011 by Tony Brijeski under the GPL V2
# TODO: add timezone settings, test on a live dvd, use image-list for the main dialog,

DIALOG="`which zenity` --width 400"
TITLE="--window-icon=/usr/share/icons/hicolor/24x24/apps/gnome-panel-clock.png  --print-partial --title="
TEXT="--text="
ENTRY="--entry "
ENTRYTEXT="--entry-text "
MENU="--list --print-column=1 --column=Pick --column=Info"
YESNO="--question "
MSGBOX="--info "
SCALE="--scale "
PASSWORD="--entry --hide-text "
TITLETEXT="Linux Lite Date and Time Setting Tool"

if [ $EUID -ne 0 ]; then
    gksu -m '
  <b>Lite Time requires Administrative privileges</b>
 
   
            Please enter your password to continue.' bash $0
         exit
     else :
fi


while [ "$SETCHOICE" != "Exit" ]; do
DAY="`date +%d`"
MONTH="`date +%m`"
YEAR="`date +%Y`"
MINUTE="`date +%M`"
HOUR="`date +%H`"

SETCHOICE=`$DIALOG --height 300 $TITLE"$TITLETEXT" $MENU $TEXT"Set Year Date and Time Tool\n\nTime=$HOUR:$MINUTE\nDate=$MONTH-$DAY-$YEAR\n\n" Exit "Quit" SETTIME "Set Current Time" SETDATE "Set Current Date"`
SETCHOICE=`echo $SETCHOICE | cut -d "|" -f 1`

if [ "$SETCHOICE" = "SETTIME" ]; then

HOUR="`date +%H`"
HOUR=`echo $HOUR | sed -e 's/^0//g'`

SETHOUR=`$DIALOG $TITLE"$TITLETEXT" $SCALE --value=$HOUR --min-value=0 --max-value=23 $TEXT"Move the slider to the correct Hour"`
if [ "$?" = "0" ]; then

if [ "${#SETHOUR}" = "1" ]; then
SETHOUR="0$SETHOUR"
fi

MINUTE="`date +%M`"
MINUTE=`echo $MINUTE | sed -e 's/^0//g'`
fi

SETMINUTE=`$DIALOG $TITLE"$TITLETEXT" $SCALE --value=$MINUTE --min-value=0 --max-value=59 $TEXT"Move the slider to the correct Minute"`
if [ "$?" = "0" ]; then

if [ "${#SETMINUTE}" = "1" ]; then
SETMINUTE="0$SETMINUTE"
fi

date $MONTH$DAY$SETHOUR$SETMINUTE$YEAR
hwclock --systohc
fi
fi

if [ "$SETCHOICE" = "SETDATE" ]; then
DAY="`date +%d`"
DAY=`echo $DAY | sed -e 's/^0//g'`
MONTH="`date +%m`"
MONTH=`echo $MONTH | sed -e 's/^0//g'`
YEAR="`date +%Y`"
SETYEAR=`$DIALOG $TITLE"$TITLETEXT" $SCALE --value=$YEAR --min-value=2000 --max-value=2020 $TEXT"Move the slider to the correct Year"`
if [ "$?" = "0" ]; then
SETMONTH=`$DIALOG $TITLE"$TITLETEXT" $SCALE --value=$MONTH --min-value=1 --max-value=12 $TEXT"Move the slider to the correct Month"`
if [ "$?" = "0" ]; then

if [ "${#SETMONTH}" = "1" ]; then
SETMONTH="0$SETMONTH"
fi


SETDAY=`$DIALOG $TITLE"$TITLETEXT" $SCALE --value=$DAY --min-value=1 --max-value=31 $TEXT"Move the slider to the correct Day"`
if [ "$?" = "0" ]; then

if [ "${#SETDAY}" = "1" ]; then
SETDAY="0$SETDAY"
fi


MINUTE="`date +%M`"
HOUR="`date +%H`"
date $SETMONTH$SETDAY$HOUR$MINUTE$SETYEAR
hwclock --systohc
fi
fi
fi
fi

done


exit 0
}



f_menu(){

WEEKDAY=`date +"%A"`
MONTH=`date +"%B"`
DAYOFMONTH=`date +"%d"`
YEAR=`date +"%Y"`
DATE=`date +"%F"`
TIME=`date +"%I:%M %p %Z"`

SHOWDATE="${MONTH} ${DAYOFMONTH}, ${YEAR}"


DATE_TIME_TO_SHOW="
<span background='Black'><span color='white'font='20'>Mini Clip project 2021\n</span>
<span font='15' color='red'><tt>Day : </tt></span><span font='15' color='red'>$WEEKDAY\n</span>
<span font='15' color='red'><tt>Date: </tt></span><span font='15' color='red'>$SHOWDATE\n</span>
<span font='15' color='red'><tt>Time: </tt></span><span font='15' color='red'>$TIME\n</span>
<span font='15' color='red'><tt>Made by:</tt></span><span font='9' color='red'>AAFIFA SAEED</span></span> "
         
while true; do
  menu_launch="$(zenity --width 600 --height 1200 --title="Mini OS" --text="${DATE_TIME_TO_SHOW}" --cancel-label="Shutdown" --ok-label="Start Module" --list --radiolist  --column "Select Module And Click Start Module Button" --column "Module Information" TRUE "File Management" FALSE "Storage" FALSE "Process" FALSE "Open Terminal" FALSE "System" False "Calendar" FALSE "Open Browser" FALSE "Network Details" FALSE "Utilities" FALSE "Add New User" FALSE "Package Management")"
         
         if [ "$menu_launch" = "Exit" ]; then    
          echo done
          exit
        elif [ "$menu_launch" = "File Management" ]; then
		      f_file
	elif [ "$menu_launch" = "Storage" ]; then
		      f_storage
	elif [ "$menu_launch" = "Process" ]; then
		      f_process
	elif [ "$menu_launch" = "Open Terminal" ]; then
		      f_NewTerminal
	elif [ "$menu_launch" = "System" ]; then
                       f_system_info
	elif [ "$menu_launch" = "Calendar" ]; then
                       f_calendar
	elif [ "$menu_launch" = "Open Browser" ]; then
                       f_browser 
        elif [ "$menu_launch" = "Network Details" ]; then
		      f_network
        elif [ "$menu_launch" = "Utilities" ]; then
		   f_utilities  
        elif [ "$menu_launch" = "Add New User" ]; then
		      f_user
        elif [ "$menu_launch" = "Package Management" ]; then
		      f_pack
         else
         exit
                  fi

done



}

usr=$(zenity --width 300 --height 100 --entry --text="Enter Your Username" --title="Login")

pass=$(zenity --width 300 --height 120 --password --text="Enter Your Password" --title="Login")
if  test "$usr" = 'aafifasaeed' && test "$pass" = '1234' 
then
       zenity --width 300 --height 100 --info --text="Login Successfull\n\n\nWelcome To Mini Clip Project" --ok-label="Launch" --title="Login"
               f_menu
              
else
       zenity --width 300 --height 100 --error --text="Acess Denied"
fi

   

