# 30_days_devops_training_veeresh_alladi

DAY 1 _ hands _on:

GitHub:
__________
scm - code management /store


Git:
--------------
Version control - v1
                  v2

git status

file.xt 

git add file.txt
git commit -m "created file"
git push


master -

branching: 

-----------------
Master (main code) - fixed
main (developers)
development (developers)
feature branch - code changes - Hand on
release/deployment/go live - (next) docs
hotfix branch - asap 

Pull Request:
-----------------
feature branch
git push origin test - > compare pr - raise - Hands On

Merge conflicts:
-------------------
main - file.txt (already)
feature - file.txt (chnage)



DAY 2:
----------
DAY 2:
------------
developer  - push - (PR) - ci cd pipeline - build (maven) - Test (unit test cases) - deploy(dev,staging,prod) - post deploy(Health check)


triggers:
-----------

on:
  pull_request:
    branches: [ main ]
  push:
    branches: [ main ]
  schedule:
    - cron: "0 1 * * *"
  workflow_dispatch:


runners:
---------
Jenkins - workernodes
GitHub actions - selfhosted (vm)
               - ubuntu-latest




checkout:
---------
GitHub actions

actions:
--------------
aws setup
python setup
terraform setup
checkout





ci cd create for python code deploy

name: CI Pipeline

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest/ self-hosted

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run a script
        run: echo "Build Successful!"

      - name: Run Python file
        run: python3 test.py

  test: (robot/ unit)
    runs-on: ubuntu-latest/test vm
    steps:
      - name: testing
        run: mvn install





  deploy:
    runs-on: ubuntu-latest/deploy vm
 
  post-deploy:
     runs-on:

