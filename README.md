 [![Build Status](https://travis-ci.org/rynecarbone/power_ranker.svg?branch=master)] [![version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/rynecarbone/power_ranker/blob/master/CHANGELOG.md) [![PyPI version](https://badge.fury.io/py/power_ranker.svg)](https://badge.fury.io/py/power_ranker)

# Power Ranker
A tool for scraping fantasy football stats from ESPN leagues, creating power rankings, and publishing the rankings to a website

# Check out package

```python3
pip3 install power_ranker
```

OR

```bash
git clone https://github.com/rynecarbone/power_ranker
cd power_ranker
python3 setup.py install
```

# Generate a Configuration File
Download a local copy of the default configuration file. Once you have the file, fill in your league name, league id, year, and week (that you want to make rankings for). You can also adjust any of the settings that control the algorithms in the power rankings, or use the default values. For detailed instructions, see the [Configuration File Documentation](https://github.com/rynecarbone/power_ranker/blob/master/power_ranker/docs/ConfigurationDocumentation.md)

To download a local copy of the configuration file, use the -dc (--download-config) option. If you are in a private league, use the -p (--private-league) option to log into your ESPN account and retreive cookie information:
```bash
power_ranker -dc 
Creating copy of: /Path/To/power_ranker/docs/default_config.cfg
To local destination: /Path/To/Current/Dir/MY_LOCAL_CONFIG.cfg
```
or 
```bash
power_ranker -dc -p
Creating copy of: /Path/To/power_ranker/docs/default_config.cfg
To local destination: /Path/To/Current/Dir/MY_LOCAL_CONFIG.cfg
Username: <Enter ESPN Login>
Password: <Enter ESPN Password>
```

Use your favorite editor to open the local "MY_LOCAL_CONFIG.cfg" file and edit it.

# Command line
After you have added your league information, pass the configuration file as an argument with the -c (--config-file) option. If you haven't already, you can add the -p (--private-league) option to log into your ESPN account and retreive cookie information.
```bash
power_ranker -c MY_LOCAL_CONFIG.cfg 
Using MY_LOCAL_CONFIG.cfg to generate power rankings

Week 3 Power Rankings
...
```
Alternatively, pass the league id, year, and week as command line arguments (-l --leagueid, -y --year, -w --week) and all other default power ranking settings will be used. Pass the -p option if in a private league.
```bash
power_ranker -l 123456 -y 2017 -w 3 
Using user input:
League ID: 123456
Year: 2017
Week: 3
Creating copy of: /Path/To/power_ranker/docs/default_config.cfg
To local destination: /Path/To/Current/Dir/MY_LOCAL_CONFIG.cfg

Week 3 Power Rankings 
...
```
After you run the rankings, a template website will be generated in a directory titled "output/". Follow the instructions on how to [Publish Power Rankings to a Website](https://github.com/rynecarbone/power_ranker/blob/master/power_ranker/docs/PublishingWebsite.md) if you want to share the output with your league. To add your own summary to the week's power rankings, edit the file "output/2017/power.html". Find the commented out section:
```html
<!--- <p>FIXME! FIXME!
         Add your own commentary here! New write-up here!
         Or just leave the rankings</p> -->
```
Remove the `<!--` and `-->` comment delimeters, and edit the text inside the paragraph markers (`<p>`, `</p>`).

# Manipulate League objects in python interpreter
After you have added your league information to the local configuration file, you can open python3 interpreter:
```python3
python3
>>> from power_ranker.league import League
>>> my_config = 'MY_LOCAL_CONFIG.cfg'
>>> my_league = League(my_config)
>>> my_league.get_power_rankings()
```
## Recalculate power rankings for a separate week
After you have a league object, you can get power rankings for a different week as well (example, for week 3):
```python3
>>> my_league.get_power_rankings(3)
```
## Generate website after calculating rankings
After you have calculated the power rankings for the desired week, you can create a directory with html 
files to showcase the rankings online. For an example of how to use Github Pages to host the website, 
see the documentation on [Publish Power Rankings to a Website](https://github.com/rynecarbone/power_ranker/blob/master/power_ranker/docs/PublishingWebsite.md)
```python3
>>> my_league.make_website()
```

# Credit
The code to extract league info from hidden ESPN API, including retreiving league settings and accessing private league data, is largely thanks to
Rich Barton's espnff package: https://github.com/rbarton65/espnff.
