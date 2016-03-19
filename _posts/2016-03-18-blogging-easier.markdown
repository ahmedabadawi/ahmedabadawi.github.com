---
layout: post
title:  "Blogging Just Got Easier!!"
date:   2016-03-18 21:37:19 +0200
categories: jekyll update
---
Throughout the years, I have been looking for an easy, developer-mind compatible,
means of blogging, created a couple of blog sites on different blogging engines
but they weren't comfortable until I stumbled upon GitHub Pages and after some
digging found my way through Static Site Generators and reached Jekyll. 
The whole package is, at least from my perspective, a developer friendly way of
blogging. Uses the same way most of the development tools I'm using, from
command line creation of a templated blog project, just like Yeoman, Cordova,
Maven... etc, and allows for writing the content in a similar way that the typical
README file is written (Markdown) so now I can use my favorite editor **VIM**
and still write very catchy blogs and yet maintain all the changes on **GIT**.
The combination for me is just right, so let me share my first time blogging
experience using GitHub Pages, Jekyll, and VIM, all within my beloved shell :)

## GitHub Pages
GitHub offers means to create websites, yes, I'm a GitHub user since 2012 and
created multiple repositories for different purposes but I just knew that there
is this wonderful feature, not sure exactly when it was introduced, I might not
be that late after all. Simply the idea is that each repository you have can
host a website representing that repository, for example if you have a repository
called Project1 then you can have a public website that might contain details
about your project and maybe documentation or a wiki, that site can be accessed
by *http://username.github.io/repo*.
Just a small convention to allow for website creation is to create a branch
named **gh-pages** and push your website to it under the repository. 
What about a site that talks about *me*, not a specific repository? You can
create a special repository and name it **username.github.io** and start
pushing your awesome site and it will be accessible directly by
*http://username.github.io* and you can even have a custom domain to point to
your new website and all hosted by GitHub and changes are immediately available
online with every push to the repository. It is just *Awesome*. Actually, I
started a pet project like 4 years ago but it didn't see the light, it was
basically a way for me and my team to have a clean way of accompanying the
source code with the documentation, **TechieWiki** and have all the searching
and cross-referencing and custom rendering techniques in it but ended up with a
folder full of markdown files side-by-side with the code and tests and had a
rule that each developer should have at least one commit on the wiki folder per
sprint, and we were using **GitHub-Markdown-Preview** to serve the wiki pages
and we called it a day.

## Jekyll
I can't say it better, as per [Jekyll Website](https://jekyllrb.com/) "Transform your plain text into static websites and blogs" Starts from the command line with a simple *new* command and ends with a complete website. Jekyll offers features can't even list them here, for me I'm just scratching the surface, it is a complete blog-aware static-site-generator with support for *Markdown* as the content markup language. I am planning to have another post about *Markdown* and its features and different flavors. 
Jekyll also gives the flexibility for the hardcore web developer in you to use
the ninja tools, SASS, CSS, and you can use bootstrap, Yeoman, complete freedom.
Next, I will be talking about the easy steps to build your blog site using this
stack.

## Few steps, yet great outcome (terminal is your friend here)

1. Start with creating a repository on GitHub
Or maybe an existing project repository that you want to have a website to
describe its awesomeness.

2. Install Jekyll
You should have ruby and rubygems installed in order to install and use Jekyll.

{% highlight shell %}
gem install jekyll
{% endhighlight %}

3. Create your blog site
Like most of the modern development tools, npm, yeoman, ... etc. from the shell you
can generate an initial folder structure and setup for your blog site

{% highlight shell %}
jekyll new <blog>
{% endhighlight %}

4. Initialize your Git local repository
Next you need to initialize the blog folder as a git repository and have your
first commit

{% highlight shell %}
cd <blog>
git init    # initialize the current folder as a git local repository
git add .   # prepare a commit by adding all files in the current folder
git commit -m "Empty Jekyll blog site"  # commit the initial structure
{% endhighlight %}

5. Link your local repository with GitHub
Connect to your GitHub repository

{% highlight shell %}
git remote add origin <github_repository_url>
git remove -v       # verify that the remote origin has been set
{% endhighlight %}

6. Create the special GitHub Pages branch and push your commit
As mentioned earlier, GitHub Pages uses a special branch for automatic
publishing **gh-pages**. You can definetly use a draft branch and merge into the
publishing branch when your posts are in good shape to be published but that's
beyond the scope of this post.

{% highlight  shell %}
git checkout gh-pages   # create the gh-pages and switch to it
git branch              # view branches and verify the current branch
git push remote gh-pages    # push the commit to the gh-pages branch
{% endhighlight %}

7. Congratulations... now you have a blog site
At this point you have a minimalist blog site creating by the Jekyll templating
and now it is live on the internet, you can verify the site by visiting
**http://username.github.io/repo**

8. Writing your first post *like this one*
Showtime... Now you will create your first blog post, Jekyll is a blog-aware
static-site-generator which means it understands posts, tags, categories, etc.
You will find a special folder in the generated folder structure called
`_posts`, Jekyll iterates through this special folder looking for *markdown*
files formatted as `yyyy-mm-dd-postname.markdown` and for each post it builds
collections that can be used on the website to create dynamic links to the
posts, for example in the homepage you can have all the posts lists with their
date, tags, and event an excerpt. Spin up your favorite text editor (VIM for
me), and start writing your first blog or like what I did, I copied the sample
post in the posts folder and started editing inside it. Then you can commit and push your
changes so it is reflected directly to your newly created blog site but first,
let's preview your site locally before publishing it to the world.

{% highlight shell %}
jekyll serve
{% endhighlight %}

You will find Jekyll doing its magic and generating your static site in a
special folder `_site` and opens a lightweight web server for you to preview
your changes / new posts. Usually, the server starts on port 4000 so you can
navigate to **http://localhost:4000/repo** to preview your changes. With some back
and forth cycles between your text editor and the preview, you have your post
ready. But what about all the defaults in there, let's add some personalization
to the website.

9. Personalize your site
Jekyll has a special file called `_config.yml`, open it in your text editor and
edit the defaults to adding your identity to the website, save it and maybe give it
another look through the preview. BTW you don't have to close the server and
restart it every time you make any changes, instead, you can make the web server
listen for changes, but any changes to the `_config.yml` are not reflected
automatically, you have to restart the web server to reflect your changes.

{% highlight shell %}
jekyll serve --watch
{% endhighlight %}

Also, you can unleash the web developer inside and start updating the HTML
templates and the CSS but that is beyond this post.

10. Ready, set, **push**
Now you have your personalized website and your first blog post, here comes my
favorite part, no different than any coding project you are working on, commit
your code to source control and the continuous integration works its magic.

{% highlight shell %}
git add .
git commit -m "First blog post"
git push origin gh-pages
{% endhighlight %}

Now go back to your public URL and watch your website live.

## Beyond the basics
This post so far hasn't dug deep into any of the aspects of making your blog
site really awesome. Next, you need to dig more into **markdown** and make use of
its features and dig more into the Jekyll templating to customize the appearance
and details of your site, also, you can start checking different UI templates and
customize to make something totally unique for you or your organization.
Speaking of organization, this can be used on the organization GitHub account
and have a moderator and allow everybody to fork and work then request pulling
posts into your organization's main blog site and having content approval cycle
for instance. Ideas are limitless, I'm just
scratching the surface.

** Happy Blogging **

## Refrences
- [GitHub Pages](https://pages.github.com/)
- [Jekyll Website](https://jekyllrb.com/)
- [Daring Fireball Markdown](https://daringfireball.net/projects/markdown/basics).

