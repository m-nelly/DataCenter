## Setup 
| Command                                   | Description                       |
| ----------------------------------------- | --------------------------------- |
| `git config --global user.name "$name"`   | Set global name for commits       |
| `git config --global user.email "$email"` | Sets global email for commits     |
| `git config --global color.ui auto`       | Sets UI to use color if available |

## Initial Commit
| Command                                            | Description                                                                      |
| -------------------------------------------------- | -------------------------------------------------------------------------------- |
| `git init`                                         | Initializes a git repo in the current directory                                  |
| `git add .`                                        | Initializes all files in the current directory to be committed to the repository |
| `git commit -m "Initial Commit"`                   | Initializes the commit with a message                                            |
| `git branch -M main`                               | Initializes a Main branch                                                        |
| `git remote add origin git@github.com/$user/$repo` | Sets remote origin to the remote repository location                             |
| `git push -u origin main`                          | Pushes local changes to the remote repository                                    |                                                   |                                                                                  |

## Cloning & Pulling
| Command                       | Description                                              |
| ----------------------------- | -------------------------------------------------------- |
| `git clone https://$repo.git` | Clone remote repository using HTTPS                      |
| `git pull`                    | Pull and merge all remote changes from the remote branch |

## Serve Git Locally
| Command                                 | Description                                  |
| --------------------------------------- | -------------------------------------------- |
| `git init`                              | Initialize local repository                  |
| `git add .`                             | Add all files to the local repository        |
| `git commit -m "Init"`                  | Commit files to the local repository         |
| `git --bare update-server-info`         | Update server info with latest repo changes  |
| `python2 -m http.server`                | Serve the current directory on port 8000     |
| `git clone http://$lhost:8000/repo.git` | Pull repo from local system on remote system |

