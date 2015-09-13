Install and Config PostgreSQL on OSX
====================================

## Install PostgreSQL with Homebrew

Update Homebrew:
```terminal
✔ brew update
```

Check that Homebrew is properly installed and configured:
```terminal
✔ brew doctor
```

Go ahead and install the `postgresql` formula:
```terminal
✔ brew install postgresql
```


## Post-install configuration

Create a `LaunchAgents` directory if it didn't exist already:
```terminal
✔ mkdir -p ~/Library/LaunchAgents
```

Create a symbolic link to the `postgresql` plist file, so `launchd` starts
`postgresql` at login:
```terminal
✔ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
```

Load the `postgresql` plist into the launch control service, so `postgresql`
starts up automatically at login:
```terminal
✔ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

Reboot your system and make sure `psql` points at `/usr/local/bin/psql`:
```terminal
✔ which psql
```


## Create and config a database

There's at least two different approaches, here:
 1. [Create a default database based on you username](#create-default-database)
 2. [Create and config an user for PostgreSQL](#create-and-config-user)


### <a id="create-default-database"></a>Create a default database based on you username ##

Make sure to use backticks when typing the command:
```terminal
✔ createdb `whoami`
```


### <a id="create-and-config-user"></a>Create and config an user for PostgreSQL ##

Create a user in `postgresql`:

```terminal
✔ createuser --createdb --login --pwprompt type-username-here
  Enter password for new role:
  Enter it again:
```

1. A user `type-username-here` is created with the ability to create databases.
2. It will issue a prompt for the password of the new superuser.
3. The database creation is automatically done by Rails.

Check [createuser docs] for further info on the createuser command.

[createuser docs]: http://www.postgresql.org/docs/9.4/static/app-createuser.html


## Drop a created user for PostgreSQL

**NOTE: Don't do this if you plan to execute the app. Only follow these steps
when you are sure you are done with the app.**

Drop the databases created by the user. For example, if you have a `example-app`
in Rails, you probably need to drop the `development` and `test` databases (not
quite sure if *it's safe* to delete the `production` database):
```terminal
✔ dropdb example-app_development
✔ dropdb example-app_test
```

Drop the user:
```terminal
✔ dropuser type-username-here
```

Check [dropdb docs] and [dropuser docs] for further info on both commands.

[dropdb docs]: http://www.postgresql.org/docs/9.4/static/app-dropdb.html
[dropuser docs]: http://www.postgresql.org/docs/9.4/static/app-dropuser.html

## Additional Info

 - [3 Battle-Tested Ways to Install PostgreSQL](https://www.codefellows.org/blog/three-battle-tested-ways-to-install-postgresql)
