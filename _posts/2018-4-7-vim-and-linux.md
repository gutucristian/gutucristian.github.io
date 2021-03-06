My vim dotfile notes! Below are options and commands I personally found useful through experience. Of course, this list is only getting started and will constantly be updated as time goes on.

In `~/.vimrc` file:
- use `set tabstop = 2` to set tabs to 2 spaces.
- use `set shiftwidth=2` to set shift width or indent amount to 2 spaces.

**To indent/un-indent code bock:** first select your desired code block using `Shift-V`. This activates visual mode and selects entire line(s). Use `Up` or `Down` arrow to make you selection and then type `Shift->` (that is `Shift` followed by `>`) to indent and `Shift-<` to unindent code block.

**To comment code block:** first type `Ctrl-V` to select area. Next, type `Shift-I` to insert character(s) and type `//` then hit `Esc`.

**To un-comment selection:** Type `Ctrl-V` to select area then type `d` to delete selected area.

`:retab` to retab tabs according to `tabstop` and `shiftwidth` rules.

`:nohl` to turn off highlight (usually after you are done searching for some pattern)

`/some_string` to search for some pattern (use `n` to go to next occurence)

Type `cw` to change current word.

# Linux commands

Linux `find` command looks for a file in some directory. For example: `find "." -name "callno.h"`: will look for file named `callno.h` in current directory and all subdirectories of current directory.

Linux `grep` command searches for string pattern in file(s). For example: `grep -rnw "." -name "thread_exit"` will recursively `-r` search for all occurences of pattern match in directory `.` (current directory). Because we passed the `-r` flag, the search space includes all subdirectories of this directory. Ultimately, running this command will search for the use of `"thread_exit"` and print line number(s) `-n` of said occurence(s). Importantly, it will make sure that entire pattern matches as given by the use of `-w`.

