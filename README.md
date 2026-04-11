# Toggle the notch on MacBooks

Toggle the MacBook notch on or off, using AppleScript to automate the process described [here](https://apple.stackexchange.com/questions/432284/make-macbook-pro-as-if-there-is-no-notch/472341#472341).

~~~flf
tell application "System Events"
	tell application "System Settings" to activate
	repeat until window 1 of process "System Settings" exists
		delay 0.5
	end repeat
	tell process "System Settings"
		tell application "System Settings"
			try
				reveal pane id "com.apple.Displays-Settings.extension"
			on error err
				if err = "REVEAL_PANE_ERR_MODAL" then
					tell application "System Events" to click menu item "Back" of menu "View" of menu bar item "View" of menu bar 1 of process "System Settings"
					repeat until (count of windows) is 1
						delay 0.5
					end repeat
					reveal pane id "com.apple.Displays-Settings.extension"
				end if
			end try
		end tell
		repeat until window "Displays" exists
			delay 0.5
		end repeat
		click button 1 of scroll area 2 of group 1 of group 3 of splitter group 1 of group 1 of window "Displays"
		repeat until exists sheet 1 of window "Displays"
			delay
		end repeat
		set reslist to checkbox 1 of group 1 of scroll area 1 of group 1 of sheet 1 of window "Displays"
		if value of reslist is 0 then
			click reslist
		end if
		click button 1 of group 1 of sheet 1 of window "Displays"
		set showall to checkbox 1 of group 1 of scroll area 2 of group 1 of group 3 of splitter group 1 of group 1 of window "Displays"
		if value of showall is 0 then
			click showall
		end if
		set rowlist to (rows of outline 1 of scroll area 1 of group 1 of scroll area 2 of group 1 of group 3 of splitter group 1 of group 1 of window "Displays")
		repeat with sel from 1 to (count rowlist)
			if selected of item sel of rowlist is true then exit repeat
		end repeat
		set selrow to item sel of rowlist
		repeat with def from 1 to (count rowlist)
			if value of static text 1 of UI element 1 of item def of rowlist contains "Default" then exit repeat
		end repeat
		set defrow to item def of rowlist
		set nextrow to item (def + 1) of rowlist
		if selrow is defrow then
			select nextrow
		else
			select defrow
		end if
	end tell
end tell
return
~~~
<br>
<br>
<br>
