---
layout: post
title: How to create bookmark or link posts in Jekyll
date: 2020-07-05 16:40:03
---

My friend Chuck Grimmett wrote a great article several years ago on [how to create "link" posts in Jekyll.](http://www.cagrimmett.com/til/2016/06/10/external-post-links-jekyll.html)

A link post is basically just a post that appears in your regular blog or microblog feed that links to outside content rather than content on your site. That might be a bookmark for an article you want to read later, a link to an piece of content you liked, or something you created yourself.

In Chuck's case, he links out to articles he's written on other sites like Quora, Medium or his company blog.

Chuck accomplishes his linkblog by using a custom frontmatter variable and some conditional logic to render the <code>title:</code> variable in files in his <code>_posts</code> folder as external matching his <code>link:</code> variable. It works great and his tutorial is, as always, easy to follow.

As I've written before, there are many ways to accomplish the same thing in Jekyll, and after reviewing Chuck's approach, I realised that it wasn't going to work for all the different needs I had on my site for several reasons.

1. I didn't want to clog up my <code>_posts</code> folder with links to external sites. I like to keep different post types more organised. 

2. I wanted to make sure that no permalink page on my site was being rendered and I wasn't sure whether or not Chuck's approach essentially tricked Jekyll but still left a secret page on the site.

3. I wanted more granularity in tagging and categorizations, and keeping things in the same <code>_posts</code> folder seemed limiting.

The solution I settled on involved creating a special Jekyll collection called <code>links</code>.

```
collections:
  links:
    output: false
    permalink: /links/:year/:month/:day/:path/
```

Then I set the <code>output:</code> to <code>false</code> because I don't want a permalink rendered on the site for my link posts right now. In the future, if I want to change that, I can just set it to <code>true</code> and I'll have the permalinks created according the settings I have in my <code>permalink:</code> variable.

I created a layout for my links in my <code>_layouts</code> folder but didn't change anything from the default layout. I like to have seperate layouts for all my post types, even if the layouts are identical, because it gives me easier ability to edit layouts in the future for particular post types if I decide I want to customize it.

Next I created my <code>_links</code> folder in which I'll be posting all my link posts. You can use whatever file structure you want. I like to make my a copy of the date of the post. For example, a post on 2020-06-16 would be filed as <code>/_links/20200616.md</code>.

The frontmatter I use for the link posts is below. 

```
---
layout: link
date: 2020-06-16 12:36:19
title: The Velocity of Circulation
label: Mises Institute
link: https://mises.org/library/velocity-circulation
---
```

I use the <code>link:</code> variable to tell Jekyll the site my post is linking to. The <code>title:</code> variable is the name of the article or piece of content. The <code>label:</code> variable is the author or site name.

If you want content or commentary on the link you can add content to the file below the frontmatter.

Next we need a way to display all the link posts on a page in chronological order and the optional content. I created a links.html page and write the following liquid code for it.

{% raw %}

```
{% assign links = site.links | sort: 'date' | reverse %}
{% for links in links %}

<div class="h-entry note post-stub">
 
 
 <h2 class="post-stub"><a href="{{ links.url | relative_url }}">
   {{ links.title }} | {{ links.label }} 
   </a> → </h2>

 {% if links.content %}

 
 <p class="p-content"> {{ links.content }}
 </p>
   {% endif %}

 
</div>

{% endfor %}
```
{% endraw %}

We could stop here, but one of the advantages of Chuck's original tutorial is that we can display the link posts in the same chronological feed as the posts on our site. I wanted to the do the same, so I used the <code>side.documents</code> variable and then created a filter based on the layouts I was using.

I wrote a short tutorial about displaying collections AND <code>_posts</code> files on the same feed, which will [walk you through the steps.](https://derykmakgill.github.io/drw/2020/07/04/site-documents.html)
