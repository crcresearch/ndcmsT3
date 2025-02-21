# Modules

Taken from the Modulefile manual page, "A typical modulefile is a simple bit of code that sets or adds entries to the PATH, MANPATH, or other environment variables." A Module is simply a way to easily organize and handle different pieces of software and different versions of the same software. This allows multiple versions of a popular package like Python to be available on the same system.

Looking to create your own modules? See `User Modules` for instructions.

------------------------------------------------------------------------

## Module Command

While using CRC systems, software which is not included within a standard install of Red Hat Enterprise Linux must be loaded through a module. Matlab for example, cannot be ran on a front end or compute node within the CRC without first loading the matlab module.

``` shell
matlab
matlab: command not found
```

To use matlab, you must use the "module" command first. Running the module command alone will give a list of available options.

``` shell
module
Modules Release 4.2.4 (2019-04-26)
Usage: module [options] [command] [args ...]

Loading / Unloading commands:
    add | load      modulefile [...]  Load modulefile(s)
    rm | unload     modulefile [...]  Remove modulefile(s)
    purge                             Unload all loaded modulefiles
    reload | refresh                  Unload then load all loaded modulefiles
    switch | swap   [mod1] mod2       Unload mod1 and load mod2

    < continues >
```

------------------------------------------------------------------------

### Searching Modules

To see all available modules, type

``` shell
module avail
```

To see if a specific module is available, type

``` shell
module avail XYZ
```

where XYZ is the name of the software you are searching for.

------------------------------------------------------------------------

### Module Categories

There are different categories in which the Modules are organized:

- system_modules
- deprecated_software
- restricted_software
- general_software
- development_tools_and_libraries.

!!! important
    Modules within the restricted_software category cannot be used without first receiving permission to access those pieces of software.
    
    The deprecated category contains modules which are out of date or no longer being supported after the next biannual outage. The date of the next biannual outage can be found on the [main page](../index.md) . These modules can still be used, however it is recommended to move your application to a newer version of the deprecated module.

------------------------------------------------------------------------

## Using Modules

After using "module avail", there will be a list of programs from which you can use. Let's say we need to use matlab, the "load" option to the module command can be used to bring matlab into our environment. Simply type:

``` shell
module load matlab
```

Now you have the default matlab loaded and ready to use by just typing

``` shell
matlab
```

If you need a specific version of a program and see it listed then you'd enter

``` shell
module load program_name/x.x
```

where x.x is the version number.

You can see which modules currently have loaded with

``` shell
module list
```

You can also remove modules from your environment that you don't need anymore with

``` shell
module unload program_name
```

Note that upon logging in, **by default**, you will not have any modules loaded with the exception of the `CRC_default` module.

------------------------------------------------------------------------

### Getting information about a module

To see a short description of the software within a module, use the whatis sub-command

``` shell
module whatis matlab
```

Some modules may also have a short informational help screen. View it with:

``` shell
module help matlab
```

------------------------------------------------------------------------

## User Created Modules

There are some cases where a module is desired for software which is installed into local user space. Local modules can coexist with CRC modules and being user space installed software, these modules will not become deprecated and removed over time by CRC administrators. Creating a local or private module can be straightforward once a piece of software is installed into user space.

!!! note
    For the rest of these instructions, it is assumed the software a module is intended for is already installed.

For more information on the module command itself, see `Module Command`.

------------------------------------------------------------------------

### Creating a Private Module

Private module files are stored within a directory whose path is prepended or appended to the module commands path. First create a directory where all private module files will be stored:

``` shell
$ mkdir ~/privatemodules
```

Next, for each software package a module file is desired, create a directory. For an example, we will create a private module for graphviz. Note that for every software package intended for a module, a directory should be created like this.

``` shell
$ mkdir ~/privatemodules/graphviz
```

Now, a module file can be created within the directory above. The module file should be named as the version of the software it is meant to load. Thus, continuing the graphviz example, a module file will be created titled 2.40.1:

