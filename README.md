# Botometer Python API

A Python API for [Botometer by OSoMe](https://osome.iuni.iu.edu).
Previously known as `botornot-python`.

Behind the scenes, this uses the Botometer's HTTP endpoint, available via
[RapidAPI](https://rapidapi.com/OSoMe/api/botometer-pro).

RapidAPI usage/account related questions should be posted on RapidAPI discussion.

## [Change Note]
### May, 2020

We have made some changes to our API, please read the [annoucnment](https://twitter.com/Botometer/status/1250557098708144131) for details. Due to the API change, the old `botometer-python` package might stop to work and produce 404 errors. Please upgrade it in your local environment to the least version.

### Sep, 2019

Mashape has renamed itself to [RapidAPI](https://rapidapi.com/).
The old mashape.com based URL and HTTP headers were deprecated in Sep 1st, 2019.
So please upgrade `botometer-python` package in your local environment to the least version for the change.

## Help
> You probably want to have a look at [Troubleshooting & FAQ](https://github.com/IUNetSci/botometer-python/wiki/Troubleshooting-&-FAQ) in the wiki. Please feel free to suggest and/or contribute improvements to that page.

## Prior to Utilizing Botometer
To begin using Botometer, you must follow the steps below before running any code:
1. Create a free [RapidAPI](https://rapidapi.com/) account.
2. Subscribe to [Botometer Pro](https://rapidapi.com/OSoMe/api/botometer-pro) on RapidApi.
    > There is a completely free version (which does not require any credit card information) for testing purposes.
3. Create a Twitter application via https://developer.twitter.com/
    > Botometer utilizes the access credentials provided by Twitter for the application.
4. Ensure Botometer Pro's dependencies are already installed. 
    > See the [Dependencies](#dependencies) section for details.

**Note:** These steps are necessary to access credentials and download other packages which are needed for Botometer to work properly. Please see [RapidAPI and Twitter Access Details](#access) below for more details on this topic.

## Quickstart
From your command shell, run 

```
pip install botometer
```

then in a Python shell or script, enter something like this:
```python
import botometer

rapidapi_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" # now it's called rapidapi key
twitter_app_auth = {
    'consumer_key': 'xxxxxxxx',
    'consumer_secret': 'xxxxxxxxxx',
    'access_token': 'xxxxxxxxx',
    'access_token_secret': 'xxxxxxxxxxx',
  }
bom = botometer.Botometer(wait_on_ratelimit=True,
                          rapidapi_key=rapidapi_key,
                          **twitter_app_auth)

# Check a single account by screen name
result = bom.check_account('@clayadavis')

# Check a single account by id
result = bom.check_account(1548959833)

# Check a sequence of accounts
accounts = ['@clayadavis', '@onurvarol', '@jabawack']
for screen_name, result in bom.check_accounts_in(accounts):
    # Do stuff with `screen_name` and `result`
```

Result:
```json
{
  "cap": {
    "english": 0.0011785984309163565,
    "universal": 0.0016912294273666159
  },
  "categories": {
    "content": 0.058082395351262375,
    "friend": 0.044435259626385865,
    "network": 0.07064549990637549,
    "sentiment": 0.07214003430676995,
    "temporal": 0.07924665710801207,
    "user": 0.027817972609638725
  },
  "display_scores": {
    "content": 0.3,
    "english": 0.1,
    "friend": 0.2,
    "network": 0.4,
    "sentiment": 0.4,
    "temporal": 0.4,
    "universal": 0.1,
    "user": 0.1
  },
  "scores": {
    "english": 0.0215615093045025,
    "universal": 0.0254864249403189
  },
  "user": {
    "id_str": "1548959833",
    "screen_name": "clayadavis",
    "...": "..."
  }
}
```

For more information on this response object, consult the [API Overview](https://rapidapi.com/OSoMe/api/botometer-pro/details) on RapidAPI.

## Install instructions
This package is on PyPI so you can install it with pip:

```
$ pip install botometer
```

<a id="dependencies"></a>
## Dependencies

### Python dependencies
* [requests](http://docs.python-requests.org/en/latest/)
* [tweepy](https://github.com/tweepy/tweepy)

Both of these dependencies are available via `pip`, so you can install both at once with

    pip install requests tweepy

<a id="access"></a>
## RapidAPI and Twitter Access Details

### RapidAPI key

Our API is served via [RapidAPI](//rapidapi.com). You must sign up
for a free account in order to obtain a RapidAPI secret key. The easiest way to
get your secret key is to visit
[our API endpoint page](https://rapidapi.com/OSoMe/api/botometer-pro/endpoints)
and look in the endpoint's header parametsrs for the "X-RapidAPI-Key" as shown below:

![Screenshot of RapidAPI header parameters](/docs/rapidapi_key.png)
    
### Twitter app
In order to access Twitter's API, one needs to have/create a [Twitter app](https://apps.twitter.com/).
Once you've created an app, the authentication info can be found in the "Keys and Access Tokens" tab of the app's properties:
![Screenshot of app "Keys and Access Tokens"](/docs/twitter_app_keys.png)

## Authentication
By default, Botometer uses **user authentication** when interacting with Twitter's API as it is the least restrictive and the ratelimit matches with Botometer's **Pro** plan: 180 requests per 15-minute window.
One can instead use Twitter's **application authentication** in order to take advantage of the higher ratelimit that matches our **Ultra** plan: 450 requests per 15-minute window. Do note the differences between user and app-only authentication found under the header "Twitter API Authentication Model" in [Twitter's docs on authentication](https://developer.twitter.com/en/docs/basics/authentication/overview/oauth).

To use app-only auth, just omit the `access_token` and `access_token_secret` in the `Botometer` constructor.

```python
import botometer

rapidapi_key = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" # now it's called rapidapi key
twitter_app_auth = {
    'consumer_key': 'xxxxxxxx',
    'consumer_secret': 'xxxxxxxxxx'
    }
bom = botometer.Botometer(wait_on_ratelimit=True,
                          rapidapi_key=rapidapi_key,
                          **twitter_app_auth)
```

## References

- ***Botometer v4:*** Mohsen Sayyadiharikandeh, Onur Varol, Kai-Cheng Yang, Alessandro Flammini, Filippo Menczer. "Detection of Novel Social Bots by Ensembles of Specialized Classifiers." [ArXiv](https://arxiv.org/abs/2006.06867)

- ***BotometerLite:*** Yang, K.; Varol, O.; Hui, P.; and Menczer, F. "Scalable and Generalizable Social Bot Detection through Data Selection." AAAI (2020). [DOI](http://doi.org/10.1609/aaai.v34i01.5460), [ArXiv](https://arxiv.org/abs/1911.09179)

- ***Botometer v3:*** Yang, Kai‐Cheng, Onur Varol, Clayton A. Davis, Emilio Ferrara, Alessandro Flammini, and Filippo Menczer. "Arming the public with artificial intelligence to counter social bots." Human Behavior and Emerging Technologies 1, no. 1 (2019): 48-61. [DOI](https://onlinelibrary.wiley.com/doi/full/10.1002/hbe2.115), [ArXiv](https://arxiv.org/abs/1901.00912)

- ***Botometer v2:*** Varol, Onur, Emilio Ferrara, Clayton A. Davis, Filippo Menczer, and Alessandro Flammini. "Online Human-Bot Interactions: Detection, Estimation, and Characterization." ICWSM (2017). [AAAI](https://aaai.org/ocs/index.php/ICWSM/ICWSM17/paper/view/15587), [ArXiv](https://arxiv.org/abs/1703.03107)

- ***Botometer v1 aka BotOrNot:*** Davis, C. A., Varol, O., Ferrara, E., Flammini, A., & Menczer, F. (2016, April). "BotOrNot: A system to evaluate social bots". In Proceedings of the 25th International Conference Companion on World Wide Web (pp. 273-274). International World Wide Web Conferences Steering Committee. [DOI](https://doi.org/10.1145/2872518.2889302), [ArXiv](https://arxiv.org/abs/1602.00975)

- Varol O., Davis C., Menczer, F., Flammini, A. "Feature Engineering for Social Bot Detection", Feature Engineering for Machine Learning and Data Analytics [Google Books](https://books.google.com/books?id=661SDwAAQBAJ&lpg=PA311&dq=info%3AsM983rg_yb8J%3Ascholar.google.com&lr&pg=PA311#v=onepage&q&f=false)

- Ferrara, Emilio, Onur Varol, Clayton Davis, Filippo Menczer, and Alessandro Flammini. "The rise of social bots." Communications of the ACM 59, no. 7 (2016): 96-104. [DOI](https://doi.org/10.1145/2818717), [ArXiv](https://arxiv.org/abs/1407.5225)


```python

```
