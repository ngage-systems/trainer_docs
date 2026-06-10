1. Install cursor or vs code

https://cursor.com/
https://code.visualstudio.com/

2. Install Remote SSH in Cursor/VSCode

3. Connect to a trainer
 a) cmd+shift+p
 b) search for and select Remote-SSH: Connect to host
 c) If the trainer you want to connect to is not on the list, select Add New SSH Host and enter [user]@[ip address or hostname of trainer] (reference ssh doc)
 d) go back to step a and this time select the device from the list

4. It may need to initialize which can take approximately 1 minute. Enter password (that was chosen when the trainer was initially provisioned) for the trainer.

5. Once connected, it may need to install some software on the trainer the first time you connect. There should be a sidebar with a button "Open Folder" and select the /home/lab folder. It will likely prompt you for the password again.

6. You will probably want 4 panes open: main coding window in the center, an agent tab on one side, and directory tree on the other, and terminal on the bottom. The exact layout can and will differ depending on the version of Cursor/VSCode.

7. In the directory tree, select the systems/ess folder. This will show a subdirectory for each "system" on this trainer.