# Portfolio Project: Git Workflow & Troubleshooting Case Study

## 📂 Portfolio Project: Git Workflow & Troubleshooting Case Study

### 1. Project Title

**Title:** Publishing an Existing Local Project to GitHub: A Git Workflow and Troubleshooting Case Study

### 2. Overview & Challenge

**Goal:** To successfully publish a multi-file, existing local project (`secure-url-shortener-tf`) to a new remote repository on GitHub and establish a continuous integration workflow.

**The Challenge:** The process was not straightforward due to a series of persistent, interlocking Git and synchronization errors, turning the task into a valuable troubleshooting exercise. This project documents the systematic approach taken to diagnose and resolve these critical issues.

### 3. Key Technical Skills Demonstrated

- **Version Control:** Git initialization (`git init`), staging (`git add`), committing (`git commit`), and pushing (`git push`).
- **Remote Management:** Configuring remote links (`git remote add origin`), removing broken links (`git remote remove origin`), and setting upstream branches (`git push --set-upstream`).
- **Error Diagnosis:** Interpreting fatal errors like `repository '... not found'`, `src refspec main does not match any`, and `index.lock: File exists`.
- **Problem Resolution:** Implementing targeted commands (`rm -f .git/index.lock`, `git checkout -b main`) to resolve conflicts and system blockages.
- **Synchronization:** Ensuring local and remote repositories are perfectly synchronized (`Everything up-to-date`).

### 4. The Diagnostic and Resolution Process

The core of this project is the step-by-step resolution of three simultaneous, critical errors.

### A. Initial Failure: Missing Repository (404 Error)

| Error Message | Cause | Resolution |
| --- | --- | --- |
| `fatal: repository '... not found'` | The repository URL was set locally, but the corresponding repository (`secure-url-shortener-tf`) **did not exist** on the GitHub server. | **Action:** Created the empty repository manually on the GitHub website (`https://github.com/new`). |

### B. Local Blockage: The Lock File Conflict

| Error Message | Cause | Resolution |
| --- | --- | --- |
| `fatal: unable to create ... .git/index.lock: File exists.` | A previous Git operation crashed, leaving a temporary lock file, which prevented *any* subsequent Git command (like `add` or `commit`) from running. | **Action:** Used the command `rm -f .git/index.lock` to manually delete the blocking file, unblocking the local Git system. |

### C. Synchronization Failure: Missing Commit/Branch

| Error Message | Cause | Resolution |
| --- | --- | --- |
| `error: src refspec main does not match any` | The local branch was either named `master` or had **no commits** saved, so there was nothing for Git to push to the remote. | **Action:** Staged and saved all files using `git add .` followed by `git commit -m "Initial project files commit."` |

### 5. Successful Outcome

The final, combined sequence of commands successfully initialized, linked, and uploaded the project:

1. `rm -f .git/index.lock` (Unblocked local Git)
2. `git add .` & `git commit -m "Initial project files commit."` (Saved local files)
3. *Manual creation of the empty GitHub repo* (Fixed 'not found' error)
4. `git push --set-upstream origin main` (Forced upload and set permanent link)

**Final Verification:** The `git status` command confirmed the successful synchronization:

> On branch mainYour branch is up to date with 'origin/main'.nothing to commit, working tree clean
> 

### 6. Links and Resources

- **Live Project Repository:** [Insert the link to your finished repository: `https://github.com/chifru19/secure-url-shortener-tf` here]
- **Tools Used:** Visual Studio Code (VS Code), Git, GitHub.

---

![Screenshot 2025-12-11 at 19.41.19.png](Portfolio%20Project%20Git%20Workflow%20&%20Troubleshooting%20C/Screenshot_2025-12-11_at_19.41.19.png)

![Screenshot 2025-12-11 at 20.12.17.png](Portfolio%20Project%20Git%20Workflow%20&%20Troubleshooting%20C/Screenshot_2025-12-11_at_20.12.17.png)

![Screenshot 2025-12-11 at 20.11.56.png](Portfolio%20Project%20Git%20Workflow%20&%20Troubleshooting%20C/Screenshot_2025-12-11_at_20.11.56.png)

![Screenshot 2025-12-11 at 19.16.19.png](Portfolio%20Project%20Git%20Workflow%20&%20Troubleshooting%20C/Screenshot_2025-12-11_at_19.16.19.png)

![Screenshot 2025-12-11 at 19.14.29.png](Portfolio%20Project%20Git%20Workflow%20&%20Troubleshooting%20C/Screenshot_2025-12-11_at_19.14.29.png)

![Screenshot 2025-12-11 at 16.36.30.png](Portfolio%20Project%20Git%20Workflow%20&%20Troubleshooting%20C/Screenshot_2025-12-11_at_16.36.30.png)
