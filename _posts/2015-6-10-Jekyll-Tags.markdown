---
layout: post
title:  "为Jekyll添加Tag功能"
date:   2015-6-10
tags:
- jekyll
- tag
---

<p class="intro">折腾了一天配置jekyll后，终于部署在github上了，并且本地的jekyll也能跑起来了。于是就想加点功能什么的...</p>

Windows上配置jekyll稍微有点蛋疼，蛋疼的问题不是装不上软件，而是...我大天朝的局域网...下个东西简直太慢了，而且还老断线。（其实ubuntu也一样，最慢的一部是gem install jekyll，连了4次才下下来第一个包，其他几次全都提示连接s3.amazonws超时...）


Jekyll的确是个轻量级的blog系统，不需要会ruby开发，只要gem install好后弄个模板，写写html代码就行了。但是收纳星人看着没法归档的日志不太爽，于是就狗哥了一下如何给jekyll加tag。



采用了[Tags-in-jekyll][TIJ]文中的代码（我真的是低级搬运工啊...），把tag_gen.rb放到_plugins/下,在为tag页面写一个通用的模板tag_index.html放到_layout/下就行了。代码如下：


tag_gen.rb:
{% highlight ruby %}
# tag_gen.rb
# Code grabbed from http://charliepark.org/tags-in-jekyll/
module Jekyll
  class TagIndex < Page
    def initialize(site, base, dir, tag)
      @site = site
      @base = base
      @dir = dir
      @name = 'index.html'
      self.process(@name)
      self.read_yaml(File.join(base, '_layouts'), 'tag_index.html')
      self.data['tag'] = tag
      tag_title_prefix = site.config['tag_title_prefix'] || 'Posts Tagged &ldquo;'
      tag_title_suffix = site.config['tag_title_suffix'] || '&rdquo;'
      self.data['title'] = "#{tag_title_prefix}#{tag}#{tag_title_suffix}"
    end
  end
  class TagGenerator < Generator
    safe true
    def generate(site)
      if site.layouts.key? 'tag_index'
        dir = site.config['tag_dir'] || 'tag'
        site.tags.keys.each do |tag|
          write_tag_index(site, File.join(dir, tag), tag)
        end
      end
    end
    def write_tag_index(site, dir, tag)
      index = TagIndex.new(site, site.source, dir, tag)
      index.render(site.layouts, site.site_payload)
      index.write(site.dest)
      site.pages << index
    end
  end
end
{% endhighlight %}



tag_index.html:

将代码中的"{.%"和"{.{"中的"."全部去掉。
{% highlight html %}
---
layout: default
---
<h2 class="post_title">{{page.title}}</h2>
<ul>
  {.% for post in site.posts %}
  {.% for tag in post.tags %}
  {.% if tag == page.tag %}
  <li class="archive_list">
    <time style="color:#666;font-size:11px;" datetime='{.{post.date | date: "%Y-%m-%d"}}'>{.{post.date | date: "%m/%d/%y"}}</time> <a class="archive_list_article_link" href='{.{post.url}}'>{.{post.title}}</a>
  </li>
  {.% endif %}
  {.% endfor %}
  {.% endfor %}
</ul>
{% endhighlight %}

最后，为了让文章里也能显示tag标签，对_layouts\post.html做了一点修改,给tag加了一个小div

post.html:

同上，去掉"."
{% highlight html %}

{.{content}}
<!-- Begins after content -->
<div class="tags">
<div class="container">
<h5>Tags</h5>
	  <li class="archive_list">
		<ul class="archive_list">
		  {.% for tag in page.tags %}
		  <a class="tag_list_link" href="/tag/{.{ tag }}">{.{ tag }}</a></li>
		  {.% endfor %}
		</ul>
	  </li>
   </div>
</div><!-- end .tags -->
<!-- Ends before postNav -->
<div class="postNav clearfix">
{% endhighlight %}


最后，在日志的YAML段加上想要的tag
{% highlight html %}
---
layout: post
title:  "为Jekyll添加Tag功能"
date:   2015-6-10
tags:
- jekyll
- tag
---
{% endhighlight %}
All done.
[TIJ]: http://charliepark.org/tags-in-jekyll/
