<div align="center">

# Dotfiles & Developer Environment

Dotfiles & Developer Environment. Supports Ubuntu 18.04 + macOS Catalina + Windows 10 w/ WSL 🚀

</div>

## Table of Contents

- [Setup Windows 10 with WSL 2](#windows-10-with-wsl2)
- [Ubuntu 18.04 or macOS Catalina](#ubuntu-1804-or-macos-catalina)
  - [Installation](#installation)

## Windows 10 with WSL2

<details>
<summary>Expand for Windows Setup</summary>

### Enable WSL

1. To enable WSL. Inside Powershell as an Administrator (right click windows symbol), type:

   ```
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   ```

2. Also enable ‘Virtual Machine Platform’. Inside Powershell as an Administrator (right click windows symbol), type:

   ```
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```

3. Restart system just to be sure that everything worked.

### Register for Windows Insider

1. Click the Start Menu > Settings > Update & Security > Windows Insider Programme and then click Get Started.

2. Once you have registered, choose the `Slow` Ring

3. Now, click Windows Update and then click `Check for Updates`

4. Once you have installed the update, restart you system for the changes to take effect

### Installing Ubuntu Shell

1. Install [Ubuntu 18.04](https://www.microsoft.com/store/productId/9N9TNGVNDL3Q) from the Windows App Store.
   <br>Note: you will be prompted to create a separate user and password for the linux subsystem.

2. Update Ubuntu - install latest versions of dependencies, etc.

   ```
   sudo apt update
   sudo apt upgrade
   ```

3. Install [essential developer tools](<(https://blogs.windows.com/buildingapps/2016/07/22/fun-with-the-windows-subsystem-for-linux/)>)

   ```
   sudo apt install build-essential
   ```

4. Install [git](https://git-scm.com/)

   ```
   sudo apt install git
   ```

### Set Version to WSL 2

1. Inside Powershell as an Administrator, set the WSL version for your Ubuntu shell:

   ```shell
   # wsl --set-version <Distro> <Version>
   wsl --set-version Ubuntu-18.04 2
   ```

2. Validate the correct WSL version is being used:

   ```shell
   wsl --list --verbose
   ```

See the [development of WSL on GitHub](https://github.com/microsoft/WSL).

### Installing Node.js

1. Install Node Version Manager

   We will use [zsh-nvm](https://github.com/lukechilds/zsh-nvm) to manage versions of node.js. To install as a Oh My ZSH plugin, we need to run:

   ```
   git clone https://github.com/lukechilds/zsh-nvm ~/.oh-my-zsh/custom/plugins/zsh-nvm
   ```

   Then load as a plugin in your `plugins=(zsh-nvm)`

   Keep in mind that plugins need to be added before `oh-my-zsh.sh` is sourced.

2. Install latest Node.js LTS version:

   ```
   nvm install --lts
   ```

3. Check everything works:

   ```
   npm --version
   node --version
   ```

   If it doesn't check that `~/n/bin` is in your `$PATH`.

### Windows Terminal

Microsoft's new [Terminal application for Windows 10](https://www.microsoft.com/store/productId/9N0DX20HK701) is a modern terminal app with support for different shells, themes, tabs and unicode (read emoji) support.

See the [development of Terminal on GitHub](https://github.com/microsoft/terminal).

<details>
<summary>Configuring the Terminal</summary>

- Settings is a JSON file (`profile.json`) so easily syncable over cloud storage or code repository
- JSON Schema for `profile.json` provides autocompletion in editors for easy discovery of options
- Set the default shell using the `globals.defaultProfile` value with the guid from your desired profile `profile[x].guid`.
- Custom font via `profiles[x].fontFace` & `.fontSize`
- Custom theme per profile with `profiles[x].colorScheme`, set with the desired `schemes[x].name` value. Comes with Solarized Dark/Light, Campbell (MS default theme) and One Half Dark/Light
- Adjustable acrylic opacity (blur)
- Editable key bindings

</details>

### Prepare for Ubuntu 18.04

The filesystem used by the Linux shell is hidden deep in your user's AppData folder. To make developing more convenient we will set up a symlink between our `projects` folder across the two environments.

1.  Create a `projects` folder in your Windows user space. I like to use `E:\Projects`
    > Note: Ubuntu will mount your `E:` drive to `/mnt/e` or your `C:` drive to `/mnt/c`
2.  Open a Ubuntu Shell
3.  Create a symlink by linking your new `projects` folder from Windows to our Ubuntu userspace.

    ```shell
    ln -sv /mnt/e/Projects ~/projects
    ```

4.  Validate the symlink with `ls -la` and creating and editing a file from each userspace to see that the changes are reflected correctly.

### Last Steps

Now that we have WSL working and a Ubuntu 18.04 Bash shell we can essentially follow the below Ubuntu guide ⬇️

</details>

## Ubuntu 18.04 or macOS Catalina

Items installed in the following scripts include:

- shell: [`zsh`](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH) · [`oh-my-zsh`](https://github.com/ohmyzsh/ohmyzsh) · [`powerline fonts`](https://github.com/powerline/fonts) · [`starship cross-shell theme`](https://starship.rs/)

### Installation

1. Clone my dotfiles into the `projects` directory

```shell
cd ~ && git clone https://github.com/sleithdylan/dotfiles ~/projects/dotfiles
```

2. Run the `setup-shell.bash` script

```shell
bash ~/projects/dotfiles/scripts/setup-shell.bash
```

- Note: If you need to edit your .zshrc, type in terminal:

```shell
# code /home/dylan/.zshrc
code ~/.zshrc
```

3. Update `config/initial-asdf-plugins.txt` with the desired `asdf` plugins you wish to use. The defaults are listed at the beginning of this section.

4. Run the `setup-devtools.bash` script

```shell
bash ~/projects/dotfiles/scripts/setup-devtools.bash
```

5. Run the `setup-devtools.bash` script again (this is because `asdf` requires a shell restart to take effect. The script accounts for re-running)

```shell
bash ~/projects/dotfiles/scripts/setup-devtools.bash
```

### Cleanup

Run the `cleanup.bash` script

```shell
bash ~/projects/dotfiles/scripts/cleanup.bash
```

## Issues

- If you are getting a folder permission error for example `"Insecure completion-dependent directories detected"`, type the following

```shell
chmod 755 /usr/local/share/zsh
chmod 755 /usr/local/share/zsh/site-functions
```

## License

[MIT License](LICENSE) © [James Hegedus](https://github.com/jthegedus/)
