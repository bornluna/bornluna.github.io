---
layout: post
title:  "回收IIS应用程序池时的逻辑步骤"
date:   2018-05-28 20:52:23 +0800
categories: update
---
IIS 应用程序池Recycle大致步骤，
一. 应用程序池创建新的进程处理新的Request（CLR线程池产生每个Reuqest的后台线程）
二. 开始终止当前进程
	被终止的进程要求CLR进行卸载
	1. CLR要求默认和所有AppDomain进行卸载
		AppDomain卸载步骤
		a. CLR挂起进程中执行过托管代码的所有线程
		b. 要求AppDomain相关线程抛出ThreadAbortException
		c. 等待线程，如果它正在类的构造、catch代码、finally代码、CER代码、非托管代码
		d. 强制垃圾回收对应AppDomain
    e. 恢复其他AppDomain的线程的执行
