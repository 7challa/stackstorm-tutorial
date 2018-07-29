# Packs

Packs, short for package, are organization units that bundle together the various
extension points that StackStorm provides. In a pack you can distribute the following:

* actions
* workflows
* rules
* sensors
* triggers
* Python `requirements.txt` files
* Additional content (think Jinja templates, Ansible playbooks, etc)

Packs are just `git` repos! You can either install them with the URL to the `git` repo:

```shell
st2 pack install https://domain.tld/git/stackstorm-mycoolpack.git
```

Or, you can install a pack from the public [StackStorm exchange](https://exchange.stackstorm.org)
by name:

```shell
st2 pack install xxx
```

In this demo we're going to install the `twitter` pack and configure it so
we can post a tweet from StackStorm.

## Install twitter pack from exchange

Frirst, install the `twitter` pack from the public
[StackStorm exchange](https://exchange.stackstorm.org).

``` shell
st2 pack install twitter
```

### Setup a Twitter app)

Visit https://apps.twitter.com and create a new App:

``` shell
name = <yuour name here> StackStorm Tutorial
description = StackStorm tutorial application
website = https://stackstorm.org
```

Agree and click submit!

![twitter_01_create_app.png](/img/twitter_01_create_app.png)

### Create an access Token

* Within your new application click `Keys and Access Tokens`.
* On the top of the page this will contain your `consumer_key` and `consumer_secret`
* At the bottom of the page, generate a new `Access Token`, thisll will be your `access_token` and `access_token_secret`

![twitter_02_create_access_token.png](/img/twitter_02_create_access_token.png)

![twitter_03_obtain_access_token.png](/img/twitter_03_obtain_access_token.png)

### Create the Twitter config in StackStorm

``` shell
sudo cp /opt/stackstorm/packs/twitter/twitter.yaml.example /opt/stackstorm/configs/twitter.yaml
```

Edit `/opt/stackstorm/configs/twitter.yaml` and fill in the value from the `Keys and Access Tokens` page. Example (you'll need to edit this with `sudo`:

```shell
sudo vi /opt/stackstorm/configs/twitter.yaml
```

Content: 

``` yaml
---
consumer_key: "9JsHlvxpkGMuWnMd1fgbJfS4W"
consumer_secret: "sH5QHYu9Wjng3KzdB2nH5dGNSte3UdP2MU3sliaLlF4i9n9wyw"

access_token: "1010576900513849344-1nQd5oZXz6zivJXHFFzsy5loBSWfmq"
access_token_secret: "41viAMyPZpYTRk38AWhNgAzqAK2V8FwrIwrbbFyhGAosk"

query:
  - "#PyOhio"
  - "@Stack_Storm"
count: 30
language: en
```

-----------
**NOTE** 
If you're struggling and just need the answer, simply copy the file from our
answers directory:
```shell
sudo cp /opt/stackstorm/packs/tutorial/etc/answers/configs/twitter.yaml /opt/stackstorm/configs/twitter.yaml
```
-----------


Now, we'll tell StackStorm to load this configuration into its database:

``` shell
st2ctl reload --register-configs
```

### Test out the Twitter pack

```shell
st2 run twitter.update_status status="Test from StackStorm CLI <your name here>"
```

Check out my demo twitter page to see if your tweet is present:

https://twitter.com/NickMaludyDemo
