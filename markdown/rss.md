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
on us. Thankfully there **aggregators**, helpful programs that regularly
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

But this is the key to subscribing. Copy the URL and paste it into the
field for 
"Add channel" or "Follow feeds" or whatever other name your aggregator uses.
And from then on your aggregator will revisit that URL occasionally,
checking for new content, and add it to your feed.

## Writing RSS

This is the stub example pulled from the Wikipedia page.

```<rss version="2.0">
<channel>
 <title>RSS Title</title>
 <description>This is an example of an RSS feed</description>
 <link>http://www.example.com/main.html</link>
 <copyright>2020 Example.com All rights reserved</copyright>
 <lastBuildDate>Mon, 6 Sep 2010 00:01:00 +0000</lastBuildDate>
 <pubDate>Sun, 6 Sep 2009 16:20:00 +0000</pubDate>
 <ttl>1800</ttl>

 <item>
  <title>Example entry</title>
  <description>Here is some text containing an interesting description.</description>
  <link>http://www.example.com/blog/post/1</link>
  <guid isPermaLink="false">7bd204c6-1655-4c27-aeee-53f933c5395f</guid>
  <pubDate>Sun, 6 Sep 2009 16:20:00 +0000</pubDate>
 </item>

</channel>
</rss>```

It's a great starting point for creating your own RSS.


[The full RSS 2.0 specification](https://www.rssboard.org/rss-specification)
