---
layout: post
category: tutorials
slug: hr example
---

# Setup Ikiwiki with docker thttpd 

## Installation on Ubuntu or Debian

```bash
  # install on ubuntu
  $ apt-get install ikiwiki
```

## Create
Create your wiki

All it takes to create a fully functional wiki using ikiwiki is running one command.

For more control, advanced users may prefer to set up a wiki by hand.

```bash
  % ikiwiki --setup /etc/ikiwiki/auto.setup
```


Or, set up a blog with ikiwiki, run this command instead.

```bash
  % ikiwiki --setup /etc/ikiwiki/auto-blog.setup
```

`librpc-xml-perl` and `python-docutils` dependencies are needed.

Either way, it will ask you a couple of questions.

```
What will the wiki be named? foo
What revision control system to use? git
What wiki user (or openid) will be admin? joey
Choose a password:

Then, wait for it to tell you an url for your new site..

Successfully set up foo:
    url:         http://example.com/~joey/foo
    srcdir:      ~/foo
    destdir:     ~/public_html/foo
    repository:  ~/foo.git
To modify settings, edit ~/foo.setup and then run:
    ikiwiki --setup ~/foo.setup
```

Done!

## Run server

To run compiled files on `thttpd` server execute next command.

```bash
docker run -v /home/$USER/public_html/blog:/content -p 80:80 larsks/thttpd -d /content
```

Now, open your web browser on  `chrome.exe  http://$(curl -s ifconfig.me)` or manually open explorer on `http://127.0.01/`

Done!

## Screenshots

**Blog**

![blog creado con ikiwiki](https://i.imgur.com/Yjij297.png)

### Customizing the wiki

There are lots of things you can configure to customize your wiki. These range from changing the wiki's name, to enabling plugins, to banning users and locking pages.

If you log in as the admin user you configured earlier, and go to your Preferences page, you can click on "Setup" to customize many wiki settings and plugins.

Some settings cannot be configured on the web, for security reasons or because misconfiguring them could break the wiki. To change these settings, you can manually edit the setup file, which is named something like "foo.setup". The file lists all available configuration settings and gives a brief description of each.

After making changes to this file, you need to tell ikiwiki to use it:

```bash
% ikiwiki --setup foo.setup
```

Alternatively, you can ask ikiwiki to change settings in the file for you:

```bash
% ikiwiki --changesetup foo.setup --plugin goodstuff
```

See [usage][1] for more options.

## Customizing file locations

As a wiki compiler, ikiwiki builds a wiki from files in a source directory, and outputs the files to a destination directory. The source directory is a working copy checked out from the version control system repository.

When you used auto.setup, ikiwiki put the source directory, destination directory, and repository in your home directory, and told you the location of each. Those locations were chosen to work without customization, but you might want to move them to different directories.

First, move the destination directory and repository around.

```bash
% mv public_html/foo /srv/web/foo.com
% mv foo.git /srv/git/foo.git
```

If you moved the repository to a new location, checkouts pointing at the old location won't work, and the easiest way to deal with this is to delete them and re-checkout from the new repository location.

```bash
% rm -rf foo
% git clone /srv/git/foo.git
```

Finally, edit the setup file. Modify the settings for srcdir, destdir, url, cgiurl, cgi_wrapper, git_wrapper, etc to reflect where you moved things. Remember to run ikiwiki --setup after editing the setup file.

## Resources

Next resources can be used for customize site

- **CSS market for Ikiwiki sites**: https://ikiwiki.info/css_market/
- **Ikiwiki Themes**:
	- https://ikiwiki.info/themes/
	- https://ikiwiki.info/theme_market/


[1]: https://ikiwiki.info/usage/
[2]: https://ikiwiki.info/tips/dot_cgi
[3]: https://hub.docker.com/r/larsks/thttpd
