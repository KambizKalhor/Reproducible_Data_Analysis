# Working with GitHub in VS Code

Install the GitHub Pull Requests and Issues extension on VScode

![Screenshot 2023-08-28 at 12.41.44.png](Working%20with%20GitHub%20in%20VS%20Code%2066ee2b386cf14849883649c0968e7134/Screenshot_2023-08-28_at_12.41.44.png)

### [Cloning a repository](https://code.visualstudio.com/docs/sourcecontrol/github#_cloning-a-repository)

You can search for and clone a repository from GitHub using the **Git: Clone** command in the Command Palette (⇧⌘P) or by using the **Clone Repository** button in the Source Control view (available when you have no folder open).

From the GitHub repository dropdown you can filter and pick the repository you want to clone locally.

![Image 8-28-23 at 12.51.jpg](Working%20with%20GitHub%20in%20VS%20Code%2066ee2b386cf14849883649c0968e7134/Image_8-28-23_at_12.51.jpg)

# Basic Commands you need

```bash
# add changes in one file
git add some_file

# add all changes
git add .

# commit changes
git commit -m 'massage for commit'

# to upload changes
git push

# to download changes
git pull

```

## **[Generating a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux#generating-a-new-ssh-key)**

you need to do it once.

You can generate a new SSH key on your local machine. After you generate the key, you can add the public key to your account on GitHub.com to enable authentication for Git operations over SSH.

[Generating a new SSH key and adding it to the ssh-agent - GitHub Docs](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux)

```bash
ssh-keygen -t ed25519 -C "kkalhor@vols.utk.edu"
```