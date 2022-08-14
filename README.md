# code-cupl README

## Overview

This language extension provides support for Atmel's CUPL language, which is used to program their line of complex programmable logic devices (CPLDs). In addition to basic syntax highlighting, it includes a number of useful code snippets for laying out common language constructs.

Traditionally, CUPL programming has been done using Atmel's proprietary IDE, WinCUPL. Unfortunately, WinCUPL hasn't been updated since 2004, and it has numerous issues on modern versions of Windows (and isn't exactly a sophisticated IDE even when it *is* working). The goal of this extension is to provide a more modern and ergonomic environment for hobbyists who want to experiment with progammable logic devices.

WinCUPL is, in fact, just a graphical shell over some command-line utilities, which means it's actually straightforward to use VS Code as a development environment. The basic process works like this:

1. [Obtain a copy of WinCUPL](https://www.microchip.com/en-us/products/fpgas-and-plds/spld-cplds/pld-design-resources). The files are available on Atmel's website, but *installing* WinCUPL on a modern system can be something of an adventure, as it has an unfortunate habit of trashing your environment variables. My recommendation is to install WinCUPL inside of a virtual machine and then simply copy its entire folder onto the machine you want to actually use for development.
2. Create a VS Code project folder and add a `.pld` file to contain your CUPL code, and optionally an `.si` file with the same name to contain your simulator configuration. The headers of both files must match; you can use the `header` snippet to generate one.
3. Create a default build task (`F1` > `Tasks: Configure Default Build Task`) that invokes the CUPL compiler via the command line. My `tasks.json` looks something like this:

```json
{
    // See https://go.microsoft.com/fwlink/?LinkId=733558
    // for the documentation about the tasks.json format
    "version": "2.0.0",
    "tasks": [
        {
            "label": "cupl",
            "type": "shell",
            // Add 's' to this jumble of flags if you're using the simulator.
            "command": "cupl.exe -m1lxfjnabe -u D:\WinCUPL\Shared\Atmel.DL D:\MY_PROJECT\MY_FILENAME.pld",
            "problemMatcher": [],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```

> This example assumes that `cupl.exe` is on your `PATH`. Note that **it's important to use absolute file paths in the command line arguments**. If you don't, it may result in some output files not being generated correctly for certain devices.

The task above will allow you to invoke the CUPL compiler by pressing `Ctrl+Shift+B` within VS Code. You'll never have to use the WinCUPL IDE again!

## Simulation

The simulator, `csim.exe`, also runs on the command line, but getting it to do anything meaningful is tricky at best. It's easiest to run the simulator as part of the build by adding the `-s` flag to the arguments for `cupl.exe`.

To run simulations, you'll need a simulator input (`.si`) file with the same name as your `.pld` file. The [CUPL Programmer's Reference Guide](https://ece-classes.usc.edu/ee459/library/documents/CUPL_Reference.pdf) describes how to construct such files, in the "Simulator Reference" chapter. This extension provides syntax highlighting for this file format as well.

The result of the simulation will be placed in a simulator output (`.so`) file in your project directory after simulation completes.

## Release Notes

### 1.0.0

Initial release of code-cupl.