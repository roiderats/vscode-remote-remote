# vscode-remote-remote
Remotely command VSCode Remote extension to open files from any terminal

Open files to VSCode with remote extension from terminals outside VSCode by 
running a listener inside VSCode terminal
Usage: 
[path_to_your/]coder [-w|--wait] filename
    to request background-script running inside VSCode terminal to open file

[path_to_your/]coder -quit
    kill the listener

Installation:
    place this line into your $HOME/.bashrc:
    if ! pgrep --pidfile "/tmp/vscode_remote_opener.$USER.pid" \ 
            >/dev/null 2>&1; then
        find /tmp -maxdepth 1 -user $USER -name "vscode-ipc*.sock"| grep -q sock
        if [ $? -eq 0 ]; then
            [path_to_your/]coder -listener &
        fi
    fi

There is an assumption that background process starting relies: VSCode starts
a bash-shell at launch. This seems to be true by default in VSCode version 
1.36.1(non-Insiders build) but your mileage may vary. Also if you close that
terminal, background process likely dies. Opening new terminal there should
then launch background process again.

TODO: 
- can we change this script name to "code" and correctly guess if file is to
be opened to remote or local VSCode? Env.var DISPLAY is set/unset is not 
enough. Tip: 'which -a code' finds all from PATH
version: 2019-08-02.2
