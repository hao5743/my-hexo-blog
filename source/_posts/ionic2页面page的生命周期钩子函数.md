---
title: ionic2页面page的生命周期钩子函数 
date: 2017-02-08 11:45:54
tags: [ionic2]
---

# ionic2页面page的生命周期钩子函数 


* Page Event	Returns	Description

* `ionViewDidLoad`	void	Runs when the page has loaded. This event only happens once per page being created. If a page leaves but is cached, then this event will not fire again on a subsequent viewing. The ionViewDidLoad event is good place to put your setup code for the page.
* `ionViewWillEnter`	void	Runs when the page is about to enter and become the active      page.
* `ionViewDidEnter`	void	Runs when the page has fully entered and is now the active page. This event will fire, whether it was the first load or a cached page.
* `ionViewWillLeave`	void	Runs when the page is about to leave and no longer be the active page.
* `ionViewDidLeave`	void	Runs when the page has finished leaving and is no longer the active page.
* `ionViewWillUnload`	void	Runs when the page is about to be destroyed and have its elements removed.
* `ionViewCanEnter`	boolean/Promise<void>	Runs before the view can enter. This can be used as a sort of "guard" in authenticated views where you need to check permissions before the view can enter
* `ionViewCanLeave`	boolean/Promise<void>	Runs before the view can leave. This can be used as a sort of "guard" in authenticated views where you need to check permissions before the view can leave



-------
end 2017-02-08 11:44:47

参考文档：[http://ionicframework.com/docs/v2/api/navigation/NavController/](http://ionicframework.com/docs/v2/api/navigation/NavController/)


