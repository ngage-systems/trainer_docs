The following is instructions on how to use agentic coding to modify or create a task. This is not recommended for developing new systems (e.g., Search or Match-to-sample), but rather to work within existing systems to create or modify tasks within a system.

Preparation

1. Install cursor or vs code. You will likely need a paid account.

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

8. Get the agentic coding folder. In the terminal, run this line

wget -qO- https://github.com/ngage-systems/trainer_docs/archive/refs/heads/main.tar.gz | tar -xz -C ~ --strip-components=2 trainer_docs-main/docs/agentic-coding

this will get the contents of that directory and put it in your home directory

9. In the agent window, switch to "Plan" mode. It's generally good practice to work with the agent to develop and review a plan and then, once satisfied, switch to Agent mode to enact the plan.

Coding

9. Now you're ready to tell the agent what you want to do. I would use a prompt like so:

"i want to modify the match_to_sample > fractal > random variant to allow me to specify the opacity of the distractor with a new dropdown option.  for reference, this is done in the colormatch > noDistractor variant already.

reference ~/agentic-coding directory"

That last line is crucial for it to have the information it needs to code and test a task.

10. When it's done developing a plan (it may ask you some clarifying questions first), review the plan to make sure it understood correctly what you want to do. If it's not right, remain in Plan mode and tell the agent how to change the plan.

11. Once satisfied, click "Build" or switch to Agent mode and tell it to implement the plan.

12. It will likely request permission multiple times as it edits files. Choose "Run" on each one.

13. When it finishes, it should give you a review of what it did. Go through this and make sure it stayed within the boundaries of what you defined.

Testing

14. If it doesn't work as specified, it is often best to switch to "Ask" mode and explain what's wrong and have the agent explain back to you it's understanding of the problem and a solution. You can then switch to agent mode and have it implement the fix(es).

15. After confirming that it's working correctly, we also want to verify it's saving the variables we want in the way we want. 