---
title: git不常用命令
date: 2016-11-08 16:18:31
tags: [git,读书笔记]
---
读《git权威指南》,记录一些git的命令。

## git rev-parse
* `git rev-parse --git-dir`	显示版本库.git目录所在位置
* `git rev-parse --show-toplevel`	显示工作区根目录
* `git rev-parse --show-prefix`	所在目录相对于工作区根目录的相对目录
* `git rev-parse --show-cdup`	显示从当前目录后退到工作区的根的身度

## git commit --amend --allow-empty --reset-author
* `--amend`:修改刚才的提交
* `--allow-empty`:允许提交空白
* `--reset-author`:重置author

## git ls-tree -l HEAD 
查看版本目录树


## git ls-files -s
显示暂存区的目录树

## git write-tree | xargs git ls-tree -l -r -t
递归操作显示目录树

## git diff
* `git diff` 工作区和暂存区比较
* `git diff --cached` 暂存区和HEAD比较
* `git diff HEAD` 工作区和HEAD比较

