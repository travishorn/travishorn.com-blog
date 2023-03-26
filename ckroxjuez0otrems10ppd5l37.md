---
title: "My Title Boxing Club schedule scraper"
datePublished: Thu Sep 27 2018 22:01:01 GMT+0000 (Coordinated Universal Time)
cuid: ckroxjuez0otrems10ppd5l37
slug: my-title-boxing-club-schedule-scraper-22525d797f3d
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409643775/qjDBt23T3.png
tags: nodejs, cli

---


I built a tool to scrape the latest schedule from Title’s website and output CSV data ready for import into Google Calendar.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409639964/w49c1baDE.png)

Title Boxing Club is a fitness gym that holds trainer-led classes daily. On their website, each location has its own “homepage.” You can find the schedule for the next 11–12 days on this page. This is the only way to access the schedule online. There is no iCal or alternative transportable version. This makes it difficult to integrate their schedule with your own personal calendar.

I decided to create a command line utility that solves this problem.

If you feed this command a location’s homepage URL (for example: [https://titleboxingclub.com/chicago-south-loop-il/](https://titleboxingclub.com/chicago-south-loop-il/)), it will pull the available schedule and automatically create a CSV file.

The output can be saved to a file and [imported into Google Calendar](https://support.google.com/calendar/answer/37118?hl=en).

### Installation

You will need to have [Node.js](https://nodejs.org/en/) installed already. Once it is installed, clone the repository and install dependencies.

```
> git clone [https://github.com/travishorn/title-schedule-scraper.git](https://github.com/travishorn/title-schedule-scraper.git)
> npm install
```


### Usage

1. Go to [https://titleboxingclub.com](https://titleboxingclub.com)

1. Enter your zip or city and state in the search bar

1. Click your location

1. Copy the URL

Once you have the URL, simply run…

```
> node index [options] <url>
```


### Options

* `-V, --version` Output the version number

* `-c, --class &lt;name&gt;` Only include the specified class type (ex: Boxing)

* `-d, --duration &lt;minutes&gt;` Only include the classes that last the specified number of minutes (ex: 60)

* `-t, --trainer &lt;name&gt;` Only include the specified trainer (ex: Jordan)

* `-h, --help` Output usage information

### Example

```
> node index -c Boxing -d 60 -t Jordan https://titleboxingclub.com/chicago-south-loop-il/
```


This command will output CSV data to the terminal. You can pipe this data to a file like so:

```
> node index https://titleboxingclub.com/chicago-south-loop-il/ > title-schedule.csv
```


Once you have the CSV, you can [import it into Google Calendar](https://support.google.com/calendar/answer/37118?hl=en) if you wish.

![The end result](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409641801/cYdmzv23U.png)*The end result*

Since Title understandably only supplies the schedule for the next week or so, you’ll need to rerun the process periodically.

### How does it work?

You can see the full source code on GitHub.
[**travishorn/title-schedule-scraper**
*Scrapes the latest schedule from Title's website and outputs a CSV file ready for import into Google Calendar. …*github.com](https://github.com/travishorn/title-schedule-scraper)

Each time the command is run…

1. CLI options are parsed by [Commander](https://www.npmjs.com/package/commander)

1. The URL is requested by [Axios](https://www.npmjs.com/package/axios)

1. [Cheerio](https://www.npmjs.com/package/cheerio) parses the returned HTML

1. Details of each class are loaded into an array of objects

1. If any options are specified (ex: Only kickboxing), the array is `filter()`ed for each one.

1. The filtered array is fed into [csv-writer](https://www.npmjs.com/package/csv-writer), which uses [Moment](https://www.npmjs.com/package/moment) to parse and format the dates/times

1. csv-writer outputs data in CSV format