# How to use MyCommand to create a custom command shortcut:

1. Decide what you want to call your command. This will be _yourCommandName_.

2. Run `/mycmd check yourCommandName` to see if someone has already made a command with _yourCommandName_. It should tell you "No command found with this ID." 

3. Now we need to create the command. Run `/mycmd-edit yourCommandName type RUN_COMMAND`

4. Next define the actual command you want to type to activate your shortcut. For now we will just use `/yourCommandName`. To do this, run `/mycmd-edit yourCommandName command /yourCommandName`

5. Now we can start adding the commands that will run when you run `/yourCommandName`. We will use _yourFirstCommand_ to symbolize the entire command that you would normally type into the chat. To add a command, run `/mycmd-edit yourCommandName runcmd add yourFirstCommand`

6. Repeat step 5 until all your commands are added (e.g. _yourFirstCommand_, _yourSecondCommand_ etc...)

7. Run your command to test it by simply running `/yourCommandName`

## Example - Create a spherical radius 100 brush that replaces all grass, snow, and dirt blocks on a slope between 50 and 90 degrees with stone:

1. Let's call this "brushstone"

2. `/mycmd check brushstone` says "No command found with this ID." - Good!

3. `/mycmd-edit brushstone type RUN_COMMAND`

4. `/mycmd-edit brushstone command /brushstone`

5. `/mycmd-edit brushstone runcmd add //brush sphere stone 100` //first command to run

6. `/mycmd-edit brushstone runcmd add //mask [grass_block,snow_block,dirt]&[#angle[50d][90d]]`

7. holding a tool, run `/brushstone` to test!
