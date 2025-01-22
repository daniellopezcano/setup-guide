# Basic git workflow chart
![[Pasted image 20220906100622.png]]


# Setting up SSH Access for GitHub
Setting up SSH access allows you to securely authenticate with GitHub without needing to enter your username and password every time you interact with a repository.
### **Step-by-Step Guide**
1. Follow up to step 4 of the [[SSH Keygen]] guide, then, copy the public key to your clipboard:
   - Display your public SSH key on the terminal:
     ```bash
     cat ~/.ssh/id_ed25519_github.pub
     ```
   - The `.pub` file contains the public part of your SSH key. Copy this key to your clipboard using your terminal or editor.

2. **Add the SSH Key to Your GitHub Account**
   - Go to the [GitHub SSH Settings](https://github.com/settings/keys).
   - Click the **New SSH Key** button.
   - Provide a descriptive title for the key (e.g., "Laptop SSH Key"). This helps you identify which device the key belongs to if you need to manage multiple keys.
   - Paste the public key into the provided field and click **Add SSH Key**.

3. **Test the Connection**
   - Verify that your SSH key has been added correctly and is working with GitHub:
     ```bash
     ssh -T git@github.com
     ```
   - If successful, you should see the message:
     ```plaintext
     Hi daniellopezcano! You've successfully authenticated, but GitHub does not provide shell access.
     ```

4. **Clone a Repository Using SSH**
   - Once the SSH key is set up, you can clone repositories securely using SSH URLs. For example:
     ```bash
     git clone git@github.com:daniellopezcano/monitor_pc.git
     ```
   - This ensures your connection is authenticated via your SSH key.

---

# Git Workflow in Visual Studio Code

### **Learning Resources**
- Explore how to use Git within VS Code through the official tutorial video:
  [Git in Visual Studio Code](https://www.youtube.com/watch?v=i_23KUAEtUM&ab_channel=VisualStudioCode)
- Merge conflicts are common when working collaboratively on Git projects. Learn to handle them effectively using VS Code:
  [Guide to Merge Conflicts in VS Code](https://www.youtube.com/watch?v=HosPml1qkrg&ab_channel=VisualStudioCode)

---

# Check Different Workflows

Understanding different Git workflows is essential for collaborating effectively in teams. Here are two valuable resources:
1. **Atlassian Git Tutorials**: [Comparing Workflows](https://www.atlassian.com/git/tutorials/comparing-workflows)
   - Covers various Git workflows like feature branching, Gitflow, and forking.
2. **A Successful Git Branching Model**: [Nvie Blog Post](https://nvie.com/posts/a-successful-git-branching-model/)
   - Explains the popular Gitflow branching strategy for managing releases, hotfixes, and feature development.

---

# Git Submodules

Git submodules allow you to include external repositories within your own repository. This is particularly useful for shared libraries, dependencies, or components that you don't want to copy directly into your project.

- **Why use submodules?**
    - Maintain a separate versioning history for the included repository.
    - Easily update the included repository when changes are made upstream.
    - Avoid duplicating code or files unnecessarily.
- **Use Cases:**
    - Sharing code between projects.
    - Including third-party libraries.
    - Managing projects with modular components.

[[git submodules]]