---
title: 'Release Notes'
taxonomy:
    category:
        - docs
---
[github-releases]:https://github.com/MycroftAI/mycroft-core/releases
[View the releases on GitHub here.][github-releases]


# v0.8.8
### New Features
* Added "expect_response" flag when speaking.  This is a step towards dialog interaction with no wake-up word will have to be spoken. PR #576
* Audio from text to speech (TTS) is now cached, avoid unnecessary calls to Mimic.  PR #599
* Wolfram Alpha skill now only attempts to handle missed intents that look like questions (Who/what/when/etc). PR #601

### Internationalization Support
* Implemented skill language testing so only skills that support current language will be loaded. PR #622
* Added localization support for the Sleep/Wake Up mechanism. PR #575

### Build process
* Updated build instructions for Arch and Fedora. PR #584
* Updated to use Mimic 1.2.0.2. PR #582
* Fixed debian build problem. PR #617
* Missing dependency was causing compile errors on Fedora. PR #573
* Mimic updates wouldn't overwrite existing installation in install-mimic.sh. PR #580

### Bug fixes
* Fixed volume increase/decrease, broken by Skill:Intent namespacing. PR #619
* Commands issued using subprocess.Popen() were remaining open in background.  PR #572
* Excessive CPU usage due to testing for wake-up word on every chuck received. PR #578
* Wake-up word was requiring double silence duration at end. PR #586
* The skill installer skill wasn't catching exit code properly. PR #631


# v0.8.7
### CLI enhancements PR #548 
* Live microphone meter.  This makes it easier to see if your microphone is hearing anything. 
* Long log line left/right scrolling
* Eliminated flicker on Raspberry Pi
### Plumbing changes
* Created general Inter Process Communication (IPC) mechanism.
* Enhanced Signal mechanism to use the IPC and allow signals with durations
* Utterance normalization.  This simplifies intent parsing by converting number words to digits, expanding contractions, removing articles, etc.  PR #531 
NOTE:  This is potentially a breaking change for custom skills.  Please verify behavior.
* Refined skill auto-reload with a self.reload_skill property, providing control over the auto-reload of skills when files in the skill's change.  It is on by default.  Auto-reloatd also honors blacklists.  PR #541 and PR #549 
* BUGFIX: The Debian build script was missing a dependency.  Thanks SoloVeniaASaludar!  PR #569


# v0.8.6
### Enhancements for the CLI interface. Includes:
* Interaction history
* Live, filtered log view
* Log color-coding
PR #536 

### Reduce max recording time
Max recording time is now 10 seconds instead of 30. This deals with cases where a noisy background prevents the listener's silence detection from triggering. 30 seconds was WAY too long to keep listening -- nobody is going to be saying something that long for now.
PR #529 

### Updated Mimic version to 1.2.0.1
* Upgraded to a new build process
* Compiles with GCC 6.2.0
* added --enable-shared flag for future Pymimic support
You will need to run `dev_setup.sh` or `./scritps/install-mimic.sh` to upgrade mimic
PR #525 

### Skill auto reload 
Mycroft now checks periodically for file system changes in the skills folders. This allows for them to automatically be reloaded without restarting any services.
PR #524 

### Add `-d` option to `mycroft.sh`
Enhances the `mycroft.sh` script by adding the `-d` option to start and enter the CLI client directly.
PR #522 

# v0.8.5
_Minor bugfix for v0.8.4_

# v0.8.4

### For Picroft:
* A configuration setting was added for playing wav files. We found that specifying a hardware device for aplay will fix a Raspberry Pi specific ALSA/PulseAudio issue. This can also be used to interact with other audio systems. The new setting are "play_wav_cmdline" and "play_mp3_cmdline", which can be overridden in your /etc/mycroft/mycroft.conf file.

