# ACM Scoreboard
>developed for codeforces.com

[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com) [![Release Version](https://img.shields.io/badge/version-1.5.1-blue.svg)](https://github.com/ThunderStruct/ACM-Scoreboard/releases) [![License](https://img.shields.io/badge/license-CC%20BY%204.0-lightgrey.svg)](http://creativecommons.org/licenses/by/4.0/)

## Usage

#### Hosted Version (recommended)

The scoreboard is already [hosted](http://www.thunderstruct.com/acm-scoreboard/), tested, and ready to use!

_Note: the server sleeps for 1 hour daily at 1AM GMT. If that's an issue, consider the next option_

#### Localhost

To ensure absolute control over the running environment, clone the project and run it on a local (or remote) HTTP server.

_Note: Python is required for the following steps_

1. `cd` to a desired directory to contain all the project files
2. `git clone https://github.com/ThunderStruct/ACM-Scoreboard.git`
3. `python -m SimpleHTTPServer` for Python 2.x. Alternatively, use `python -m http.server` for Python 3.x
4. The scoreboard is now accessible through port 8000 (`localhost:8000`)

This platform requires a CORS-enabled (or HTTP) server. Localhost servers do not normally have cross-origin resource sharing on, in that case one of the following browser plugins can be used:

  - Chrome: [Allow-Control-Allow-Origin: *](https://chrome.google.com/webstore/detail/allow-control-allow-origi/nlfbmbojpeacfghkpbjhddihlkkiljbi?hl=en)
  - Firefox: [CORS Everywhere](https://addons.mozilla.org/en-US/firefox/addon/cors-everywhere/)

## Setup

There are 3 main components in the setup screen:

  1. **User handles** - can be inserted one at a time or using a `'\n'` separated file (carriage return `'%0D'` can also be used)
  2. **Problem ID and color** - the contest ID + letter pair that identify a Codeforces problem and optionally an associated color (set to `White` if not chosen). All problems' weights are 500 points by default, which can be changed by hovering over the inserted entry
  3. **Contest duration** - the contest's start and end times (GMT not local time!)

The verify button in the setup screen cross-references the given handle names' solved problems with the given listed problems (for the contest creator's awareness - requires at least 1 handle and 1 problem)

Once the setup is complete and the "START CONTEST" button is clicked, pre-contest preparations start. The preparations include:

  1. **Fetching Missing Problem Names** - in case the contest was copied from a running one, all previously fetched problem names will be copied, resulting in less preparation time. In case of an error during problem-name retrieval, an input toast will be shown to prompt for problem removal or manual name insertion (empty strings are allowed)
  2. **User Handles Validation** - the inserted user handles are recursively validated. All invalid user handles trigger a confirmation toast to remove or keep them


## Scoreboard

  - The contest table's rows are dynamically sorted descendingly (top-scorer at the top) after each update

  - The scoreboard updates the data automatically once every 2.5 minutes from the last update

  - The scoreboard table shows the retrieved data in the form of (number of submissions / total problem penalties)

  - Color coding:
    - Red: only wrong submission(s)
    - Light green: at least 1 correct submission
    - Dark green: first submission of a sepcific problem

  - The scoreboard table wrapper is draggable (enable/disable this feature using the hotkey `d`). This feature was added to help with projectors' misalignment and similar cases


## Tools

Some useful tools are found in the floating tools button at the bottom-right corner of the screen!

### Hotkeys

|  Key  |                    Action                    |    Default     |
|:-----:|:--------------------------------------------:|:--------------:|
|  `t`  |       Toggle the tools floating button       |  On (visible)  |
|  `d`  |  Enable/disable the contest table dragging   |    Disabled    |
|  `f`  |              Toggle fullscreen               |       Off      |
|  `m`  |       Mute/unmute contest ending sound       |     Unmuted    |

#### Documentation

This method provides a reference by opening the website's documentation in a new tab

No Requirements

#### Light Mode

This tool toggles between Lights On/Off styles

No Requirements

#### Log Detailed Report

This method displays more detailed information corresponding to each entered Codeforces handle in the developer console.

*note: this method logs potentially large amounts of data, and developer consoles on many browsers might hide part of the report if the console is not open. To avoid that issue, make sure the developer console is _open_ while using this tool*

Requirements:
  - At least 1 entered user handle
  - Setup start time and end time (optional - filters out the the details that do not fall in the given time range)

The results of the method *per handle* include:
  - Handle name
  - A table of total submissions count and correct submissions count grouped by day
  - A list of grouped details of each submission
  
![getSubmissionDetails() example](https://i.imgur.com/yyq6AU6.png)

#### Log Last Submission

This method displays the last *accepted* submission's data as of the time it's called in the developer console

Requirements:
  - At least 1 problem submission

The results of the method include:
  - Handle name
  - The last submission's problem details


#### Copy Contest

This tool generates an encoded, compressed string and copies it to the clipboard. It is recommended to always have that encoded string in case of an accidental page-refresh or any technical issue to easily load the contest quickly without re-entering the data

Requirements:
  - At least 1 entered user handle
  - At least 1 entered problem data
  - Contest start and end times

  *note: ~~the generated string does _NOT_ contain cancelled submissions' data. All cancelled submissions must be re-entered!~~ the generated string contains all cancelled submissions' data as of version 1.1.0*


#### Load Contest

This tool takes a previously generated encoded setup string (using the Copy Contest tool), decodes it, and loads the setup data

Requirements:
- To be in _setup_ mode (contest not started)


#### Modify Contest Duration

This tool modifies a current contest's duration by adding or subtracting a given amount of time (in seconds)

For instance, to add an extra minute to the current contest, insert `60` (seconds). To subtract half an hour from the remaining duration, insert `-1800`

A confirmation toast will appear after insertion showing the timer value after the modification

Requirements:
- A running contest


#### Cancel Submission

This tool takes a codeforces submission ID (can be found using Log Detailed Report or, alternatively, `console.log(scores)` in the developer console) to cancel it incase of an illegal submission or similar

As of version 1.2.0, a submission ID can be removed from the cancellation list simply by adding a negative `-` sign before the ID

Scores _must_ be updated afterwards to see effect

~~*note: currently, the only way to _undo_ this action is to setup the contest again*~~

Requirements:
- At least 1 problem submission


#### Toggle Blind Mode

This method toggles the _blind_ mode on and off. During blind time, automatic score updating is disabled (can still be manually updated, however)

Requirements:
- Contest must be running

#### Update Scores

This method manually updates the scores and resets the automatic updater's interval

Requirements:
- Contest must be running

## Scoring

* Difficulty (the points assigned to the problem, which is 500 by default and can be changed by hovering over the problem and choosing a new score)
* The submission time relative to the contest start time
* The number of wrong submissions
* The submissions evaluation time (code efficiency; has minor weight)
* All scoring components are based on a time-related factor.
1. 20 minutes = [(problemPoints / 2) / (duration / 20)] points
2. The above formula would penalize a last-second submission by half the problem points
3. Each wrong submission is penalized by 10 minutes
4. Problem evaluation time gives up to 20 minutes bonus (for a 15ms submission)
* Wrong submissions on unsolved problems do not affect the total scores.

Title attributes are assigned to the contest table's cells for convenience

* Hovering over any problem in the header row of the contest table will display its score points
* Hovering over any submission cell will display the last submission's date/time
* Hovering over any handle will display the user's current total score

*note: the scoring equation can be customized in contest.js -> calculateScore()*

## Credits
- This scoreboard is solely created by Mohamed Shahawy (ThunderStruct)
- Hubspot's [PACE](github.hubspot.com/pace/docs/welcome/) loading screen is used
- A modified version of [jQuery-SimpleColorPicker](https://github.com/tkrotoff/jquery-simplecolorpicker) (available [here](https://github.com/ThunderStruct/jquery-simplecolorpicker))
- Matthew Crumley's SO [post](https://stackoverflow.com/a/294421/3551916) on LZW string compression

**Happy ACMing!**

## Todos

  - [x] Add a "legend" over the scoreboard to describe the color coding scheme (added in 1.1.0)

  - [x] Add the cancelled submissions to the encoded setup string generated by the Copy Contest tool (added in 1.1.0)

  - [x] Add a tool to edit the contest time (add or subtract x minutes from remaining duration) (added in 1.3.0)

  - [x] Add a tool to enable/disable contest table dragging (added in 1.3.0 as a hotkey instead of a tool)

  - [x] Show a confirmation alert on page-unloading attempts while a contest is running (added in 1.1.1)

  - [x] Display scores data in the console when blind-mode is on (added in 1.4.0)

  - [x] Auto retrieve the problems' names using the given problem IDs and replace the "Problem name" field with "Problem color" (added in 1.4.0)
  

  #### Contribution Guide

  Feel free to make a pull request if you find any room for enhancements or fixing bugs. The following brief file descriptions should be helpful! Please make sure to read the repo's [CONTRIBUTING](https://github.com/ThunderStruct/ACM-Scoreboard/blob/master/CONTRIBUTING.md) guide before making any changes
  

## License
![CC](https://i.creativecommons.org/l/by/4.0/88x31.png) This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/)