For more `grep` command information [read here](https://stackoverflow.com/questions/16956810/how-do-i-find-all-files-containing-specific-text-on-linux).

____________________________________________________________

GNU `tar` is an archiving program designed to store multiple files in a single file (an archive) and to manipulate such files. File compression tools (like gzip and bzip2) compress single files (not groups of files). So, we use `tar` to group files into one monolithic file and then use a file compression tool like `gzip` to compress the file and save memory. In Windows, this is equivalent to `WinRAR` and `WinZip`. Read more [here](https://stackoverflow.com/questions/295860/why-do-people-use-tarballs).

To unpack a tarball (unpacks tar files compressed with gzip as denoted by `.tar.gz` file extension):

`tar -xvzf filename`

To make a new tarball `fred.tar.gz` from a directory `fred` (creates a `.tar.gz` file):

`tar -cvzf fred.tar.gz fred`

`tar` option parameter meanings:

- **c**	 create an archive
- **f**  filename	the name of the archive file
- **v**	 verbose: tell me what's going on
- **x**	 extract from an archive
- **z**	 put the archive through gzip

Read more [here](http://computing.help.inf.ed.ac.uk/FAQ/whats-tarball-or-how-do-i-unpack-or-create-tgz-or-targz-file) and [here](http://wiki.linuxquestions.org/wiki/Packing_and_Unpacking_Files).

____________________________________________________________

## Updating system `PATH` variable

### On MacOS

In `~/.zprofile` add additional paths you would like to be in your system `$PATH`. Example

```
export PATH="/usr/local/mysql/bin:$PATH"
export PATH="/usr/local/kafka_2.12-2.5.0/bin:$PATH"
```

### On Linux

Often I have issues setting up environment variables and/or updating path variables. As a general rule, when adding some directory to `$PATH` it's good idea not to overwrite previous value, just append desired directory (e.g., `$HOME/bin`), in your `~/.bashrc` add at the end line: 

`export PATH="/usr/lib/jvm/java-8-openjdk-amd64/jre"` (this path is just an example, use whatever you need)

This sets the `$PATH` permanently for all future bash sessions. To set it permanently, and system wide (all users, all processes) add set variable in `/etc/environment`:

`sudo vim -H /etc/environment`

This file only accepts variable assignments like:

`VARNAME="my value"` (do not use `export` here)

Read more [here](https://askubuntu.com/questions/58814/how-do-i-add-environment-variables) and [here](https://askubuntu.com/questions/121073/why-bash-profile-is-not-getting-sourced-when-opening-a-terminal).

____________________________________________________________

`~/.profile` vs. `~/.bashrc` vs. `~/.bash_profile`

When bash is invoked as an interactive login shell (e.g., connecting to machine via ssh) it first reads and executes commands from the file `/etc/profile`, if that file exists. After reading that file, it looks for `~/.bash_profile`, `~/.bash_login`, and `~/.profile`, in that order, and reads and executes commands from the **first** one that exists and is readable. 

When an interactive shell that is not a login shell (e.g., terminal, xterm) is started, bash reads and executes commands from `~/.bashrc`, if that file exists (on MacOS with zsh this is `.zshrc`)

`~/.profile` is the place to put stuff that aplies to your whole session, such as programs you want to start when you log in and environmnent variable definitions.

`~/.bashrc` is the place to put stuff that applies only to bash itself, such as alias and function definitions, shell options, and prompt settings.

`~/.bash_profile` can be used instead of `~/.profile`, but it is read by bash only, not by any other shell (on MacOS with zsh this is `.zprofile`) ([1](https://superuser.com/questions/183870/difference-between-bashrc-and-bash-profile))

Most of the time you don’t want to maintain two separate config files for login and non-login shells — when you set a PATH, you want it to apply to both. A common practice is to source `~/.bashrc` from `~/.profile`. This way you can put `PATH` and common settings in `.bashrc` once and source them wherever you need ([1](http://www.joshstaiger.org/archives/2005/07/bash_profile_vs.html), [2](https://askubuntu.com/questions/432508/why-does-ubuntus-default-profile-source-bashrc), [3](https://askubuntu.com/questions/121073/why-bash-profile-is-not-getting-sourced-when-opening-a-terminal)).

Good [read](http://mywiki.wooledge.org/DotFiles) on configuring your login sessions with for file.

More on choosing between `.bashrc`, `.profile`, `.bash_profile` [here](https://superuser.com/questions/789448/choosing-between-bashrc-profile-bash-profile-etc).

To reload `~/.bashrc` without logging out and back in run `source ~/.bashrc` or `. ~/.bashrc`.

Sources:
- https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html
- https://askubuntu.com/questions/60218/how-to-add-a-directory-to-the-path
- https://stackoverflow.com/questions/56784894/macos-catalina-10-15beta-why-is-bash-profile-not-sourced-by-my-shell

____________________________________________________________

Understanding the job control commands in Linux. A job is a process that the shell manages. Each job is assigned a seuqential id. Beacuse a job is a process, each job has an associarted process id (PID) which is given out by the OS. There are three type of job statuses:
1. **Foreground**: when you enter a command in the terminal the command blocks (i.e., occupies) that terminal window until it complestes.
2. **Background**: when you enter an ampersand (&) symbol at the end of a command line, the command runs without blocking the terminal window. The shell prompt is displayed immediately adter you press the return key.
3. **Stopped**: if you press`Ctrl+Z` for a foreground job, the job stops running. Note: it does not terminate, instead it pauses exectution.

Commands:

- `jobs`: lists all jobs

- `bg %n`: sends current or specified job to background where `n` is the job id (not PID!)

- `fg %n`: brings current or specified job to foreground where `n` is the job id

- `Ctrl+Z`: stops foreground job and places it in the background as a _stoped_ job.

To open a terminal session in vim type `:term`. To switch between the vim and terminal session press `Ctrl+W` ([see](https://stackoverflow.com/questions/1236563/how-do-i-run-a-terminal-inside-of-vim)).
