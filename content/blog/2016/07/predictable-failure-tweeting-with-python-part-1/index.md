---
title: "Predictable failure - Tweeting with Python Part 1"
date: "2016-07-11"
series: ["Tweeting with Python"]
series_order: 1
categories: 
  - "tech"
tags: 
  - "bitcoin"
  - "cryptocurrency"
  - "debian"
  - "iot"
  - "linux"
  - "python"
  - "raspberry-pi"
  - "raspbian"
  - "technology"
  - "twitter"
  - "twython"
coverImage: "tweeting_cover.jpg"
---

As explained in a [previous series of mine]({{< ref "/tags/bitcoin" >}}), I run a small Bitcoin mining operation on my Raspberry Pi. Once configured, it generally just sits in a corner (behind my TV unit now) out of sight, out of mind and continues to mine away on the USB ASIC hardware. Unfortunately from time to time it does hang and with it, the entire mining operation ceases. If I'm not persistent in keeping track of it _(hint: I'm not)_  then it may be down for a considerable amount of time.

Rather than let such waste occur, I decided to solve my problems with code!

I knew it wouldn't be all that difficult to prevent the kind of hang that occurred in Linux by scheduling a reboot but the real key would be to introduce some notifications so that I would be aware a reboot occurred AND it was successful.

I could introduce a monitoring system but the only device that was powered on 24/7 in my home is my router and my Pi. So that isn't really feasible. I did know however, through some previous experiences that I could send tweets from my Raspberry Pi via the Python library Twython (no points for originality there guys).

## Sign up to Twitter and get API access

Head on over to [http://www.twitter.com](http://www.twitter.com) and sign up for a 2nd Twitter account. We will use this account to send the tweets to your regular account. In this example however, I'll use my current account.

Once you have your second account, go to [https://dev.twitter.com/oauth/overview/application-owner-access-tokens](https://dev.twitter.com/oauth/overview/application-owner-access-tokens). Rather and logging in with a username and password to send the tweets, we will interface with Twitter via their APIs instead. We will utilise the OAuth system of tokens to authenticate to Twitter and thus protecting your credentials (even if it is a dummy account, tokens can be managed more granularly than passwords).

## Installing Twython

Presuming you are running Python 2 versions 2.7.9 or higher or Python 3 versions 3.4 or higher, you also have a handy Python package manager of sorts called Pip. So lets use this tool to install Twython:

```bash
pip install twython
```

Painless right?

### Python script to tweet shutdown

Next step will be to create our Python shutdown script that will tweet out the Raspberry Pi is shutting down. Let's navigate to our home directory and use Nano (or your favourite Linux editor) to create the file:

```bash
cd ~
nano TweetShutdown.py
```

With our new Python script file open, enter the following

```python
from twython import Twython
import os
import time
import datetime
 
# Send date and timestamp to string, tweet to string then combine
dts = datetime.datetime.fromtimestamp(time.time()).strftime('%y%m%d%H%M%S')
 
tweet = "@DXPetti I'm going down #GALMPI "
 
tweetstr = tweet + dts
 
# Twitter application authentication
APP_KEY = 'replacewithyours'
APP_SECRET = 'replacewithyours'
OAUTH_TOKEN = 'replacewithyours'
OAUTH_TOKEN_SECRET = 'replacewithyours'
 
api = Twython(APP_KEY, APP_SECRET, OAUTH_TOKEN, OAUTH_TOKEN_SECRET)
 
api.update_status(status=tweetstr)
```

Now there is a lot happening in the above so lets tear it apart and break it down.

```python
from twython import Twython
import os
import time
import datetime
```

Like any good script (or program), the first thing we do is import the libraries we need to do the job. In this example, we are importing the Twython libraries from the twython wrapper followed by time and datetime libraries from Python itself (notice there is no _from_ prefacing these lines?). These libraries will drive the functions used later on in the script.

```python
# Send date and timestamp to string, tweet to string then combine
dts = datetime.datetime.fromtimestamp(time.time()).strftime('%y%m%d%H%M%S')
 
tweet = "@DXPetti I'm going down #GALMPI "
 
tweetstr = tweet + dts
```

The comment line spells this out pretty clearly but here we are creating 3 strings; first is utilising the **time** and **datetime** libraries to get a date and timestamp so that each tweet will be unique. The logic behind this is that Twitter filters out tweets that contain the same content from any one account. Considering that these tweets will go out on a regular basis and the message itself will not change; to avoid getting spam flagged we will add the date and timestamp to final tweet. Second string is the shutdown message. Nice and simple, targeted to my Twitter account (so I get a notification _ala_ alert) and added a hashtag of the system name. Last but not least, we combine the two previous strings to form the final tweet to send out.

```python
# Twitter application authentication
APP_KEY = 'replacewithyours'
APP_SECRET = 'replacewithyours'
OAUTH_TOKEN = 'replacewithyours'
OAUTH_TOKEN_SECRET = 'replacewithyours'
 
api = Twython(APP_KEY, APP_SECRET, OAUTH_TOKEN, OAUTH_TOKEN_SECRET)
```

Next up, we need to tell the script all the key authentication details generated earlier to have the permission to send a tweet.  Plug your details from the dev.twitter.com dashboard into the corresponding section above and the final line passes those to the API and authenticates you to Twitter.

```python
api.update_status(status=tweetstr)
```

We have what we want to tweet and we are authenticated against the API, lets send that tweet. Remember the string **tweetstr** we built early by combining our message + date and timestamp? Now we are sending that string in full as a Twitter status update or tweet.

[Part 2]({{< ref "blog/2016/08/predictable-failure-tweeting-with-python-part-2" >}}) will see us execute our script as well as create more for restarting and startup confirmation.
