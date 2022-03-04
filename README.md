# discord-timers

A simple extension for discord.py which provides basic timer support.

## Installing

```bash
pip install git+https://github.com/iagolirapasssos/discord-timers
```

## Example

```python
import datetime

from discord.ext.commands import Option
from typing import Literal
from typing import Optional
from discord.ext import commands, timers


bot = commands.Bot(command_prefix="!")
bot.timer_manager = timers.TimerManager(bot)

"""
Tested example with the latest version of the library https://github.com/iDevision/enhanced-discord.py/ is slash commands"""
@bot.command()
async def remind(
    self, 
    ctx,
    text: str = Option(description="Enter your text"),
    days:Optional[int] = Option(description="Enter with the days [optional]", default=1,
    minutes:Optional[int] = Option(description="Enter with the minutes [optional]", default=0),
    steptime:Optional[int] = Option(description="Time in minutes to send messages [optional]", default=1)
    ):
		
    expires = datetime.datetime.utcnow() + datetime.timedelta(days=days, minutes=minutes)

    timers.Timer(self.bot, "reminder", expires, args=(ctx.channel.id, ctx.author.id, text, expires, steptime)).start()

    await ctx.send("Timer successfully set!")

@bot.event
async def on_reminder(self, channel_id, author_id, text, expires, steptime):
    channel = self.bot.get_channel(channel_id)

    duration = expires - datetime.datetime.now()

    hours, remainder = divmod(abs(int(duration.total_seconds())), 3600)
    minutes, seconds = divmod(remainder, 60)
    days, hours = divmod(hours, 24)

    await channel.send("Hi, <@{0}>, {1}d, {2}h, {3}min and {4}s are left for {5}!".format(author_id, days, hours, minutes, seconds, text))

bot.run("token")
```