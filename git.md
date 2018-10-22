# Basic git commands
```
git config --global user.name "Mona Lisa"
git config --global user.email "email@example.com"
# Per repository basis
git config user.name “Alex Sha”
git config user.email “alex@example.com”
# Cloning, Commiting
git clone URL
git add .
git commit -m "Commit name"
git push
git pull
```

A script to automate commiting and pushing `gacp`
```
#!/bin/bash
git add .
git commit -m "$1 $(date)"
git push
```
