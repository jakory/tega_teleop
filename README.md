# tega\_teleop
A python rosnode for teleoperating the Tega robot and an opal tablet. Creates a Qt GUI with buttons for triggering speech, lookats, and animations on the robot, as well as commands to send to an opal tablet.

## Configure and Run
On startup, this python node will try to connect to roscore. If roscore is not running, the program will exit. 

The program will also try to read in the interaction script file listed in the "tega\_teleop\_config.txt" file. There is an example interaction script file located in src/. This script file should list, in order, the filenames of all audio files that the robot will be commanded to play back during the interaction. The second column should list the words that shown on the speech buttons in the GUI (for example, the filename may be "robot\_line\_01.wav" and the words to show may be "Hi, I'm Tega"). This allows the program to be used for different interactions and different sets of audio files without having to change the python code for creating buttons.

### More on scripts
The most basic script would be a single list of filenames with a single list of button labels. The robot is commanded to say one thing, then the next thing, and so on--always the same things, and always in the same order.

In a slightly more complicated script, the teleoperator may need to decide which of several different things the robot should say next. For example, the teleoperator might have to choose to have the robot say "Awesome!", "Hmm...", or "Aw, are you sure?" in response to something a child says during the interaction. That is three different options.

This interface can deal with that level of complication. All you need to do is list the maximum number of different options the teleoperator will have (for example, 3) in the config file: set "options" to "3". Then, on each line of your script file, list the filenames of the teleoperator's speech options and their button labels in tab-delimited format: \[filename1 label1 filename2 label2 etc.\]. When the script is loaded, whenever there are multiple options, these will be shown on buttons simultaneously so the teleoperator can choose which to trigger. You probably don't want to list more than 2-4 options for any line of speech, since the teleoperator *does* have to keep track of when to use each one. Increased cognitive load and all.

There is the option of including a set of "unchanging script" buttons to trigger speech. This is useful if, for example, the robot could always have the option of saying "Mmhm" and "Awesome!" regardless of the rest of the script, and if you don't want to include these phrases as options on every single line of your main script file. Following the example\_unchanging\_script.txt file, make a list with one column of audio filenames and one tab-delimited column of button labels. You probably shouldn't list more than 3-5 of these "always there" speech buttons, since otherwise the interface may start to look clunky with too many buttons.

### Opal tablet communication
Commands to the opal tablet are sent over a rosbridge\_server websocket connection. For communication with the tablet to occur, you need to have the rosbridge\_server running, using the following command:

roslaunch rosbridge\_server rosbridge\_websocket.launch

You will also need to ensure that the opal tablet's config file lists the IP address or hostname of the machine running roscore.

## ROS messages
### SAR Opal messages
The program publishes "/[sar\_opal\_msgs](https://github.com/personal-robots/sar_opal_msgs "/sar_opal_msgs")/OpalCommand" to the ROS topic "opal\_tablet\_command". See [/sar\_opal\_msgs](https://github.com/personal-robots/sar_opal_msgs "/sar_opal_msgs") for more info.

### R1D1 messages
The program publishes "/[r1d1\_msgs](https://github.com/personal-robots/r1d1_msgs "/r1d1_msgs")/TegaAction" to the ROS topic "tega\_action". See "/[r1d1\_msgs](https://github.com/personal-robots/r1d1_msgs "/r1d1_msgs") for more info. 

## TODO
- Put all interaction scripts in a folder, add dropdown menu to interface so the user can select which script to load at runtime

