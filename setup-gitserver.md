### Git server with SSH Keys


1. Create the SSH Key pair to be used from the clients that will sync to the Git over SSH
            `ssh-keygen -C "servomekanism@gmail.com"` or if you need another file maybe:
            `ssh-keygen -t rsa -b 4096 -C servo -f ~/.ssh/id_rsa_gitserver`
2. Create git user and install git at the git server (make sure you create the user first):
            `useradd -m git`
            `pacman -S git`
3. Create the respective folders at the git server
            `su - git`
            `mkdir ~/.ssh && touch ~/.ssh/authorized_keys`
4. Add the SSH keys to the git server from the client(s)
            `cat .ssh/id_rsa.pub | ssh git@192.168.1.1 "cat >> ~/.ssh/authorized_keys"`
5. Setup a local repository
            `su - git`
            `cd`
            `git init --bare project-name.git`


## Using the git server
If this is a new repository:

`git init`

`vim .gitignore`

`git add .`

`git commit -m "first commit`

`git remote add origin git@gitserver:my-project.git`

`git remote -v`

If this is a local repo that needs to be pushed on the git server:

`git remote set-url origin git@gitserver:my-project.git`

Then do as usual:

`git commit -m "first commit"`

`git push origin master`
