---
layout: post
title:  "7Layer api gateway configure for first time"
date:   2015-04-05 22:05:40
categories: 7Layer
---
>The guide is apply to gateway 8.2

>And the purpose is for evaluating, so try to make the configuration workflow is most simple

Preparations,  set simple password for `root` and `ssgconfig`, configure hostname
-----------

1. login with user root, default password is `7layer`
![login with user root]({{site.url}}/assets/Configure Gateway for First Time/5.png)

2. Need change password when first login, like `QWEasd!@#456`
![change root password]({{site.url}}/assets/Configure Gateway for First Time/10.png)

3. Retype new password `QWEasd!@#456`
![retype new root password]({{site.url}}/assets/Configure Gateway for First Time/15.png)

4. For type password simple, change password of current user(root) to `7layer`, type `passwd`
![type passwd]({{site.url}}/assets/Configure Gateway for First Time/20.png)

5. Change password of user ssgconfig with `7layer`, type `passwd ssgconfig`
![type passwd for ssgconfig]({{site.url}}/assets/Configure Gateway for First Time/25.png)

6. Type `ifconfig` for get the ip address `192.168.44.130`
![get ip address]({{site.url}}/assets/Configure Gateway for First Time/26.png)

7. Type `vi /etc/hosts`  
![edit host]({{site.url}}/assets/Configure Gateway for First Time/27.png)

8. Put ip address `192.168.44.130` and hostname `g3.7layer.com`(just for test, you can change it as your wish) relationship into `hosts`
![edit host 2]({{site.url}}/assets/Configure Gateway for First Time/28.png)

9. Logout and login with `ssgconfig`
![logout]({{site.url}}/assets/Configure Gateway for First Time/30.png)
![login with ssgconfig]({{site.url}}/assets/Configure Gateway for First Time/35.png)


Configure networking and system time settings
------------

10. Select 1 `1) Configure system settings`
![Configure system settings]({{site.url}}/assets/Configure Gateway for First Time/40.png)

11. Select 1 `1) Configure networking and system time settings`
![Configure networking and system time settings]({{site.url}}/assets/Configure Gateway for First Time/45.png)

12. Select 1 `eth0` to configure
![select eth0]({{site.url}}/assets/Configure Gateway for First Time/50.png)

13. Type enter(default is yes) 
![enable interface on boot]({{site.url}}/assets/Configure Gateway for First Time/55.png)

14. Type enter(default is yes)
![would you like to configure IPv4 networking]({{site.url}}/assets/Configure Gateway for First Time/60.png)

15. Type enter(default is DHCP), you can change to static according to the actual environment
![enter the protocol]({{site.url}}/assets/Configure Gateway for First Time/65.png)

16. Type enter, leave empty for dhcp hostname, sure of that you can enter a right hostname for dhcp
![dhcp hostname]({{site.url}}/assets/Configure Gateway for First Time/70.png)

17. Type enter(default is no), don't configure IPv6 networking
![would you like to configure IPv6 networking]({{site.url}}/assets/Configure Gateway for First Time/75.png)

18. Type enter(default is no), don't configure another network interface
![would you like to enter another network interface]({{site.url}}/assets/Configure Gateway for First Time/80.png)

19. Type enter(default is no), don't change the current default IPv4 gateway and interface
![would you like to change the current default IPv4 gateway and interface]({{site.url}}/assets/Configure Gateway for First Time/85.png)

20. Enter fully qualified hostname `g3.7layer.com` which configed in `/etc/hosts` before
![Enter fully qualified hostname]({{site.url}}/assets/Configure Gateway for First Time/90.png)

21. Type enter, leave empty for one or more name server
![Enter one or more name server]({{site.url}}/assets/Configure Gateway for First Time/95.png)

21. Type enter, leave empty for one or more search domain
![Enter one or more search domain]({{site.url}}/assets/Configure Gateway for First Time/100.png)

22. Type enter, don't change the current timezone for evaluating, you can change it if you want
![change the current timezone]({{site.url}}/assets/Configure Gateway for First Time/105.png)

23. Type enter, don't change the current timeservers configuration
![change the current timeservers configuration]({{site.url}}/assets/Configure Gateway for First Time/110.png)

24. Networking&time summary, type `y` to apply the changes
![network&time summary]({{site.url}}/assets/Configure Gateway for First Time/115.png)

25. Back to main menu, and select `R` to reboot the SSG appliance
![reboot SSG appliance]({{site.url}}/assets/Configure Gateway for First Time/120.png)

Create a new Layer7 Gateway database
--------------
26. Login with `ssgconfig`, select 2
![display layer 7 gateway configuration menu]({{site.url}}/assets/Configure Gateway for First Time/125.png)

27. select 2 `Create a new Layer 7 Gateway database`
![create a new layer 7 gateway database]({{site.url}}/assets/Configure Gateway for First Time/130.png)

28. Type enter(default is YES)
![database connection]({{site.url}}/assets/Configure Gateway for First Time/135.png)

29. Configure database host, type enter(default is localhost)  
![database host]({{site.url}}/assets/Configure Gateway for First Time/140.png)

30. Configure database port, type enter(default is 3306)  
![database port]({{site.url}}/assets/Configure Gateway for First Time/145.png)

31. Configure database name, type enter(default is ssg)  
![database name]({{site.url}}/assets/Configure Gateway for First Time/150.png)

32. Configure database username, type enter(default is gateway)
![database username]({{site.url}}/assets/Configure Gateway for First Time/155.png)

33. Configure database password, type `7layer` for evaluating  
![database user password]({{site.url}}/assets/Configure Gateway for First Time/160.png)

34. Configure database administrative user, type enter(default is `root`)
![database admin username]({{site.url}}/assets/Configure Gateway for First Time/165.png)

35. Configure database administrative user password, type `7layer`
![database admin user password]({{site.url}}/assets/Configure Gateway for First Time/170.png)

36. Type enter, don't configure database failover connection
![database failover connection]({{site.url}}/assets/Configure Gateway for First Time/175.png)

37. Configure SSM administrative user, type `admin` for evaluating
![SSM admin username]({{site.url}}/assets/Configure Gateway for First Time/180.png)

38. Configure SSM administrative password, type `7layer` for evaluating
![SSM admin password]({{site.url}}/assets/Configure Gateway for First Time/185.png)

39. Configure cluster hostname, type `g3.7layer.com` what used above
![cluster hostname]({{site.url}}/assets/Configure Gateway for First Time/190.png)

40. Configure cluster passphrase, type `7layer` for evaluating
![cluster passphrase]({{site.url}}/assets/Configure Gateway for First Time/195.png)

41. Enable the node, type enter  
![enable the node]({{site.url}}/assets/Configure Gateway for First Time/200.png)

42. Configuration summary, type enter if everything is fine
![configuration summary]({{site.url}}/assets/Configure Gateway for First Time/205.png)

43. Back to main menu, and select `R` to reboot the SSG appliance
![reboot SSG appliance]({{site.url}}/assets/Configure Gateway for First Time/210.png)


Test the gateway instance
--------------
44. Put the hostname mapping into the host machine which installed policy manager
![edit host]({{site.url}}/assets/Configure Gateway for First Time/215.png)

45. Try to connect the gateway instance by Policy Manager tool 
![try to connect the gateway]({{site.url}}/assets/Configure Gateway for First Time/220.png)

46. Connected successful  
![connected successful]({{site.url}}/assets/Configure Gateway for First Time/225.png)
