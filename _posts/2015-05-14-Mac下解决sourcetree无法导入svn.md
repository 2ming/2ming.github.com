---
layout: post
title: mac下解决souretree无法导入svn
---

{{ page.title }}
================

<p class="meta">14 May 2015 - mac下解决souretree无法导入svn</p>

<pre class="language-javascript">
Can't locate loadable object for module SVN::_Core in @INC (@INC contains: /usr/local/git/lib/perl5/site_perl /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/5.18.2/darwin-thread-multi-2level /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/5.18.2 /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl /Library/Perl/5.18/darwin-thread-multi-2level /Library/Perl/5.18 /Network/Library/Perl/5.18/darwin-thread-multi-2level /Network/Library/Perl/5.18 /Library/Perl/Updates/5.18.2 /System/Library/Perl/5.18/darwin-thread-multi-2level /System/Library/Perl/5.18 /System/Library/Perl/Extras/5.18/darwin-thread-multi-2level /System/Library/Perl/Extras/5.18 .) at /System/Library/Perl/Extras/5.18/SVN/Base.pm line 59.

BEGIN failed--compilation aborted at /System/Library/Perl/Extras/5.18/SVN/Core.pm line 5.
Compilation failed in require at /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/Git/SVN/Utils.pm line 6.

BEGIN failed--compilation aborted at /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/Git/SVN/Utils.pm line 6.

Compilation failed in require at /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/Git/SVN.pm line 26.

BEGIN failed--compilation aborted at /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/Git/SVN.pm line 33.

Compilation failed in require at /Applications/SourceTree.app/Contents/Resources/git_local/libexec/git-core/git-svn line 25.

BEGIN failed--compilation aborted at /Applications/SourceTree.app/Contents/Resources/git_local/libexec/git-core/git-svn line 25.
Completed with errors, see above
</pre>

首先确认是否安装过 CommandLineTools

打开终端，输入命令

<pre class="language-javascript">xcode-select --install</pre>

<img src="//wanggao421.github.com/images/20150514/1.png" alt="xcode">

选择“安装”，然后同意安装协议，等待完成

[参考网址](http://blog.csdn.net/sqc3375177/article/details/23662755)

如果是单独安装的，执行如下命令：

<pre class="language-javascript">sudo ln -s /Library/Developer/CommandLineTools/Library/Perl/5.18/darwin-thread-multi-2level/auto/SVN /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/5.18.2/darwin-thread-multi-2level/auto/</pre>

<pre class="language-javascript">sudo ln -s /Library/Developer/CommandLineTools/Library/Perl/5.18/darwin-thread-multi-2level/SVN /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/5.16.2/darwin-thread-multi-2level/</pre>

如果是通过Xcode安装的，执行：

<pre class="language-javascript">ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.18/darwin-thread-multi-2level/SVN /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/5.18.2/darwin-thread-multi-2level/</pre>

<pre class="language-javascript">ln -s /Applications/Xcode.app/Contents/Developer/Library/Perl/5.16/darwin-thread-multi-2level/auto/SVN /Applications/SourceTree.app/Contents/Resources/git_local/lib/perl5/site_perl/5.18.2/darwin-thread-multi-2level/auto/</pre>

这里要注意一个问题，看你的报错/Library/Perl/5.18 

<img src="//wanggao421.github.com/images/20150514/1.png" alt="xcode">

如果你的路径不是5.18 那么上面执行命令的时候把5.18／5.18.2改成你相对应上图红色标注的文件名

然后再次导入svn，你会发现，sourcetree终于可以愉快的玩svn了


