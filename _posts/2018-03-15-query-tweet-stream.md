---
layout: post
title: "Capturing Twitter's public tweet stream with Python and RethinkDB"
tagline: ""
description: "Twitter API can be used to harvest public tweets in real-time"
category: articles
tags: [python, jupyter_notebook, tweet_stream, api]
---


You can easily collect public tweets from Twitter.com using its api. In this post I demonstrate how to use [Tweetpy](https://github.com/tweepy/tweepy) Python package to connect to Twitter api and call its API to capture tweets. We will store the captured real-time tweets for term **\#btc** inside a NoSQL data-store named RethinkDB to show how everything is going to be done. RethinkDB is a NoSQL Data-store used mainly to store and retrieve real-time JSON data streams, for further info please visit [RethinkDB website](https://www.rethinkdb.com).

### Twitter API
In order to use twitter api we need a twitter account(obvoiusly) and you also need to create a twitter app then add an access token for that app to access twitter api via your account. Plese see the [twitter developer docs](https://developer.twitter.com/en/docs/basics/authentication/overview) for the details.

### RethinkDB:
Please visit [RethinkDB](https://www.rethinkdb.com) for more instruction on how to install RethinkDB on your machine.


#### Loading Python modules


```python
import io
import json
import pandas
import numpy
import rethinkdb

# need this for using pandas built-in plotting facility
import matplotlib.pyplot as plt
%matplotlib inline

pandas.set_option('display.max_rows', 10)
pandas.set_option('display.max_columns', 10)
```


```python
from tweepy.streaming import StreamListener
from tweepy import OAuthHandler
from tweepy import Stream
```

#### Twitter API's keys
Go to [http://apps.twitter.com](http://apps.twitter.com) and create a Twitter App for your Twitter account.


```python
# Go to http://apps.twitter.com and create an app.
# The consumer key and secret will be generated for you after
consumer_key="CONSUMER KEY"
consumer_secret="CONSUMER SECRET"

# After the step above, you will be redirected to your app's page.
# Create an access token under the the "Your access token" section
access_token="ACCESS TOKEN"
access_token_secret="ACCESS TOKEN SECRET"
```

#### Creating a tweet stream listener class

1. ***to_stdout_listener***: Prints the tweets to the **stdout**
1. ***to_file_listener***: Write out the tweets to a **directory on the filesystem**
1. ***to_rethinkdb_listener***: Store the tweets to a **RethinkDB instance**

Tweets are in **JSON** format.


```python
class BaseStreamListener(StreamListener):
    """This is the base class for tweet stream listener."""
    def on_error(self, status):
        print(status)
    
class to_stdout_listener(BaseStreamListener):
    """ A listener handles tweets that are received from the stream.
    This is a basic listener that just prints received tweets to stdout.
    """
    def on_data(self, data):
        print(data)
        return True

class to_file_listener(BaseStreamListener):
    """ A listener handles tweets that are received from the stream.
    This is a basic listener that saves the received tweets inside a directory.
    """
    save_dir = "./datasets/tweets/"
    
    def _dump(self, data):
        data = json.loads(data)
        file_name = data['id_str']+".json"
        with io.open(self.save_dir+file_name, "w") as tweet_file:
            json.dump(data, tweet_file)
        print("written tweet data %s to file-system" % file_name)
            
    def on_data(self, data):
        try:
            self._dump(data)
        except:
            pass
        return True
    
class to_rethinkdb_listener(BaseStreamListener):
    """ A listener handles tweets that are received from the stream.
    This is a basic listener that saves the received tweets to a rethinkdb instance.
    """
    host = "localhost"
    port = 28015
    db = "tweet_stream"
    table = "btc"
    create_db = False
    
    def make_connexion(self):
        # don't forget to start rethinkdb 'cd && rethinkdb --bind all' first!
        self.connexion = rethinkdb.connect(self.host, self.port)
    
    def close_connexion(self):
        self.connexion.close()
    
    def on_data(self, data):
        if self.create_db:
            try:
                rethinkdb.db_drop(self.db).run(self.connexion)
            except Exception as err:
                print(error)
            rethinkdb.db_create(self.db).run(self.connexion)
            rethinkdb.db(self.db).table_create(self.table).run(self.connexion)
        try:
            tweet_data = json.loads(data)
            rethinkdb.db(self.db).table(self.table).insert(tweet_data).run(self.connexion)
        except Exception as err:
            print(error)
        else:
            print("written tweet data %s to rethinkdb" % tweet_data['id_str'])
        return True
    
def make_stream_pipe(consumer_key, consumer_secret,
                    access_token, access_token_secret, listener):
    """Make a file-stream like object to read the tweets from."""
    auth = OAuthHandler(consumer_key, consumer_secret)
    auth.set_access_token(access_token, access_token_secret)
    return Stream(auth, listener)
```

#### Starting to listen to the public tweets for 'btc' and printing them to the stdout


```python
# listen to the tweet stream ... and print out the tweets to **stdout**
listener = to_stdout_listener()
stream = make_stream_pipe(consumer_key, consumer_secret,
                    access_token, access_token_secret, listener)

stream.filter(track=['btc'])
```

#### Starting to listen to the public tweets for 'btc' and writing them them to a path on the filesystem


```python
# listen to the tweet stream ... and write out the tweets to the path **'./datasets/tweets/'**
listener = to_file_listener()
stream = make_stream_pipe(consumer_key, consumer_secret,
                    access_token, access_token_secret, listener)

stream.filter(track=['btc'])
```

#### Starting to listen to the public tweets for 'btc' and storing them to the **localhost['tweet_stream']['btc']**
**localhost['tweet_stream']['btc']** is a RethinkDB server listening on **localhost** that contain a database named **tweet_stream** which in turn has a table called **btc** in it.


```python
# listen to the tweet stream ... and push the tweets into a rethinkdb table
listener = to_rethinkdb_listener()
listener.create_db = False
listener.make_connexion()

stream = make_stream_pipe(consumer_key, consumer_secret,
                    access_token, access_token_secret, listener)

# open up 'localhost:8080' on the machine where rethinkdb is running to inspect the data
# sample query 'r.db("tweet_stream").table("btc")'
stream.filter(track=['btc'])
```

#### You can detach from the tweet stream via calling 'disconnect' method from a stream listener object

```python
# close the stream
stream.disconnect()
```

#### You also can use this snippet to close a to_rethinkdb_listener object

```python
if isinstance(listener, to_rethinkdb_listener):
    listener.close_connexion()
```
