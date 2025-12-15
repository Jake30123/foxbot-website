# Foxbot Website Setup

Welcome to the Foxbot Website repository! This guide will walk you through setting up the necessary dependencies to run this Jekyll theme locally on both Windows and Linux environments.

## Step 1: Clone the Repository

1.  **Open your terminal** (Linux/macOS) or **Git Bash** (Windows).
2.  Navigate to the directory where you store your projects.
3.  Clone the repository using the following SSH command:

    ```bash
    git clone git@github.com:Jake30123/foxbot-website.git
    ```

4.  Change into the project directory:

    ```bash
    cd foxbot-website
    ```

---

## Step 2: Install Ruby and Dependencies


### A. Windows Installation

Windows requires a specific installer and toolset to handle the Ruby Gems successfully.

1.  **Install Ruby:**
    * Download the latest **Ruby+Devkit** installer for your systemfrom [RubyInstaller.org](https://rubyinstaller.org/downloads/).
    * During installation, make sure to check the box for **"Add Ruby executables to your PATH"**.

2.  **Run the Ridk Tool:**
    * At the end of the installation, a new terminal window should open and prompt you to run the **`ridk install`** tool.
    * Press `Enter` to select **MSYS2 and MINGW development toolchain** (Option 1) and install the necessary C/C++ build tools. This is crucial for installing many Jekyll gems.

### B. Linux Installation (Debian/Ubuntu Example)

1.  **Update Packages:**

    ```bash
    sudo apt update
    ```

2.  **Install Ruby and necessary tools:**

    ```bash
    sudo apt install ruby-full build-essential zlib1g-dev
    ```

3.  **Configure Gem Path (Crucial for permissions):**
    * You must configure your system to install gems to a user-specific directory instead of system-wide, which avoids permission errors (`sudo` is usually bad for gems).
    * Add the following lines to your shell's configuration file (e.g., `~/.bashrc` or `~/.zshrc`):

        ```bash
        # Install Ruby Gems to ~/gems
        export GEM_HOME="$HOME/gems"
        export PATH="$HOME/gems/bin:$PATH"
        ```

    * Apply the changes by restarting your terminal or running:
        ```bash
        source ~/.bashrc
        ```

---

## Step 3: Install Bundler and Jekyll Gems

1.  **Install Bundler (globally):**

    ```bash
    gem install bundler
    ```

2.  **Install Project Gems:**
    * Make sure you are in the `foxbot-website` directory.
    * This command reads the `Gemfile` and installs all the project's requirements:

    ```bash
    bundle install
    ```
---

## 🏃 Step 4: Run the Website Locally

Run the following command from the `foxbot-website` directory:

```bash
bundle exec jekyll serve
```