Below is my personal notes regarding the Angular Web framework (namely, Angular 8).

# Getting Started

First, we need `Node.js`. `Node.js`, is used by the Angular command line interface (CLI) behind the scenes to bundle and optimize our project. We will also use the node package manager (`npm`) to manage the different dependencies that Angular has (e.g., the Angular framework itself and other libraries that the framework uses).

To install `Node.js` visit the `Node.js` website and download the long term support (lts) release. In my case this is `12.14.1`.

This will install a tarball in your `~/Downloads` folder with a name approximately similar to `node-v12.14.1-linux-x64.tar.xz`. To be able to call the `Node.js` interpreter and run `Node.js` code from [Bash](https://www.gnu.org/software/bash/manual/html_node/What-is-Bash_003f.html) we must add the `Node.js` interpreter executable to our `$PATH`. The `$PATH` variable specifies the directories in which executable programs are located on the machine that can be started without knowing and typing the whole path to the file on the command line. To do this:

1. Unpack and decompress `node-v12.14.1-linux-x64.tar.xz`:

    `tar xf node-v12.14.1-linux-x64.tar.xz`

2. Now you will see `node-v12.14.1-linux-x64` in your current directory. Navigate to `/usr/local/lib/` and make `nodejs` directory:

    `cd /usr/local/lib/ && mkdir nodejs`

3. Move `node-v12.14.1-linux-x64` to `/usr/local/lib/`:

    `mv ~/Downloads/node-v12.14.1-linux-x64 .`

4. Export `/usr/local/lib/nodejs/bin` which contains `node` executable to `$PATH` in `~/.bashrc`:

    append `export PATH=$PATH:/usr/local/lib/nodejs/node-v12.14.1-linux-x64/bin` to `~/.bash_rc`

**Note 1:** it is recommended to append after existing `$PATH`.

**Note 2:** `~/.bash_rc` is executed by `bash` on **non-login** shells.
