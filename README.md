Overload – customize your commands
==================================

Overview
--------

Overload started out with three goals:

  * add better default behavior to commands that have poor default behavior,
  * enhance existing options/flags for commands, and
  * add additional functionality to existing commands in a way that is more
    natural to use.

It can be called directly – `overload svn ls` – or configured in your .bashrc
file. It can also be setup using symbolic links, but I generally discourage
that now.


Defining commands
-----------------

The easiest way to define commands is by creating a .overloadrc file in your
home directory. In this file, you should define bash functions that match the
underscorized verison of command you'd like to match. For example,

    svn_add() {
    	if [ -z "$1" ]; then
    		ARGS=$(svn st | awk '/^\?/{print $2}' | xargs)
    		if [ -n "$ARGS" ]; then
    			$original_command add $ARGS
    		fi
    	else
    		$original_command add "$@"
    	fi
    }

I've provided one of my own .overloadrc files to show some examples. For example, I have defined

  * svn us – runs svn up and status
  * svn commit – I force an svn up before every commit
  * svn ct – give it a ticket number, and it commits with the message "Completed Ticket #<TICKET NUMBER>"

Currently, I assume bash.


Configuring commands to always use overload
-------------------------------------------

After adding overload to your path, you can simply add

    source overload svn hg

To the bottom of your .bashrc (or equivalent.) For the .overloadrc I shared, I
only overloaded 'svn' and 'hg', so those are the only two I have configured in
my .bashrc.
