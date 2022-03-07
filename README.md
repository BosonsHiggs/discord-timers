# discord-timers

A simple extension for discord.py which provides basic timer support.

## Installing

```bash
pip install https://github.com/BosonsHiggs/discord-timers
```

## Example

```python
import datetime, discord

from discord.ext.commands import Option
from typing import Literal
from typing import Optional
from discord.ext import commands, timers

intents=discord.Intents(guilds=True, messages=True)

class PersistentViewBot(commands.Bot):
	def __init__(self, prefix, intents, slash_commands, message_commands, slash_command_guilds):
		super().__init__(
			command_prefix=prefix, 
			intents=intents, 
			slash_commands=slash_commands,
			message_commands=message_commands,
			slash_command_guilds=slash_command_guilds
		)

	async def on_reminder(self, guild_id, channel_id, role_id, author_id, text, expires, flagevent):
		guild = bot.get_guild(guild_id)
		channel = guild.get_channel(channel_id)

		duration = abs(expires - datetime.datetime.now())

		hours, remainder = divmod(int(duration.total_seconds()), 3600)
		minutes, seconds = divmod(int(remainder), 60)
		days, hours = divmod(int(hours), 24)

		text = f"{text}".format(author_id, days, hours, minutes, seconds)
		text = text.replace(r'\n', '\n')

		await channel.send(f"{text}".format(author_id, days, hours, minutes, seconds, text))

### Register slash command globally (1 hour)
#bot = PersistentViewBot("", intents, True, False,  None)

### Test the command quickly
bot = PersistentViewBot("", intents, True, False,  [guilds_ids_here])

#bot.timer_manager = timers.TimerManager(bot)
bot.remove_command('help')



##Tested example with the latest version of the 
##library https://github.com/iDevision/enhanced-discord.py/ is slash commands

@bot.command()
async def remind(
	ctx: commands.Context,
	text: str = Option(description="Enter your text"),
	minutes: int = Option(description="Enter with the minutes"),
	):
	
	flagevent=None
	minutes = minutes if days is not None else 1

	expires = datetime.datetime.now() + datetime.timedelta(minutes=minutes)

	timers.Timer(self.bot, "reminder", expires, steptime, args=(guild_id, channel_id, text, expires, flagevent)).start()

	await ctx.send("Timer successfully set!")

bot.run("token")
```