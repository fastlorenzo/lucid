**If upgrading from previous versions**
Below are important instructions if you are upgrading lucid from a previous version. If you make a new installation you can ignore what follows.

**PLEASE MAKE SURE THAT YOU GO THROUGH ALL STEPS BELOW WHERE IT SAYS "BREAKING CHANGE", DON'T SKIP ANY VERSION**

***Version 1.4.0***
- **BREAKING CHANGE** Added [000_StartupTrigger.py](https://github.com/OH-Jython-Scripters/lucid/blob/master/automation/jsr223/000_StartupTrigger.py) to the repository needed for start up triggers to work. Added installation instructions in [README.md](https://github.com/OH-Jython-Scripters/lucid/blob/master/README.md#installing). You'll need to add that file into your existing installation.
- The lucid.utils functions `postUpdateCheckFirst` and `sendCommandCheckFirst` often failed to compare a stored float value with the new one. For example the float value of `1.3` might be stored as `1.29999995232`. Due to that most decimal fractions cannot be represented exactly as binary fractions comparison was impossible. This update adds an additional and optional keyword argument to the functions `postUpdateCheckFirst` and `sendCommandCheckFirst` named `floatPrecision`. If you supply it, rounding will occurr on the stored value to the number of decimals you specify. Example: `postUpdateCheckFirst('V_RainFallHour' 1.3, floatPrecision=1)` would not post an update to the item if the currently stored value is `1.29999995232`

***Version 1.3.0***
- Bug fixed (again). Channel event triggers do not have item name.
- `event.isChannel` added.

***Version 1.2.2***
- Bug fixed. Channel event triggers do not have item name.

***Version 1.2.1***
- Bug fixed. self.event.state was of string type for switches and contacts.

***Version 1.2.0***
- **BREAKING CHANGE** Predefined cron expression `EVERY_DAY_AT_NOON` has been renamed to `EVERY_DAY_AROUND_NOON`.
- Predefined cron expressions `EVERY_MINUTE_A`, `EVERY_MINUTE_B`, `EVERY_HOUR_A` and `EVERY_HOUR_B` have been removed since they are not needed.
- Added predefined cron expressions `EVERY_5_MINUTES` and `EVERY_15_MINUTES`.

***Version 1.1.0***
- **BREAKING CHANGE** New feature. To avoid the "top of the minute problem" we space out our cronjobs (that are using predefined cron expressions) not to occur exactly on the minute or at the top of the hour. That will help your own server as well as shared resources that you'll be using to spread out the work more evenly. Each of the predefined cron expressions will be randomized at openHAB reload. If you want to run a cron expression hourly at minute 0 and second 0 you should not use the predfined cron expressions. Read [more](https://github.com/OH-Jython-Scripters/lucid/blob/master/README.md#time-based-cron-triggers).

***Version 1.0.0***
- **BREAKING CHANGE** New mandatory configuration items (`customItemNames` and `customGroupNames`.) must be manually entered into your lucid configuration file. Please compare your lucid configuration file with the provided [example configuration file](https://github.com/OH-Jython-Scripters/lucid/blob/master/automation/lib/python/lucid/example_config.py). Mandatory configuration items are marked.

- **BREAKING CHANGE** There is now a `self.event` object available in your lucid rules which should be used to access the event object. Therefore the `getEvent` function is removed from lucid.utils. If you have any custom lucid scripts, remove import statements for `getEvent` from lucid.utils e.g. `from lucid.utils import getEvent`.

- The openHAB "[python constants](https://stackoverflow.com/questions/17291791/why-no-const-in-python)" `DEBUG`, `INFO`, `WARNING` `ERROR`, `NULL`, `UNDEF`, `ON`, `OFF`, `OPEN`, and `CLOSED` are now provided as local variables in each lucid rule's `execute` function. If you have any custom lucid scripts, remove the import statements e.g. `from logging import DEBUG, INFO, WARNING, ERROR` For example to set the logging to `DEBUG`, use: `self.log.setLevel(DEBUG)` without any quotes surrounding `DEBUG`.

  NOTE! The "[python constants](https://stackoverflow.com/questions/17291791/why-no-const-in-python)" `NULL`, `ON`, `OFF`, `OPEN` and `CLOSED` are of the openHAB native type, e.g. they aren't string type. You can't directly compare a string value with them. Also, note that the events.postUpdate function expects a string as the second argument. Make sure you use lucids `postUpdate`, `postUpdateCheckFirst`, `sendCommand` or `sendCommandCheckFirst` functions in all your lucid scipts and **don't** convert the value argument to string type.

  Where you previously had to use statements like `if mySwitch.state.toString() == "ON"` you now simply use `if mySwitch.state == ON`.

- New functions in lucid.utils: `postUpdate` and `sendCommand` which are wrappers for `events.postUpdate` and `events.sendCommand`. The new functions should typically be used when you want to send an update or post a command to an item regardless if the new value is the same as the current or not. Note that unless the item you intend to update or send a command to is of the openHAB item `String type` you should **NOT** convert the new value to a python string when passing it as an argument. (As opposite yo when using the native built in function counterparts.)

- The "[python constants](https://stackoverflow.com/questions/17291791/why-no-const-in-python)" `ON`, `OFF`, `OPEN`, and `CLOSED` are now provided as local variables in each lucid rule's `getEventTriggers` function. Please note that within the scope of this function they are all string type because that that's the expected type when passed as arguments to the triggers.

- A few cron trigger expression "[constants](https://stackoverflow.com/questions/17291791/why-no-const-in-python)" are now provided as local variables in each lucid rule's `getEventTriggers` function. Available expressions are: `EVERY_10_SECONDS`, `EVERY_15_SECONDS`, `EVERY_30_SECONDS`, `EVERY_MINUTE`, `EVERY_MINUTE_A`, `EVERY_MINUTE_B`, `EVERY_OTHER_MINUTE`, `EVERY_10_MINUTES`, `EVERY_30_MINUTES`, `EVERY_HOUR`, `EVERY_HOUR_A`, `EVERY_HOUR_B`, `EVERY_6_HOURS`, `EVERY_DAY_AROUND_NOON`. The expressions were generated by [CronMaker](http://www.cronmaker.com/) **PLEASE NOTE** that when using these constants, the cron jobs are "spaced out" to avoid the "top of the minute problem". You are advised to read more about that in the [documentation](https://github.com/OH-Jython-Scripters/lucid/blob/master/README.md#time-based-cron-triggers).

- **BREAKING CHANGE** configuration item `timeofday` has changed name to `timeOfDay`.

- **BREAKING CHANGE** If you are using [ideAlarm](https://github.com/OH-Jython-Scripters/ideAlarm) or [weatherStationUploader](https://github.com/OH-Jython-Scripters/weatherStationUploader), they also need to be upgraded to their latest version.

***Version 0.9.0***
- Initial version.
