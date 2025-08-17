# How to start an RSS feed

Before Twitter, before LinkedIn, before Facebook, there was
[RSS](https://en.wikipedia.org/wiki/RSS).
Really Simple Syndication is the photocopied 'zine of microblogging.
If there was social media in Mad Max, it would be RSS. It's
totally outside any centralized control, cheap, gritty, and punk af. 

## Reading RSS

The toughest thing to get used to is that there is no central platform.
RSS is just a bunch of people creating RSS-formatted files and posting them
on the internet. The burden of assembling them into a reading list falls
on the reader. Thankfully there **aggregators**, helpful programs that regularly
check those RSS files for changes and lay them out for you in a feed.

I use [Feedly](https://feedly.com) daily, and enjoy
[Inoreader](https://www.inoreader.com) too. I've also heard that 
[Feeder](https://feeder.co) and 
[NewsBlur](https://newsblur.com) are solid options. There are plenty
of others, some with niche functionality. The cool part is that there isn't
a "main" one or a home base. They're all their own thing. All they do
is gather up changes and put them in a list for you.

To subscribe to someone's feed, you'll need to get the URL. These are
frequently posted on their blog under an "RSS feed" link or the
RSS logo.

![The RSS logo, a dot at the center of two quarter-circles, 
giving the appearance of waves radiating out from a central point
](https://upload.wikimedia.org/wikipedia/en/4/43/Feed-icon.svg)

The URL may be inscrutable. For my blog it looks like

```https://brandonrohrer.com/feed.xml```

For an example feed created for this post, it looks like

```https://raw.githubusercontent.com/brohrer/blog/refs/heads/main/code/example_feed.xml```

This is the key to subscribing. Copy the URL and paste it into the
field for 
"Add channel" or "Follow feeds" or whatever other name your aggregator uses.
And from then on your aggregator will revisit that URL occasionally,
checking for new content, and add it to your feed.

![A Follow Sources dialog page, with a field for pasting a feed URL
and below that a list of feeds that match the URL
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/rss/follow_sources.png "A Follow Sources dialog from Feedly")

![An overview of the feed, showing channel title and a thumbnail
view of the first post
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/rss/feed_overview.png "A channel overview on Feedly")

![An example post with the title Post Content and some text reading 
This can be any html.
followed by a vintage drawing of Christopher Robin reading to 
Winnie the Pooh, who is stuck in a hole.
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/rss/example_post.png "An example post, viewed on Feedly")

Most aggregators also have a way to explore the more popular feeds.

[A list of 15 industries and a handful skills as topics to search for feeds
](https://raw.githubusercontent.com/brohrer/blog_images/refs/heads/main/rss/feed_topics.png "Some popular feed topics, offered on Feedly")

## Writing RSS

If the process of following RSS feeds seems duct-tape-and-bailing-wire,
you'll love writing them.

### 1. Find a place to host it

Aggregators need to be able to find your feed .xml file on the internet.
Any place you can upload a text file and share it with the world should do.
One free option is [GitHub](https://www.github.com) if that's a tool
you're familiar with. Here's 
[the .xml file for the example feed
](https://github.com/brohrer/blog/blob/main/code/example_feed.xml)
which is on GitHub.

```$<$rss version="2.0"$>$
  $<$channel$>$
    $<$title$>$Your Feed$<$/title$>$
    $<$link$>$https://www.brandonrohrer.com$<$/link$>$
    $<$description$>$Your Blog's name$<$/description$>$ <br>
    $<$item$>$
      $<$title$>$Blog title$<$/title$>$
      $<$link$>$https://www.brandonrohrer.com/rss.html$<$/link$>$
      $<$pubDate$>$Tue, 16 Aug 2025 12:31:00 EDT$<$/pubDate$>$
      $<$guid$>$https://www.brandonrohrer.com/rss.html_02$<$/guid$>$
      $<$description$>$$<$![CDATA[
        $<$h1$>$Post content$<$/h1$>$
        $<$p$>$
          This can be any html.
        $<$/p$>$
        $<$img src="https://upload.wikimedia.org/wikipedia/commons/9/97/Winnie-the-Pooh_45-1.png"$>$
      ]]$>$$<$/description$>$
    $<$/item$>$ <br>
  $<$/channel$>$
$<$/rss$>$```

You can copy this directly into your own feed.XML and modify it. 
A trick to remember with GitHub is that the link to your feed will actually
be the "raw" link, which is available from the icon on the right side
of the screen when looking at the file in GitHub.

There are two major sections, the `channel` information at the top,
then information for each `item` below that.

### 2. Add channel information

- `title` is the channel name. It can be anything you want. It’s what
people will see when they pull up your channel in their aggregator.
- `link` is a website associated with your channel. For me, it's the
landing page of my blog.
- `description` is typically a one-line explanation of what readers can
expect to see in your feed.

There are [lots of other elements
](https://www.rssboard.org/rss-specification#optionalChannelElements)
you can add here if you like, but these are the required ones.

### 3. Add item information

Once you have the channel info in place you can add an item.
An item needs a few basic pieces of information.

- `title` is the name of the particular post. 
- `link` is a URL associated with it.
- `pubDate` is [a date in the format of
](https://whitep4nth3r.com/blog/how-to-format-dates-for-rss-feeds-rfc-822/#valid-rfc-822-date-format)
`Tue, 16 Aug 2025 12:31:00 EDT`. It shows up at the top of a post as
the publication date.
- `guid` (globally unique identifier) is a string that aggregators can use
as an ID for this post. I find it useful for when I update the content
of the post and I want aggregators to re-load it on their next pass.
Changing the guid signals to the aggregator that the post needs to be
re-loaded.
- `description` is the body of the post. It can be a one-line teaser
for the linked content or it can be an entire novel. Everything
between the `![CDATA[` and `]]` delimiters will be interpreted as straight
html, which is super useful if you want to do pretty formatting or
include images. Not all aggregators will interpret all html tags
(for instance `$<$script$>$`
[is likely to get skipped](https://validator.w3.org/feed/docs/warning/SecurityRisk.html)
for security reasons),
but any html that gives the aggregator pause will usually just be skipped over.

There are a number of
[other item elements](https://www.rssboard.org/rss-specification#hrelementsOfLtitemgt)
you an add if you wish, but these are the subset I've found most useful.

You can add as many items as you like. Just repeat the `$<$item$>$` section.

```...
    $<$item$>$
      $<$title$>$First post$<$/title$>$
      ...
    $<$/item$>$ <br>
```...
    $<$item$>$
      $<$title$>$Second post$<$/title$>$
      ...
    $<$/item$>$ <br>
```...
    $<$item$>$
      $<$title$>$Third post$<$/title$>$
      ...
    $<$/item$>$ <br>
...```

And that's it. You've created an RSS feed. Every time you edit your .xml
file, you've updated your feed.

Debugging feed .xml files can be fiddly. There are a lot of details that
have to be just so, and it can take a loooong time to
wait for the aggregrator to re-scan so that you can check the results.
A cool thing I discovered while writing this is that there is
[a validation service](https://validator.w3.org/feed/) where you can
enter the URL for your feed and it will check your feed for errors
right away. It revealed several imperfections in my own feed that I 
was obvlivious to.

## RSS is different than other social media

### Things that you give up

Analytics
Dopamine
Engagement
Interaction.
Infinite scrolling
Doom scrolling
Accidental discovery
Polish and

### Things you get

Control.
Things that you give up
Analytics
Dopamine
Engagement
Interaction.
Reach.Boosting. There is no algorithm, just posts showing up in order.
Deliberate reading

Control.
Independence no From corporate constraints.
An indestructible distribution channel.
More deliberate content.
Quirk.
Rabbit holes.

It's a great starting point for creating your own RSS.


[The full RSS 2.0 specification](https://www.rssboard.org/rss-specification)

[Atom is better](https://danielmiessler.com/blog/atom-rss-why-we-should-just-call-them-feeds-instead-of-rss-feeds)
