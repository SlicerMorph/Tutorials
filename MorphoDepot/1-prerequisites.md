_MorphoDepot Tutorial · Part 1 of 8 — Prerequisites & System Configuration_

[⬅ Overview](./README.md)  ·  [Next: Slicer Installation & Setup ➡](./2-slicer-setup.md)

---

## **1. Prerequisites & System Configuration**

First, you need to configure your operating system to communicate with GitHub. Experience shows that setting up these credentials *before* installing Slicer prevents critical connectivity errors later. We also strongly advise using [MorphoCloud Instances](https://morphocloud.org) to cut down the necessary time for setup. On MorphoCloud all prerequisites are installed for you. 

### **1.1 GitHub Account Setup**

1. **Register:** Create an account at [GitHub.com](https://github.com/) if you do not have one.  
2. **Security:** Enable Two-Factor Authentication (2FA).  
3. **Mobile App:** Install the GitHub Mobile App on your phone. This is the easiest way for approving 2FA requests.

### **1.2 Install Command Line Tools**

> [!NOTE]
> [You can skip to Section 1.3](./1-prerequisites.md#13-authenticate-with-github-via-terminal) if you are using MorphoCloud.

You must install git (for version control) and gh (GitHub CLI for authentication) on your computer.  
Important: Make a note of the exact folder path where you install gh. You may need this in the next step.

* **Windows:**  
  * Download and install [Git for Windows](https://git-scm.com/download/win).  
  * Download and install the [GitHub CLI (gh)](https://cli.github.com/).  
* **Mac:**  
  * Open the **Terminal** app.  
  * Type git and press Enter. If not installed, a pop-up will ask to install "Command Line Tools". Accept and install.  
  * Install the GitHub CLI via Homebrew (brew install gh) or download the [Mac installer](https://cli.github.com/).  
* **Linux:**  
  * **Git:** Use your distribution's package manager (e.g., sudo apt install git).  
  * **GitHub CLI (gh):** **Do not use the standard package manager** (e.g., apt or yum) as they often provide outdated or incompatible versions.  
    1. Go to the [GitHub CLI Releases page](https://github.com/cli/cli/releases).  
    2. Download the correct Linux archive (tar.gz) for your architecture.  
    3. Unarchive the file.  
    4. **Note the path** where you extracted the bin/gh file.

### **1.3 Authenticate with GitHub via Terminal**

1. Open your computer’s **Terminal** (Mac/Linux) or **Command window** (Windows).  
2. Run the following command:

   `gh auth login`

   **Troubleshooting:** If you receive a **"Command not found"** or **"File not found"** error, it means `gh` executable is not in your system path. You must locate the installation folder from Section 1.2 and run the command using the **full path**.  
   * *Example (Mac/Linux):* /Users/yourname/downloads/gh\_2.20.0/bin/gh auth login  
   * *Example (Windows):* "C:\\Program Files\\GitHub CLI\\gh.exe" auth login  
3. Follow the prompts:  
   * **Account:** GitHub.com  
   * **Protocol:** HTTPS  
   * **Authenticate:** Login with a web browser.  
4. Copy the one-time code provided in the terminal, paste it into the browser window that opens, and authorize the connection. You might also need to use the GitHub Mobile to do the 2FA.  
5. **Success:** The terminal should verify that you are logged in. If not, you will need to redo the procedure.

You can confirm you have successfully logged in via command `gh auth status`, which should display that you are logged in and your username.

---

[⬅ Overview](./README.md)  ·  [Next: Slicer Installation & Setup ➡](./2-slicer-setup.md)
