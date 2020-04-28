# WordPress去除SSR功能和脚注

**进入wordpress安装目录,然后修改如下文件内容**

```shell
vi wp-includes/widgets/class-wp-widget-meta.php
```

进入目录之后注释如图所示部分

```shell
 <li><a href="<?php echo esc_url( get_bloginfo( 'rss2_url' ) ); ?>"><?php _e('Entries <abbr title="Really Simple Syndication">RSS</abbr>'); ?></a></li>
<li><a href="<?php echo esc_url( get_bloginfo( 'comments_rss2_url' ) ); ?>"><?php _e('Comments <abbr title="Really Simple Syndication">RSS</abbr>'); ?></a></li>
```

![](E:\TyporaText\框架\wordpress\images\SSR注释部分.PNG)

注释之后保存退出即可。

---

**去除掉主题作者留下的注解**

进入wordpress的安装目录，修改如下文件

```shell
vi wp-content/themes/'主题名称'/footer.php;
```

进入之后，注释掉如下图所示部分

![](E:\TyporaText\框架\wordpress\images\关闭脚注部分.PNG)

每个主题都有所不同，可以看页面的html代码，去对比找到，如下图

![](E:\TyporaText\框架\wordpress\images\html脚注展示.PNG)

注释之后保存退出即可。