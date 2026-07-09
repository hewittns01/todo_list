# Putting this to-do list on GitHub Pages

I don't have access to a GitHub connection in this session, so you'll need to run these steps yourself. It only takes a few minutes, and you'll only need to touch git commands once.

## 1. Create the repo
On github.com, click **New repository**. Name it something like `nathan-todo`. Public is required for free GitHub Pages (private works too if you're on GitHub Pro). Don't initialize with a README.

## 2. Push this file, from RStudio
In RStudio, open (or create) an RStudio Project rooted in this folder (`NathanToDoList`): **File > New Project > Existing Directory**, and point it at this folder. Then, in the Console, run:

```r
install.packages(c("usethis", "gert"))

usethis::use_git()                       # initializes the repo if it isn't already one
gert::git_add("index.html")
gert::git_commit("Add to-do list")
usethis::use_github(organisation = NULL) # creates the GitHub repo and pushes, using your existing GitHub PAT
```

`usethis::use_github()` will prompt you to name the repo (use `nathan-todo`) and pushes `main` for you. If you'd rather create the GitHub repo yourself first (as in step 1) and just wire this project to it, use instead:

```r
usethis::use_git_remote("origin", url = "https://github.com/YOUR-USERNAME/nathan-todo.git")
gert::git_push()
```

Once the project is git-tracked, RStudio's **Git** tab (top-right pane) shows the same staging/commit/push controls if you'd rather click through than run commands — no PowerShell or Terminal needed either way.

## 3. Turn on GitHub Pages
In the repo, go to **Settings > Pages**. Under "Build and deployment," set Source to **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.

Your list will be live in a minute or two at:
```
https://YOUR-USERNAME.github.io/nathan-todo/
```

## 4. Create a personal access token
The page reads and writes an Excel file (`tasks.xlsx`) directly to this repo, so it needs a token to authenticate as you.

1. Go to **github.com/settings/tokens?type=beta** (fine-grained tokens).
2. Click **Generate new token**.
3. Under "Repository access," choose **Only select repositories** and pick `nathan-todo`.
4. Under "Permissions," set **Contents** to **Read and write**.
5. Generate the token and copy it (you won't see it again).

## 5. Connect the page
Open your live page, click **GitHub settings**, and fill in:
- Repository owner: your GitHub username
- Repository name: `nathan-todo`
- File path: `tasks.xlsx` (default — no need to create this file yourself, the page creates it on the first task you add)
- Branch: `main`
- Personal access token: paste the token from step 4

Click **Save & sync**. From then on, every task you add, check off, or edit commits automatically to `tasks.xlsx` in the repo — no more git commands needed.

## How it works day to day
The Excel file is the source of truth. When you open the page, it pulls the latest `tasks.xlsx` from GitHub. When you make a change, it waits about a second (to batch quick edits) and then pushes an updated version back. The token lives only in your browser's local storage — it's used to talk to GitHub's API directly and isn't sent anywhere else.

If you ever want to look at or edit `tasks.xlsx` directly (e.g. bulk-editing in Excel), you can — just make sure the page has pulled your changes (click "Sync now") before you edit it live in the browser again, to avoid overwriting each other.
