#Git Snippets

##Start a git repository

1. Create a directory on client side where I want the code to be (/Users/lenglee/badassery)
2. Go to that directory
3. Initialize the git repo (git init) - Makes a .git directory and populate it with useful stuff. This is done in badassery.
4. Start a gitignore. Do this by saving a file in sublime text that has the word .DS_Store (and nothing else)
5. Type in badassery directory: git add .gitignore
6. git commit -am 'first commit'
7. Make a new repo on github
8. Copy instructions (this is just an example) 
    1. git remote add origin git@github.com:lenglee1/Badassery.git
    2. git push -u origin master

##Add something to git repo

1. Save a file in the git directory (ie. badassery)
2. git add <file name>
3. git commit -m 'add comment'
4. git pull origin master
5. git push origin master

