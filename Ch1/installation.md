# Installation

First things first, let's install Gear. We will download Gear via `cog`. This is the easiest way to get started.

> Note: If you, for some reason, prefer not to use `cog`. Then you may visit the **INSERT LINK HERE** page to view additional ways to install Gear. If you wish.

The following steps install the latest stable version of the Gear compiler. This documentation is updated incase of any major changes. The output might differ slightly between versions because Gear may update or change it's outputs and errors. However, all the code examples you find here will work with all future Gear versions.

> **Command Line Notation**<br>
In this chapter and throughout the book, we’ll show some commands used in the terminal. Lines that you should enter in a terminal all start with `$`. You don’t need to type the `$` character; it’s the command line prompt shown to indicate the start of each command. Lines that don’t start with `$` typically show the output of the previous command. Additionally, PowerShell-specific examples will use `>` rather than `$`.

## Installing **cog** on Linux or MacOS

If you’re using Linux or macOS, open a terminal and enter the following command:

```bash
$ curl --proto '=https' --tlsv1.2 https://sh.DOMAINHERE.TLD -sSf | sh
```

The command downloads a script and starts the installation of the **cog** tool, which installs the latest stable version of Gear. You might be prompted for your password. If the install is successful, the following line will appear:

```
Cog and Gear has been installed!
```

A linker is required to use Gear. Gear uses `qbe` as it's compiler backend instead of `llvm` meaning it requires a tool for linking objects together. To install one on MacOS run:

```bash
$ xcode-select --install
```
Linux users can install typical tools such as `gcc` or `clang` which contain linkers. On Ubuntu, you may install the `build-essential` package.

## Installing **cog** on Windows

On Windows, go to https://www.DOMAINHERE.TLD/install and follow the instructions on that page. 

At some point during the installation you will be required to install a linker for your project.

To acquire the build tools, you’ll need to install **Visual Studio 2022**. When asked which workloads to install, include:

- “Desktop Development with C++”
- The Windows 10 or 11 SDK
- The English language pack component, along with any other language pack of your choosing

The rest of this book uses commands that work in both `cmd.exe` and **PowerShell**. However, we recommend using **PowerShell**.

### Troubleshooting
To check whether you have Gear installed correctly, open a terminal and enter this line:

```bash
$ cog --version
```

You should see the version number for the latest stable version of cog and Gear that has been released like so:

```
cog x.y.z
gear x.y.z
```

If you see this information, you have installed Gear successfully! If you don’t see this information, check that Gear is in your %PATH% system variable as follows.

In Windows CMD, use:

```cmd
> echo %PATH%
```

In PowerShell, use:

```psh
> echo $env:Path
```

In Linux and macOS, use:

```bash
$ echo $PATH
```

If that's all correct, and you are still struggling with installation. Check out our community servers and forums for additional help.

## Updating and Uninstalling
Once Gear is installed via cog, updating to a newly released version is easy. From your terminal, run the following update script:

```bash
$ cog install cog@latest
```

To uninstall Gear and Cog, run the following uninstall script from your shell:

```bash
$ cog uninstall cog
```

## Local Documentation
The installation of Gear also includes a local copy of the documentation so that you can read it offline. Run `cog docs` to open the local documentation in your browser.

You can also use `cog docs <option>` to load a specific page.