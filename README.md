This code will allow you to run a Discord bot that is able to query the Steem Global Blacklist API created by witness @themarkymark.

https://steemit.com/utopian-io/@themarkymark/global-blacklist-api-2-0-released

You may clone the repo as follows. (Probably the easiest method to run.)

```
https://github.com/anthonyadavisii/blacklist_check.git
```

This code has been uploaded to PyPi and may be installed as follows:

```
python3.6 -m install Global-Blacklist-API-Check-Bot
```

OR (if you're feeling brave, try PIP and let me know if it works!)

```
pip install Global-Blacklist-API-Check-Bot
```

In order to use the code, you need to set your bot token as environmental variable using ("export TOKEN=<token>") prior to running blacklist_check.py.

If you would like to add the function to pre-existing bot, you should be able to use the following:

```@bot.command()
async def blacklist_check(ctx,user):
    """Queries blacklist status of user using @themarkymark's Blacklist API."""
    contents = urllib.request.urlopen("http://blacklist.usesteem.com/user/"+user).read()
    bl = (str(contents).split('"blacklisted":[')[1]).split(']')[0]
    if bl == '':
        await ctx.send('@'+user+' is not on any blacklist tracked via the API')
        return
    else:
        blacklists = []
        bl = bl.split(',')
        for b in bl:
            blacklists.append(b.replace('"',''))
        await ctx.send('@'+user+' is on the following blacklists: ')
        for b in blacklists:
            await ctx.send(b)
```

This above is what we have currently on the @steemflagrewards bot but it did not seem to work in my development environment so what removed the context dependencies for purposes of this package. (ie ctx.send replaced with bot.say)

Once bot is running, you may test by issuing "?blacklist_check <user>" without the '@' sign. The result should list our the blacklists the user has been added to if applicable.
