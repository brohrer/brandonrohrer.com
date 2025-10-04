# Traffic Control

The internet is a pretty rough and tumble place to hang out.
It's not even that there are so many jerks out there, it's just that
all of them can reach you at once.
Luckily you have the power to shut them down.

This installment focuses on blocking IP addresses that are doing things
they shouldn't. It's the lastest in a series of posts on

1. [Setting up a webserver](hosting.html)
1. [Setting up a domain name](hosting2.html)
1. [Setting up some security](hosting3.html)
1. [Keeing a webserver healthy](hosting4.html)

## Block an IP address

The most straightforward way to block and IP address is in the firewall.
It is the tool build specifically for this.

To block the address `101.101.101.101`, run from the command line

```
sudo ufw insert 1 deny from 101.101.101.101
```

This instructs ufw (the Uncomplicated FireWall) to insert a rule at the
the top of the list (position 1) to deny all incoming traffic from
the address. After running this, no restart of the firewall is needed.
The rule is active.

The position 1 is important because in ufw, the first rule that matches
is applied. If there was a rule to allow all addresses that started
with `101.` and that rule came before the deny rule, then the deny
rule would never be reached.

While it's possible to block specific ports, or even to block an
IP address from seeing particular pages, complex rules
and conditions get difficult to analyze very quickly, and can lead to cases
where there are loopholes. Use fancy rule combinations sparingly.

## Parse logs

In their raw form access logs are technically human-readable, but they are
a lot. I found it really useful to do a little parsing to pull out the bits
I'm interested in.
(I'm working with the default nginx log format, so adjust this
according to your own.)

I [wrote a script](https://codeberg.org/brohrer/webserver-toolbox/src/branch/main/log_reader.py)
to take advantage of the repeatable structure of these logs
to dissect them into their parts. It uses tricks like splitting the log
based on brackets and spaces. It productes a pandas dataframe with
columns containing the IP address, requested URI, HTTP status code,
and every component of the date and time.

If you use exactly the same setup and log format as I do you might be
able to get away with using this code right out of the box. More likely
you'll have to (or want to) adjust it a bit for your own circumstances.
Make it your own!

This better organized version of the log can be used to generate a list
of pages that were queried, a list of IP addresses that visited the site,
or even just a chronological list of every access attempt.

Here's a
[pages.py](https://codeberg.org/brohrer/webserver-toolbox/src/branch/main/pages.py)
script that shows how many times pages were hit in a given day, for example

![Sample output from pages.py
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/hosting/pages_outputs.png "Count and page names")

Here's a 
[ips.py](https://codeberg.org/brohrer/webserver-toolbox/src/branch/main/ips.py "Count and IP address")
script that shows how many times a particular IP address visited that day

![Sample output from ips.py
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/hosting/ips_outputs.png)

Here's a
[history.py](https://codeberg.org/brohrer/webserver-toolbox/src/branch/main/history.py)
script that just repeats the access logs directly, but in a stripped down format that's easier to read.

![Sample output from history.py
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/hosting/history_outputs.png "Time, HTTP status, IP address, and page name")

## Browsing the logs

At first glance these logs are just an unbroken wall of text, but after
spending a couple of minutes with them, oddities emerge.

Looking at the IP address access count, why is there one address that accessed
the website 633 times? That's more than four times the next most frequent.
What was happening there? Surely that can't be legit, can it?

Looking at the access history, why was someone trying to find a
2016 New York Times article on this website? (The funny characters that come
before are a utf-8 encoding of a unicode right double quotation mark.)
That looks like someone was really flailing.

Looking at the pages viewed, once you get past the stylesheets,
javascripts, feed.xml., and robots.txt, there is the top-level blog,
a popular post about transformers which I put a ton of effort into, and
then `ssh_at_home`, a hastily-written set of notes about setting
up an ssh server for personal use. Why is that one so regularly visited?

The longer you look, the more questions arise. Some of the most
interesting patterns come from people doing mischief.

## Bad behavior #1: Scanning for secrets

![Access log history showing scanning for secrets
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/hosting/scanning_secrets.png "Looking for juicy passwords and tokens")

Here, a single IP address quickly tries to access a handful of variations
on a `.env` file, which is a common place to store access tokens
and other credentials. There is no good reason to be doing this, unless
you are doing it to yourself, proactively scanning for vulnerabilities
in order to fix them.

In my judgment this is a one-strike-and-you're out behavior. This IP
address will get added to my block list.
Luckily we already blocked access to all dot-files in the nginx server block
during initial setup. That's why these are resulting in a 403 status code
(access denied) rather than a 404 (not found). But if someone is doing this
they may be looking for other ways into your system. Why not just block
them at the firewall?

## Bad behavior #2: Fishing for files

![Access log history showing fishing for php files
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/hosting/fishing_files.png "Someone really wants my website to be written in php")

In this segment, a single IP address is trying to find a
php file, and they appear to be randomly casting about. These files
don't exist on my webserver and never have. This requestor appears to 
be shooting in the dark for files that they do not have a link to.
I don't know why, but I don't like it. I have all
php files blocked in my server block, but still this behavior shows someone
doing something other than browsing my website, which leads me not
to trust them. For me, this is a blockable offense.

Other file fishing I see often is for WordPress-related files.

![Access log history showing fishing for WordPress files
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/hosting/fishing_wordpress.png "Someone really wants my website to be written in WordPress")

I also get requests trying to access links from my pages, as if they were
hosted on my server, such as 
`/%20%20%20%20%20%20%20%20%20%20%20%20https:/en.wikipedia.org/wiki/Convolution`.
The `%20` is URL encoding for a space character. I also don't know why 
there are 12 spaces in front of the URL. It looks like sloppy automated
parsing of a bot accessing links that were extracted from my html files.
I don't know what benign purpose this could serve. I'm content to
block these as well.

With the history browser there is a `--status` argument that lets you
quickly see just the 404's and 403's and helps the file fishing to pop out.

```
uv run history.py --status 404
```

## Bad behavior #3: 

A lot of requests in quick succession. And annoyance.




-------

whois 

https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands



sudo vi /etc/ufw/block_ips.sh
sudo sh /etc/ufw/block_ips.sh

