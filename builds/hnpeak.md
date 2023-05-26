---
title: HNPeak
tagline: Find out what rank a HackerNews post peaked at
---

# HNPeak
gh: [https://github.com/lukew3/hnpeak](https://github.com/lukew3/hnpeak)

When I found out that somebody had posted [lukew3/mathgenerator](https://github.com/lukew3/mathgenerator) to [YC Hackernews](https://news.ycombinator.com) I wanted to know what position [that post](https://news.ycombinator.com/item?id=34047076) had peaked at. By the time that I noticed that Github stars and issues were pouring into my Github, the post had been up for about 3 hours and was at rank 7. I knew that it was likely at a higher position earlier but didn't know what it had reached. This curiosity led me to desire a product that could tell you which position a HN post had peaked at. Months later, I decided to build it.

The idea is pretty simple. Every time that HN is updated, scrape the website and check if there is an entry that is at a higher position than it was the last time it was checked. The database is 3 columns:
```
id (INT) | peak_rank (INT) | peak_time (INT)
```
Since HN is designed to be minimal, the scraping was easy. The main content of the site is a table with each post entry being a `<tr>` tag with an id attribute equal to the id in the query string of discussion posts. If the id isn't found in the table, add it with the peak rank being the current rank and peak time being the current time in seconds from epoch. If id is found and the current rank is better than the peak rank update the peak rank and peak time to the current value.

I decided to write this program in Rust because I haven't done a proper project in Rust yet and wanted a program that can run as a small process on my server at minimal cost. I stumbled my way through this build, but I'm happy that I got it to work.

