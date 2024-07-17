---
layout: post
title: "headlineparse + children and media use"
date: 2024-07-12
---


Made a small update to the headlineparse tool, which you’ll find [here](https://github.com/rdef/headlineparse).

## Tool overview
The tool can be used as-is from a directory, provided python and the related libraries are installed. It parses any RTF files that appear to match the Factiva layout within its directory, and then turns each headline into a DataFrame row. It performs sentiment analysis on the headline, and then exports it as an Excel spreadsheet. It doesn’t have onboard visualisation capabilities in order to keep the project reasonably lightweight, so any charts will need to be created in another tool.

The tool uses `[pandas](https://pandas.pydata.org/)` for DataFrames and `[vader](https://vadersentiment.readthedocs.io/en/latest/)` for sentiment analysis. Vader is more useful for getting consistent sentiment analyses from short text, although like all sentiment analysis, caution should be taken in assumptions made between different datasets and forms of language.

## Project history
This tool was developed for a project that sought to track changes in news headlines related to children and digital media. The project itself used a reasonably long but uncomplicated search term (noted below) to identify candidate articles, then download the headlines into RTF files that could then be manually analysed and qualitatively coded. This represented quite a large organisational task, as well as consistency issues related to qualitative coding of large datasets.

I developed `headlineparse` to overcome these issues, and it works well enough. I’d note that, depending on your institutional license, you may or may not be able to use the tool in this way.

## Example
In our case, we used the tool to capture some 1200 articles, with ~100 duplicate entries, published over 23 years. The graph shows composite scores of sentiment, ranging from 1.0 (most supportive) to -1.0 (most oppositional) regarding the topic.

The major gridlines are yearly, and I’ve added in a red trend line that notes the tendency towards more negative content over time. I’d just caution the value of the trend line itself, as these are not really meant to track data of this type in this way. While it does present a trajectory, this is mainly because there are quite a few 0-scored items early on, and it is using the first entry as a starting point. It would be better to signal a rolling average score on a weekly or monthly basis, but we needed this graph for discussion more than we needed it for publication. What the trend line does do is point to a trend towards a negativity of -0.2. While this may not sound significant, vader does not provide a relative compound score of sentiment. This is an absolute score. This would suggest that there is a significant negative tendency here.

![Graph of the sentiment analysis of the captured headlines](_posts/images/children-and-tech.png)

There are a lot of neutral articles sitting at or near 0, and these are indicative of items that are not making any significant claim in their title, while still speaking to the topic of children and technology. It would be easy to read this neutrality as being ‘unimportant’ or otherwise signalling a non-problem, but it’s worth paying attention to the degree to which technology is seen as unremarkable or unimportant.

## Data selection and reliability
Headlines operate on a clickbait logic, which is to say that they are an entreatment to read that theoretically draw on traditional rhetoric to engage readers. Of this, pathos is notably highly effective in engaging readers (ethos at times, and rarely logos) and drawing engagement.

What this means is that headlines are likely to be relatively heightened in emotional terms relative to both the situation being discussed and the content at the heart of the matter. This also means that sentiment analysis tools, such as vader, are going to find it easier to have sensitive responses to short form text, as it will tend to include phrases and terms that are key for sentiment analysis. What is also important for this is that, barring opinion/editorial content, headlines are generally going to avoid having the kinds of statements that tend to trip up sentiment analysis, specifically irony, insincerity, and pantomime. Can’t say the same thing for puns, however.

The source data for the graph above was created with a provisional search string, which will be revised for the final working paper. The newspapers targeted for analysis were selected to align with the papers that the National Library of Australia keeps on record, which are noted here. The timeframe for data selection runs from 2000/01/01 to 2022/12/31. No other search terms were filtered.

`(child* OR infant* OR toddler* OR baby OR babies) AND (digital* OR technolog* OR internet OR online OR electronic* OR networked OR screen time OR mobile OR phone* OR smart phone* OR smartphone* OR cell phone OR tablet OR ipad OR iPhone OR computer* OR "smart toys" OR “lectronic toys”R social media OR platform* OR youtube OR video* OR streaming OR streams OR games OR gaming OR messaging OR chatroom*)`

