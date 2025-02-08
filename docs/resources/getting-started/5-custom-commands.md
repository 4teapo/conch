# Custom Commands

After setting up permissions, you'll probably need to setup commands. By default, Conch only comes with logging commands and the license. Setting up your own commands is incredibly easy though, on both the server and client.

Let's create a simple `kick` command on the server to kick users out of the server. We can use `conch.register` to register a new command. This accepts a name, and then a table containing details about the command. We can add this to a script that runs whenever the server starts.

```luau
conch.register("kick", {

})
```

You'll need to pass the arguments that you want to pass to the command, the permissiosn necessary to execute the command and the callback to run when executing the command.

```luau
conch.register("kick", {
	permissions = { "kick-player" },
	arguments = function()
		return conch.args.player(), conch.args.string()
	end,

	callback = function(player, reason) -- typechecked!
		player:Kick(reason)
	end
})
```

We can also optionally add information to what the arguments and command actually entails by providing a description to the command and/or adding names and descriptions to each of the arguments!

```luau
conch.register("kick", {
	description = "Kicks the given player from the server",
	permissions = { "kick-player" },
	arguments = function()
		return
			conch.args.player("player", "The player that should be kicked from the server"),
			conch.args.string("reason", "The reason why the player has been kicked from the user.")
	end,

	callback = function(player, reason) -- typechecked!
		player:Kick(reason)
	end
})
```

With this setup, any player that has the `kick-player` permission will be able to run the `kick` command.

It's possible to also register commands quickly, by using the `conch.register_quick` function. This removes the ability to specify arguments which are useful for analysis, but is useful for when you need to quickly make a function accessible. You can pass the permissions required after the function.

```luau
conch.register_quick("kick", function(player_name, reason)
	local player = Players:FindFirstChild(player_name)
	if not player then error(`{player_name} is not a real player`) end
	player:Kick(reason)
end, "kick-player")
```
