# Your first PR

Pull requests with patches, improvements, new features are a great help. 

Follow the steps to get your work included in the project.

#### 1. Fork the project, clone your fork, and add the UCI-PWA remote:

```
# Clone your fork of the repo into the current directory
git clone https://github.com/<your-username>/inbound.git

# Navigate to the cloned directory
cd inbound

# Assign the original repo to a remote called "upstream"
git remote add upstream https://github.com/samagra-comms/inbound.git
```

#### 2. Get the latest changes from upstream:

```
git checkout master
git pull upstream master
```

#### 3. Create a new branch from the master/latest working branch to contain your changes. Best way is to call is to follow the type described in Git Commit Conventions stated above: <githubId>/<description/scope/topic>

```git checkout -b <topic-branch-name>```

Example:

```git checkout -b mike/buckets-undefined-index```

Or

```git checkout -b mike/fix```

#### 4. Commit your changes

Commit your changes in logical chunks. Please adhere to **Git Commit Conventions** and **Coding guidelines** or your code is unlikely to be merged into the main project. Use Git's [interactive rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase) feature to tidy up your commits before making them public.

#### 5. Locally rebase the upstream master branch into your topic branch:

```git pull --rebase upstream master```

#### 6. Push your topic branch up to your fork:

```git push origin <topic-branch-name>```

#### 7. [Open a Pull Request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/about-pull-requests) with a clear title and description against the **master/latest** branch.