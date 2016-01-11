---
layout: post
title:  "How to organize git branches on a project"
date:   2016-01-11 16:57:38
categories: git
comments: true
---

### Use case
Let's say there is a team of five developers and one manager. Manager supplies tasks, developers make them happen. Some developers are more confidant, so they push their code stright to master. Some developers are not so confidant, so they develop new features in feature branches and submit pull requests for a review. Some features need managers approval, so we need to have QA server for him to be able to look at it. After number of features accumulated in master branch, manager decides that there should be a production release.

### Master branch
All code ends up in master branch, either stright from developer's machine or after pull request gets merged.
Master branch ideally should be deployable at any point in time. You need to have a good test suite to be confidant about that.

### Feature branches
All the work going in feature branches, which get merged into master branch after code review. Github pull requests is a convenient way to do that. If your team is less formal, everyone can push stright to master.

### QA server
Sometimes manager or client needs to look at how particular feature works. You can have QA server for this and deploy either your feature branch or master branch on it.

### Production server
Production has it's own branch. At any point in time, you can checkout to production branch and see what code is currently running on production server. Also, having dedicated branch makes it easy to supply bugfixes. If something goes wrong, you make a fix in production branch, cherry pick it back to master and redeploy production.

### Instruments
- Capistrano 

Create QA and production deployment environments, which mainly going to be different by branch they are deploying and server ip address. You all set, just run `cap qa deploy` to deploy to QA server or `cap production deploy` to deploy to production server.