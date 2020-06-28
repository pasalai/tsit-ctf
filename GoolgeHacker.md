---
title: GoolgeHacker
date: 2020-04-16 08:43:58
tags: [Google,渗透测试,信息搜集 ]
categories: [笔记,渗透测试 ]
---
# Goolge Hacker 语法整理
## 概述
* intitle：搜索网页标题中包含有特定字符的网页。
* inurl：搜索包含有特定字符的URL。例如输入“inurl:/admin_login”，则可以找到带有admin_login字符的URL,通常这类网址是管理员后台的登录网址。
* intext:搜索网页正文内容中的指定字符。
* Filetype:搜索指定类型的文件。例如输入“filetype:PDF”，将返回PDF文档。
* Site：找到与指定网站有联系的URL。例如输入“Site:www.sunghost.cn”。所有和这个网站有联系的URL都会被显示。
<!--more-->
## 详解
### intitle
intitle语法通常被用来搜索网站的后台、特殊页面和文件，通过在Google中搜索“intitle:登录”、“intitle:管理”就可以找到 很多网站的后台登录页面。此外，intitle语法还可以被用在搜索文件上，例如搜索“intitle:"indexof"etc/shadow”就可以 找到Linux中因为配置不合理而泄露出来的用户密码文件。
### inurl
Google Hack中，inurl发挥的作用的最大，主要可以分为以下两个方面:寻找网站后台登录地址，搜索特殊URL。
寻找网站后台登录地址：和intitle不同的是，inurl可以指定URL中的关键字，我们都知道网站的后台URL都是类似login.asp、 admin.asp为结尾的，那么我们只要以“inurl:login.asp”、“inurl:admin.asp”为关键字进行搜索，同样可以找到很 多网站的后台。此外，我们还可以搜索一下网站的数据库地址，以“inurl:data”、“inurl:db”为关键字进行搜索即可。
寻找网站的后台登录页面
搜索特殊URL：通过inurl语法搜索特殊URL，我们可以找到很多网站程序的漏洞，例如最早IIS中的Uncode目录遍历漏洞，我们可以构造 “inurl:／winnt／system32／cmd exe?／c+dir”这样的关键字进行搜索，不过目前要搜索到存在这种古董漏洞的网站是比较困难的。再比如前段日子很火的上传漏洞，我们使用 ““inurl:upload.asp”或“inurl:upload_soft.asp”即可找到很多上传页面，此时再用工具进行木马上传就可以完成入 侵。
### intext
intext的作用是搜索网页中的指定字符，这貌似在Google Hack中没有什么作用，不过在以“intext:to parent directory”为关键字进行搜索后，我们会很惊奇的发现，无数网站的目录暴露在我们眼前。我们可以在其中随意切换目录，浏览文件，就像拥有了一个简 单的Webshell。形成这种现象的原因是由于IIS的配置疏忽。同样，中文IIS配置疏忽也可能出现类似的漏洞，我们用“intext:转到父目录” 就可以找到很多有漏洞的中文网站。

> 随意浏览网站中的文件

### Filetype
Filetype的作用是搜索指定文件。假如我们要搜索网站的数据库文件，那么可以以“filetype:mdb”为关键字进行搜索，很快就可以下载到不少网站的数据库文件。当然，Filetype语法的作用不仅于此，在和其他语法配合使用的时候更能显示出其强大作用。
### Site
黑客使用Site，通常都是做入侵前的信息刺探。Site语法可以显示所有和目标网站有联系的页面，从中或多或少存在一些关于目标网站的资料，这对于黑客 而言就是入侵的突破口，是关于目标网站的一份详尽的报告。语法组合，威力加倍虽然上文中介绍的这几个语法能各自完成入侵中的一些步骤，但是只使用一个语法 进行入侵，其效率是很低下的。Google Hack的威力在于能将多个语法组合起来，这样就可以快速地找到我们需要的东西。下面我们来模拟黑客是如何使用Google语法组合来入侵一个网站的。
### 信息刺探
黑客想入侵一个网站，通常第一步都是对目标网站进行信息刺探。这时可以使用“Site:目标网站”来获取相关网页，从中提取有用的资料。
### 搜索相关页面
下载网站的数据库。搜索“Site:目标网站 Filetype:mdb”就可以寻找目标网站的数据库，其中的Site语法限定搜索范围，Filetype决定搜索目标。用这种方法有一个缺点，就是下载到数据库的成功率较低。在这里我们还可以采用另一种语法组合，前提是目标网站存在IIS配置缺陷，即可以随意浏览站点文件夹，我们搜索“Site:目标网站 intext:to parent directory”来确定其是否存在此漏洞。在确定漏洞存在后，可以使用“Site:目标网站 intext:to parent directory+intext.mdb”进行数据库的搜索。

### 找到网站数据库
登录后台管理下载到数据库后，我们就可以从中找到网站的管理员帐户和密码，并登录网站的后台。对于网站后台的查找，可以使用语法组合“Site:目标网站 intitle:管理”或者“Site:目标网站 inurl:login.asp”进行搜索，当然我们可以在这里进行联想，以不同的字符进行搜索，这样就有很大的
概率可以找到网站的后台管理地址。接下去黑客就可以在后台上传Webshll，进一步提升权限，在此不再阐述。

### 利用其他漏洞
如果下载数据库不成功，我们还可以尝试其他的入侵方法。例如寻找上传漏洞，搜索“Site:目标网站 inurl:upload.asp”。此外，我们还可以根据一些程序漏洞的特征，定制出Google Hack的语句。
Google Hack可以灵活地组合法语，合理的语法组合将使入侵显得易如反掌，再加入自己的搜索字符，Google完全可以成为你独一无二的黑客工具。

### 合理设置，防范Google Hack
Google Hack貌似无孔不入，实则无非是利用了我们配置网站时的疏忽。例如上文中搜索“intext:to parent directory”即可找到很多可以浏览目录文件的网站，这都是由于没有设置好网站权限所造成的。在IIS中，设置用户访问网站权限时有一个选项，叫做“目录浏览”，如果你不小心选中了该项，那么其结果就如上文所述，可以让黑客肆意浏览你网站中的文件。这种漏洞的防范方法十分简单，在设置用户权限时不要选中“目录浏览”选项即可。


## 黑客常用的语法 
```
admin site:edu.cn
site:sunghsot.cn intext:管理|后台|登录|用户名|密码|验证码|系统|账号|后台管理|后台登录
site:sunghsot.cn intitle:管理|后台|登录|用户名|密码|验证码|系统|账号|后台管理|后台登录
inurl:login/admin/manage/admin_login/login_admin/system/boos/master/main/cms/wp-admin/sys|managetem|password|username
site:www.sunghost.cn inurl:file
site:www.sunghost.cn inurl:load
site:www.sunghost.cn inurl:php?id=
site:www.sunghost.cn inurl:asp?id=
site:www.sunghost.cn inurl:fck
site:www.sunghost.cn inurl:ewebeditor
inurl:ewebeditor|editor|uploadfile|eweb|edit
intext:to parent directory
intext:转到父目录/转到父路径
inurl:upload.asp
inurl:cms/data/templates/images/index/
intitle:powered by dedecms
index of/ppt
Filetype:mdb
site:www.sunghost.cn intext:to parent directory+intext.mdb
inurl:robots.txt 
intitle:index.of "parent directory"  
index of /passwd
site:sunghost.cn filetype:mdb|ini|php|asp|jsp 
```
## 其他相关指令：
* related，cache，info，define，link，allinanchor等。 

