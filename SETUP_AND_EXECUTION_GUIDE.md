# 🚀 Complete CI/CD Project Setup & Execution Guide

This document is a complete, beginner-friendly guide covering everything from installing the required software to executing your automated CI/CD pipeline. 

---

## 🛠️ Phase 1: Installation & Setup

### 1. Git & GitHub
**Git** is a version control system that tracks changes in your code. **GitHub** is a website where you store your Git repositories online.

*   **Install Git:**
    1. Go to [https://git-scm.com/download/win](https://git-scm.com/download/win) and download the Windows installer.
    2. Run the installer and click "Next" through all the default options.
    3. To verify, open your Command Prompt (cmd) and type: `git --version`.
*   **Set up GitHub:**
    1. Go to [https://github.com/](https://github.com/) and create a free account if you don't have one.
    2. Click the `+` icon in the top right and select **New repository**.
    3. Name it `devops-pipeline-demo`, leave it Public, and click **Create repository**. (Do NOT add a README or .gitignore yet).

### 2. Docker Desktop
**Docker** packages your application and its environment into a "container" so it runs exactly the same way on any computer.

*   **Install Docker:**
    1. Go to [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/) and download Docker Desktop for Windows.
    2. Run the installer. Ensure "Use WSL 2 instead of Hyper-V" is checked if prompted.
    3. Once installed, restart your computer if required.
    4. Open the "Docker Desktop" application and let it run in the background. (You might need to accept terms and conditions).
    5. To verify, open Command Prompt and type: `docker --version`.

### 3. Jenkins
**Jenkins** is an automation server. It will detect when you push code to GitHub, build your Docker image, and run it.

*   **Install Jenkins:**
    1. Go to [https://www.jenkins.io/download/](https://www.jenkins.io/download/) and download the **Windows** installer (under LTS).
    2. Run the installer. When it asks for Logon Type, select "Run service as LocalSystem" (this is easiest for local testing).
    3. Leave the port as `8080`.
    4. Once installed, it will open `http://localhost:8080` in your browser.
    5. It will ask for an **Administrator password**. It provides a file path on the screen (usually `C:\Program Files\Jenkins\secrets\initialAdminPassword`). Open that file using Notepad, copy the password, and paste it into the browser.
    6. Click **Install suggested plugins** and wait for them to install.
    7. Create your First Admin User (username, password, name, email) and click Save and Finish.

---

## 🔗 Phase 2: Connecting Your Code to GitHub

Now that the software is installed, you need to push your local project to your new GitHub repository.

1. Open **VS Code**.
2. Go to `Terminal -> New Terminal` from the top menu.
3. Run the following commands one by one to push your code:

```bash
# Initialize git in your folder
git init

# Add all your files
git add .

# Save the changes with a message
git commit -m "First commit of my Flask CI/CD app"

# Link your local folder to your GitHub repository 
# (REPLACE THE URL BELOW WITH YOUR ACTUAL GITHUB REPO URL)
git remote add origin https://github.com/YOUR_USERNAME/devops-pipeline-demo.git

# Push the code to GitHub
git branch -M main
git push -u origin main
```
*(Note: GitHub may ask you to sign in via a browser popup during the push).*

---

## ⚙️ Phase 3: Setting Up the Jenkins Pipeline

Now we tell Jenkins to read the `Jenkinsfile` in your GitHub repository and execute it.

1. Open Jenkins in your browser (`http://localhost:8080`).
2. Click **New Item** on the left menu.
3. Enter a name (e.g., `Flask-Docker-Pipeline`), select **Pipeline**, and click **OK**.
4. Scroll down to the **Pipeline** section at the bottom.
5. In the **Definition** dropdown, select **Pipeline script from SCM** (SCM means Source Control Management, i.e., GitHub).
6. In the **SCM** dropdown, select **Git**.
7. In the **Repository URL** field, paste your GitHub repository link (e.g., `https://github.com/YOUR_USERNAME/devops-pipeline-demo.git`).
8. Under **Branches to build**, make sure branch specifier says `*/main` (change it from `*/master` if necessary).
9. Ensure **Script Path** says `Jenkinsfile`.
10. Click **Save**.

---

## ▶️ Phase 4: Execution (Running the Pipeline)

### The Manual Run (First Time)
1. In your Jenkins Pipeline dashboard, click **Build Now** on the left menu.
2. A new build (`#1`) will appear in the Build History in the bottom left.
3. Click on the `#1`, then click on **Console Output**.
4. You will see Jenkins reading your `Jenkinsfile`, downloading your code, building the Docker container, and running it.
5. Once it says **SUCCESS** at the bottom, go to a new browser tab and open `http://localhost:5000`. 
6. **You should see your web application running!**

### The Automated Run (The True CI/CD Demonstration)
This is what you show the examiner!

1. Open **VS Code**.
2. Open `templates/index.html` and change some text (e.g., change "Welcome" to "Welcome to the CI/CD Pipeline").
3. Save the file.
4. Open the VS Code terminal and push the changes:
   ```bash
   git add .
   git commit -m "Updated website title"
   git push origin main
   ```
5. Go back to Jenkins and click **Build Now** again. (In a real enterprise setup, GitHub tells Jenkins automatically, but for local Windows setups, clicking Build Now simulates this trigger).
6. Wait for the build to say **SUCCESS**.
7. Go to `http://localhost:5000` and refresh the page. 
8. **The website updates automatically without you ever touching the server manually!** 

*(If Jenkins fails at the Docker build stage with a "docker not recognized" error or permissions issue, you may need to restart the Jenkins service from Windows Services, or add Jenkins to the Docker-users group on your Windows machine).*
