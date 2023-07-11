# Shortcuts
# CTRL+A -> Start of line
# CTRL+E -> End of line
# CTRL+C -> Interrupt process
# CTRL+Z -> Suspend process

# Repeat last command
!!
# Repeat last command where "string" occurred       
!string
# Last argument of last command
!$          

# Create alias
echo "alias mycommand='echo test'" >> ~/.bashrc 
# Command substitution
echo $(pwd)
# Inline loop
for i in *.avi; do ffmpeg -i "$i" "{i%.*}.mp4"; done
# Return to last working dir
cd -
# Shortcut for `>/dev/null 2>&1`: merge stdout+stderr and redirect to /dev/null
&> /dev/null
# A good usecase
detach ()
{
    ("$@" &> /dev/null & disown)
}
detach firefox
# Start script from line number 5
tail -n +5 script.sh | bash

