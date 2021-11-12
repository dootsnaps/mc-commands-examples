# How to use MyCommand to create a custom command shortcut:

1. Decide what you want to call your command. This will be _yourCommandName_.

2. Run `/mycmd check yourCommandName` to see if someone has already made a command with _yourCommandName_. It should tell you "No command found with this ID." 

3. Now we need to create the command. Run `/mycmd-edit yourCommandName type RUN_COMMAND`

4. Next define the actual command you want to type to activate your shortcut. For now we will just use `/yourCommandName`. To do this, run `/mycmd-edit yourCommandName command /yourCommandName`

5. Now we can start adding the commands that will run when you run `/yourCommandName`. We will use _yourFirstCommand_ to symbolize the entire command that you would normally type into the chat. To add a command, run `/mycmd-edit yourCommandName runcmd add yourFirstCommand`

6. Repeat step 5 until all your commands are added (e.g. _yourFirstCommand_, _yourSecondCommand_ etc...)

7. Run your command to test it by simply running `/yourCommandName`

## Example - Create a spherical radius 100 brush that replaces all grass, snow, and dirt blocks on a slope between 50 and 90 degrees with stone:

Normally to do this, you would have to manually type (or copy-paste) the required FAWE commands into the chat to bind the brush that you want to the tool that you are holding. In this case, the commands are:

`//brush sphere stone 100`

`//mask [grass_block,snow_block,dirt]&[#angle[50d][90d]]`

Typing all that every time you re-log onto the server is a pain! Let's create a shortcut using the above method that will run both those commands for us with a simple alias:

1. Let's call this "brushstone"

2. `/mycmd check brushstone` says "No command found with this ID." - Good!

3. `/mycmd-edit brushstone type RUN_COMMAND` sets the command to "run command" type.

4. `/mycmd-edit brushstone command /brushstone` binds the command to `/brushstone`.

5. `/mycmd-edit brushstone runcmd add //brush sphere stone 100` runs our first FAWE command.

6. `/mycmd-edit brushstone runcmd add //mask [grass_block,snow_block,dirt]&[#angle[50d][90d]]` runs our second FAWE command.

7. Holding a tool, in my case a golden sword, run `/brushstone` to test. It will give you the output of the FAWE commands:

![Image](https://i.imgur.com/gj3stTT.png)

And when we right-click some terrain while holding our golden sword:

![Image](https://i.imgur.com/ZmP1naX.png)

Only the steep slopes of the terrain have been replaced with stone! Exactly what we wanted.



### Bonus: Running `/mycmd check brushstone` now gives us an output telling us the command's settings:

![Image](https://i.imgur.com/CQQB3ZR.png)

Notice the last line: `Registered (Real Command): false`. This means that when you type the command in chat to run it, the server will yell at you about an "Unknown or incomplete command" while you are typing it into the chat, and will not give a tab-complete for it. However, the command will still run, don't worry! 

![Image](https://i.imgur.com/fUoZfvV.png)

If we want, to make the command "registered" we can run:

`/mycmd-edit brushstone register true`

This will register the command, but unfortunately it won't be effective until the next server restart. Hence, don't expect this to actually work right away!

## Advanced: Requiring Arguments

Letting us run a list of commands with a shortcut is already great, but what if we could also pass arguments to those commands, such as a desired brush radius? Here's how to do it:

1. `/mycmd-edit yourCommandName require_all_arguments true`
2. Replace the arguments you want to pass in the target command with the variables `$arg1`, `$arg2`, etc. Our goal is to change the command from, for example, `/thisIsACommand 50 700 20` to `/thisIsACommand $arg1 700 $arg2`, which would require the user to run `/yourCommandName <arg1> <arg2>` to properly fill those variables.

If you are adding the command as a new `runcmd`, use `/mycmd-edit yourCommmandName runcmd add /thisIsACommand $arg1 700 $arg2`. 

If you want to edit a previously-added command, use `/mycmd-edit yourCommandName runcmd <index> /thisIsACommand $arg1 700 $arg2`, where `<index>` is replaced by the number of the command which you want to edit, starting from 1. So if the command we want to edit is the second on the list, we would use `/mycmd-edit yourCommandName runcmd 2 /thisIsACommand $arg1 700 $arg2`.

3. Now when the user runs, for example, `/yourCommandName 30 70`, the `/thisIsACommand` sub-command on `yourCommandName`'s `runcmd` list would be run as `/thisIsACommand 30 700 70`, since we filled in 30 and 70 for `$arg1` and `$arg2`.

**Example using our stone brush:**

1. `/mycmd-edit brushstone require_all_arguments true` makes sure the user inputs the argument we want
2. `/mycmd-edit brushstone runcmd 1 //brush sphere stone $arg1` replaces the radius (previously 100) with the first argument typed by the user
3. Now when we run /brushstone we will need to pass the radius we want: `/brushstone 50` will give us the same brush as before, but now it has a radius of 50 blocks!

Note: if you don't set `require_all_arguments` to `true`, in this particular example it will still work. Why? Because FAWE has a default brush radius of 5 blocks when a radius is not specified by the user. So in this case it's not strictly necesarry. As long as 1) you are only passing a single argument, 2) that argument has a default value in the target command, and 3) the target command doesn't have additional arguments after the one you are setting, you can leave `require_all_arguments` set to `false`. 