``` shell
$ <your favorite text editor here> ~/privatemodules/graphviz/2.40.1
```

Nearly anyone of the module files administered by the CRC can be used as a template to create a private module. Below is an example for a private graphviz module file. Graphviz has been compiled locally within user AFS space on CRC systems, for software which is a precompiled binary, see `precompiled_binary` below:

    #%Module
    #  --------------------------------------------------------------------------  #

    proc ModulesHelp { } {
    puts stderr "Graphviz version 2.40.1"
    puts stderr "\nGraphviz is open source graph visualization software. It has several main graph layout"
    puts stderr "programs. It also has web and interactive graphical interfaces, and auxiliary tools, libraries,"
    puts stderr "and language bindings. Graphviz has many useful features for concrete diagrams, such as options for"
    puts stderr "colors, fonts, tabular node layouts, line styles, hyperlinks, and custom shapes."
    }

    if { [ module-info mode load ] } {
    puts stderr [ concat "Loaded shampton's version of module " [module-info name] ]
    }

    set graphviz_path "/afs/crc.nd.edu/user/letter/netid/intended_software"

    # Setting paths
    prepend-path  PATH              $graphviz_path/bin
    prepend-path  LD_LIBRARY_PATH   $graphviz_path/lib
    prepend-path  MANPATH           $graphviz_path/share/man

    # Cleaning up environment
    unset graphviz_path

    module-whatis "\nGraphviz is open source graph visualization software."

It is important to use absolute paths while creating module files. As seen above, a module file simply manipulates paths.

------------------------------------------------------------------------

### What if my Software is a precompiled Binary?

A module file for a precompiled binary software package is even easier to create than a module file for software compiled locally. Below is an example of a module file for the Rust programming language compiler and package manager, which typically come as precompiled binaries.

``` shell
#%Module
#  --------------------------------------------------------------------------  #

proc ModulesHelp { } {
puts stderr "The rust modulefile defines the default system paths"
puts stderr "and environment variables needed to use Rust and"
puts stderr "Cargo, Rust's package manager."
puts stderr "Type \"module load rust\" to load the default version of Rust."
}

if { [ module-info mode load ] } {
puts stderr [ concat "Loaded <your name>'s version of module " [module-info name] ]
}

prepend-path    PATH   /afs/crc.nd.edu/user/letter/netid/path/to/.cargo/bin

module-whatis "Rust is a systems programming language that runs blazingly fast, prevents segfaults, and guarantees thread safety. More information can be found at:\n\t\thttps://doc.rust-lang.org/book/second-edition/index.html"
```

------------------------------------------------------------------------

### Using Private Modules

In order for the module command to find the newly created private modules, the module path must be changed. This can be done with

``` shell
$ module use -a ~/privatemodules
```

The above command will only affect the current login shell. In order to always have access to private modules, add the above command to your shell configuration script.

- For BASH users (new user default as of May 2018), add the above command to the bottom of your ~/.bashrc file
- For cshell / tcsh users, add the above command to the bottom of your ~/.cshrc file.

Be sure to log out and log back in for the change to occur. Now, you can use the module commands (load, unload, avail, etc) on the private modules.

------------------------------------------------------------------------

### Different Versions of Software

Just like the CRC, private modules can be created for different versions of the same software. Simply create a new module file named after the version of software it is targeting and be sure the paths within the module file are correct. Thus you may get:

\$ module avail

``` shell
--------- /afs/crc.nd.edu/user/user/privatemodules ----------
graphviz/2.40.1  rust/1.19.0(default)  rust/1.25.0  sage/8.0
```

To create a default version of a module, create a .version file within the directory of the module which follows suit to below:

``` shell
#%Module
set ModulesVersion "1.19.0"
```

------------------------------------------------------------------------

### Considerations

When using module commands, the operation will occur to the first occurrence of the module the command finds. Thus, it may be a good idea to name a module something different from the CRC system modules.
