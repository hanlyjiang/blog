---
title: 多个Github仓库合并到一个作为分支存在
date: 2016-01-11 10:12:50
category:
- 版本控制
- GIT
tags:
- git
---
目的： 将github上多个仓库合并到一个里面，保留提交记录

	git clone https://github.com/hanlyjiang/AndroidLearning.git
	cd AndroidLearning
	
	# 合并第一个仓库
	git remote add RepoA https://github.com/hanlyjiang/AndroidUtils.git
	git pull RepoA
	git checkout -b android_utils_master RepoA/master
	git push origin android_utils_master
	git checkout -b android_utils_gh_pages RepoA/gh-pages
	git push origin android_utils_gh_pages
	
	# 合并第二个仓库
	git remote add RepoA https://github.com/hanlyjiang/AndroidDemos.git
	git pull RepoB
	git checkout -b android_demos_master RepoB/master
	

然后再访问https://github.com/hanlyjiang/AndroidLearning 即可看到仓库都作为分支被推送过去了，接着在github上删掉被合并的两个仓库