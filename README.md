
# Table of Contents

1.  [Almos&rsquo;s Auto-Rice Bootstrapping Scripts (AARBS)](#orgd2d77fe)
    1.  [Installation:](#org2c6588f)
    2.  [What is AARBS?](#org74af308)
    3.  [Why are you using Emacs?](#org772a211)
    4.  [Customization](#org9a11da9)
        1.  [The `progs.csv` list](#orgdb61772)
        2.  [The script itself](#org4355d30)


<a id="orgd2d77fe"></a>

# Almos&rsquo;s Auto-Rice Bootstrapping Scripts (AARBS)


<a id="org2c6588f"></a>

## Installation:

On an Arch-based distribution as root, run the following:

    curl -LO larbs.xyz/larbs.sh
    sh larbs.sh

That&rsquo;s it.


<a id="org74af308"></a>

## What is AARBS?

AARB&rsquo;s is my auto install script based mostly on Luke Smith&rsquo;s LARBS script.
The main difference between the two is that he uses Vim, and I use Emacs,
so much of my build relies heavily on emacs packages and functionalities.


<a id="org772a211"></a>

## Why are you using Emacs?

Because I can use it as an IDE, a general purpose document editor,
an image and PDF viewer, an email client, a git client, a tiling window manager&#x2026;
the list just goes on and on.
It may not be the most UNIX-y thing in the world if you&rsquo;re really tied down in
definitions, but that is just silly.


<a id="org9a11da9"></a>

## Customization

By default, AARBS uses the programs [here in
progs.csv](progs.csv) and installs [my
dotfiles repo (voidrice) here](https://github.com/lukesmithxyz/voidrice), but you can easily change this by
either modifying the default variables at the beginning of the script or
giving the script one of these options:

-   `-r`: custom dotfiles repository (URL)
-   `-p`: custom programs list/dependencies (local file or URL)
-   `-a`: a custom AUR helper (must be able to install with `-S` unless
    you change the relevant line in the script


<a id="orgdb61772"></a>

### The `progs.csv` list

AARBS will parse the given programs list and install all given programs.
Note that the programs file must be a three column `.csv`.

The first column is a &ldquo;tag&rdquo; that determines how the program is
installed, &ldquo;&rdquo; (blank) for the main repository, `A` for via the AUR or
`G` if the program is a git repository that is meant to be
=make && sudo make install=ed.

The second column is the name of the program in the repository, or the
link to the git repository, and the third column is a description
(should be a verb phrase) that describes the program. During
installation, AARBS will print out this information in a grammatical
sentence. It also doubles as documentation for people who read the CSV
and want to install my dotfiles manually.

Depending on your own build, you may want to tactically order the
programs in your programs file. AARBS will install from the top to the
bottom.

If you include commas in your program descriptions, be sure to include
double quotes around the whole description to ensure correct parsing.


<a id="org4355d30"></a>

### The script itself

The script is extensively divided into functions for easier readability
and trouble-shooting. Most everything should be self-explanatory.

The main work is done by the `installationloop` function, which iterates
through the programs file and determines based on the tag of each
program, which commands to run to install it. You can easily add new
methods of installations and tags as well.

Note that programs from the AUR can only be built by a non-root user.
What AARBS does to bypass this by default is to temporarily allow the
newly created user to use `sudo` without a password (so the user won&rsquo;t
be prompted for a password multiple times in installation). This is done
ad-hocly, but effectively with the `newperms` function. At the end of
installation, `newperms` removes those settings, giving the user the
ability to run only several basic sudo commands without a password
(`shutdown`, `reboot`, `pacman -Syu`).

