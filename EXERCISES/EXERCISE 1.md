## GitFlow Practice Exercise: **"DevTasker"**

### Scenario

You're working solo (or in a small team) on a task management app called **DevTasker**. Your goal is to simulate a real software development cycle using **GitFlow**: features, releases, hotfixes, and versioning.

---

### PHASE 1: Project Setup

* Initialize a new project folder.
* Initialize a Git repository.
* Configure GitFlow (accept the default branch names like `main` and `develop`).

---

### PHASE 2: Develop Features

1. **Feature A: Add Task**

   * Create a feature branch from `develop`.
   * Implement a basic task-adding function.
   * Merge the feature back into `develop`.

2. **Feature B: List Tasks**

   * Start a new feature branch.
   * Add logic to list all tasks.
   * Merge it into `develop`.

---

### PHASE 3: Prepare a Release

* Begin a release branch for version `1.0.0`.
* Update documentation and version number.
* Merge the release into both `main` and `develop`.
* Tag the release.

---

### PHASE 4: Hotfix a Bug in Production

* Pretend a user found a bug in the list feature.
* Start a hotfix branch from `main`.
* Fix the bug and finalize the hotfix.
* Merge it back into both `main` and `develop`.
* Tag the hotfix version (e.g., `1.0.1`).

---

### PHASE 5: Continue Development

* Add a new feature (e.g., delete task).
* Follow the GitFlow steps: branch off from `develop`, complete it, and merge back in.

---

### PHASE 6: New Release

* Start a release for version `1.1.0`.
* Finalize documentation or any cleanup.
* Merge and tag.