### For Everyone:
* Added more verbose skill service logging, such as where skills are loaded from and what intents are registered.
* A wake word detected sound has been added for better interaction. This wav file is configurable as well for your own custom sound!
* When pairing your device it could be difficult to understand the spoken code. To help with this, we added a NATO-inspired phonetic spelling, such as "C as in Charlie"
* Load data files for skills automatically when loading the skill. Skills no longer need to call self.load_data_files(dirname(file))
* The Adapt intent parser version used has been upgraded to 0.3.0

# v0.8.3
### Skill refinements
#### For the Weather Skill:
* When talking about the current city, the city name is generally not spoken (more natural)
* A "pretty" name of just the city is used instead of the complete name
* Works around the recurring issue with OWM where they report bad min/max temps (same as the current temp)
* Changed "Location is not valid" to "I don't know that location" (people don't say "not valid")

#### For the Time Skill:
* The timezone is extracted from the device location setting
* Time responses are more varied and shorter
* This change adds MycroftSkill.location_pretty and MycroftSkill.location_timezone properties. PR: #492

### Cli improvements:
* "Input:" doesn't get intermingled with the output (usually -- long pauses can still cause it to happen)
* "Output:" is now displayed, not just spoken
* Ctrl+C is handled gracefully
PR: #494

# v0.8.2
This release enables our new and enhanced location configuration services at https://home.mycroft.ai.  Users can set default locations for their account and for individual devices.  This provides rich location information for Skills running on the Mycroft device, including governmental entity names (city, state, providence, country), GPS coordinates and timezone information.

# General enhancements
* Location integration (#487)
* BUGFIX:  Authorization token was not updated after expiration (#452)
* BUGFIX:  Speech To Text config initialization (#484)
* BUGFIX:  Shutdown all skills on process termination (#479)

# Developer tools
* BUGFIX:  dev_setup.sh ignored WORKON_HOME when VIRTUALENV_ROOT was unset (#372)

# v0.8.1
### General enhancements
- Changed network timeout to improve disconnected behavior (#461).  This is most important when first bringing up a unit which has no network connection configured yet.
- Updating 'requests' package version in requirements.txt (#464).  This fixes sporadic SSL3 errors that were appearing in the logs with long STT requests on some platforms.
- Pairing now says "zero" instead of "oh" (#401)
- NEW: Skills will load from one-level-deep subfolder.  This makes it simpler to use git to install a group of skills if the author keeps several in one repo.  For example, it will now find "/opt/mycroft/skills/bobs_skills/calendar/__init__.py" as well as "/opt/mycroft/skills/bobs_calendar/__init__.py" 

### Picroft support
- Stop splitting speech at period when on Picroft (#466).  The lockup that can happen with analog audio output (an issue with Raspbian/Pi hardware) happen much less frequently.

### Developer tools
- Fixed unit tests, added "cleanup" to prevent blocking (#453)
- Added skill_container to packaged distribution (#450)
- Bringing Google TTS support up to speed (#454)
- Update build_host_setup_arch.sh for Arch Linux (#459)

# v0.8.0
### New API and web service integration
- Connenction to the new account management back-end. We have moved to [home.mycroft.ai](https://home.mycroft.ai) from [cerberus.mycroft.ai](https://cerberus.mycroft.ai). Older code is incompatible with the new API so we will continue to maintain Cerberus for some time. Not sure where to pair? Let Mycroft tell you the correct website.
- Many more configuration options, including support for multiple social media login types as well as stand-alone accounts.
- Change TTS engine, wake-word, units, time and date format, and more settings from the web interface.
- We lost support for loading location and timezone configuration from the web but this is coming again soon. These settings can still be found in the mycroft.conf file; [Take a look here.](https://github.com/MycroftAI/mycroft-core/blob/master/mycroft/configuration/mycroft.conf#L3)

### Core enhancements
- Major configuration format refactor. The flat mycroft.ini file has now been replaced by a json implementation in mycroft.conf. This should allow for simpler usage and will be much more powerful.
- The identity.json has been renamed to identity2.json, to avoid conflicts with the Cerberus account management system.
- Configuration loading changes: home.mycroft.ai will periodically be polled poll for changes and no longer requires a complete restart for settings changes to take effect.
- Wolfram Alpha and Open Weather Map now have a isolated API layer, we need not package those libs.
- The listening process has been refined, removing unnecessary threads for better performance.
- The message bus websocket port and path have changed to avoid conflicts. Mycroft now listens on port 8181 with the route '/core'

### Mycroft Mark 1 unit enhancements
- Our unit specific enclosure client (for updating Arduino sketches) and packaging method have been modified for platform detection either by configuration file or from serial port communication. This will allow for better compatibility for other systems, such as standalone Raspberry Pi's.

# v0.7.20
### Wifi Setup Client (for Mycroft unit)
- When no internet connection found (usually during STT) the instructions now direct user to press button on the unit
- Added auto-check for internet connection on bootup
- Wifi local access point activates at start, user gets guided into connecting
- Once connected to access point, captive web portal allows selection and configuration of desired wifi

# v0.7.18 / v0.7.19
- First pass of wifi setup, recalled

# v0.7.17

### Push to Talk
- Pressing the button when it isn't listening starts it listening
- Pressing the button when listening will stop the listen
- Added a mycroft.util.signal() mechanism for out-of-thread communication
- Pressing the button now creates an "buttonPress" signal from the Enclosure.  The viseme playback and aplay check for the 'buttonPress' signal to abort.

### Misc
- Removed "Sorry I didn't catch that", irritating during false activations
- added configuration for the Mimic TTS engine to change speech speed
- ArchLinux build script changes
- Introduced (but didn't enable) the wifisetup client

# v0.7.16

- Added support to the Enclosure API for the visualization of Text to Speech (using "visemes")
- Mycroft's default Mimic text to speech support now sends visemes to the Enclosure.  On a Mycroft unit, this produces lip syncing on screen when it speaks

This integrates changes from the last enclosure code release:

- Illumination changes also impact the matrix dispaly
- Added visemes, which will be visible once changes are made to the TTS client
- Removed the weather icons, added a generic mechanism for sending encoded icons across the serial line

It also breaks display of text and sprites, see https://github.com/MycroftAI/mycroft-core/issues/358
This will be addressed in an upcoming release.


# v0.7.15

Refined the enclosure menu behavior:
- Menu now comes up after pressing the button for 3 seconds
- Reordered the menu
- Added Illum menu item, which controls the brightness of the eyes
- Several minor visual refinements to the menu

# v0.7.14

### Hardware Changes
- Hardware units now show a spinning eye animation when shut down, rebooted, or turned on, and stop displaying it when power is fully on or off
- When volume is changed using the unit encoder dial, the eyes now display the current volume (0-11) for a few seconds, then reset to normal
- By holding down the encoder button on a unit for five seconds, users can access menu mode, which allows power control (reboot and shutdown), factory reset, and the hardware test (replacing the automatic hardware test trigger), and will soon include wi-fi setup

### Other Changes
- Volume levels can now be configured by the user to determine what each level (0-11) corresponds to
- Mycroft's speech is now broken up into smaller individual chunks, making it easier to understand


# v0.7.13

### Wolfram Changes
With this new release, the Wolfram Alpha Skill now gives much more concise responses. Rather than reading several definitions for a word, Mycroft will only read the first. Along with that, when Wolfram cannot find anything for a particular query, rather than speaking the result from the Did you mean? query, he simply mentions he could not find any results but did find something for an alternative query.

### Hardware Test
This release also brings in a basic hardware test. When activated by holding the button for 5 seconds, Mycroft will now run through tests to verify that the display, knob, microphone, and speaker are all working properly.

# v0.7.12

This is a patch release to fix an issue with the manifest file, preventing the new volume sound from being included in the packaging.

# v0.7.11

This is a patch release to fix an issue that was preventing avrdude from uploading Arduino code to the Mycroft unit.

# v0.7.10

This release includes some less visible changes and fixes. These changes and others coming soon will allow us to better iterate the development of mycroft-core, integrate multiple build systems, and decouple skills from the process.

### Volume changes
If you have a Mycroft unit, the volume behavior has changed. Delay has been decreased and instead of speaking the volume level setting at each click, a simple sound has been added. This reduces the chance of turning the dial several notches and then waiting for Mycroft to stop speaking. He will still speak if you set the volume with voice control.

### Enclosure improvements
The enclosure code is in the process of some heavy refactoring, but some quick fixes were added to make it more responsive and tweak the weather animation.

### Tweaks
We have also separated packaging scripts from this repository, so they are no longer tied to a release. This also reduces some clutter and opens the possibility to download daily master builds via apt. Check back for an update at http://docs.mycroft.ai.

# v0.7.8

### Base media skill
Thanks to [forslund](https://github.com/forslund) for his new base media class for developers to extend from. We think this will provide a more cohesive experience for all media skills, as they will all have similar behavior.

### Weather display on enclosure
This release also brings in a new piece of integration with the unit. When asked what the weather is, Mycroft will now display what the conditions currently are as well as what the current temperature is. This marks further improvement in enhancing the experience on the device.

### Bug fixes

- We fixed a bug where it would send `what 's` rather than `what's` to Wolfram Alpha, resulting in unintended behavior.
- We added the new media skill to the list of blacklisted skills in the skill loader so that it wasn't trying to load an effectively empty skill.
- We updated the `load_data_files` function to also load regex files, so that developers only need to call one function to load all of the files they need.

# v0.7.7
This update includes a few updates to the integration between the device's faceplate and skills, as well as some minor configuration changes.

### Device changes
We continued our integration of the faceplate with skills by updating it to work with the stock skill. Now, when you ask Mycroft what the stock price of a company is, he will display the company ticker and price, just like a traditional stock display.

We also started improving the user experience on the device by removing some of the delay with interacting with the button. Previously, when you pressed the button to stop a skill, you often had to wait a few seconds before it would actually stop. Now, skills should stop much more quickly.

### Wakeword configuration
We've received many questions about changing your Mycroft's name so that it will respond to whatever you want. Listening to your feedback, we've made this configuration process much easier than before. Now, all you need to do is put the new name in the configuration file, as well as its pronunciation. Take a look at our [documentation](https://docs.mycroft.ai/development/troubleshooting.notes#how-do-i-change-what-mycroft-responds-to) for more.

# v0.7.6

### Listener improvements
We've improved the listener in various ways this week. Specifically, there is now a significant drop in false positives in detecting the wakeword, making it so that Mycroft will respond less to phrases that don't contain `Hey Mycroft`.

To go along with these changes, we've also changed the wakeword from `Mycroft` to `Hey Mycroft` in order to make Mycroft more responsive.

### Sleep skill improvements
We also improved the sleep functionality of Mycroft. He will now respond much more consistently to `wake up`, rather than also waking up on other phrases like he did before.

### Wolfram Alpha skill improvements
Previously, queries to Wolfram would fail when it thought that there were other possible alternatives to the query said. Now, the Wolfram Alpha skill will now search again for the first suggested alternative, which increases its responsiveness.

Additionally, the WA skill will now properly reply to you when you say something like `what's a tangram`. Before, it would repeat the entire query back to you. Now, it parses your query to give a more natural response.

Finally, we removed the `I am searching for...` message, as it was frequently increasing response time, and sounded unnatural. We replaced it with the more natural `Sorry, I didn't find a response for...` on failure. 

### New animations
We've added a "thinking" animation to displayed on the faceplates of units while they wait to get information.

# v0.7.4

### New skills
We added three new skills this week - a stock skill that will give you stock prices of companies, a speak skill that will repeat what you say back to you, and a personal skill that allows Mycroft to tell you more about himself.

## Wolfram responsiveness
We significantly increased the amount of times that the Wolfram skill will give back a useful result.

### Unit integration
This update comes with more integrations with the unit and skills. Mycroft will now spell a word on his faceplate when you ask him to spell a word. 


