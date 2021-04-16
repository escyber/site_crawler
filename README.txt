This is a set of programs that emulate some of the functions of a 
search engine.  They store their data in a SQLITE3 database named
'spider.sqlite'.  

The program consists of 3 parts: Python Search Spider, Page Ranker, and Visualizer


Prerequisites 


Install:

1. The SQLite browser to view and modify the databases from:
http://sqlitebrowser.org/

(!) When starting a new crawling project, this file can be removed.
Mac: rm spider.sqlite

2. Source code editor, for example Atom 

== 1ST STEP - Python Search Spider ==

Update line 33 of "webcrawler.spider.py" with any web url you are going to crawl. 
For example: = 'https://www.apple.com/'


== 2ND STEP ==

In Mac terminal run: 
python3 webcrawler.spider.py

The program crawls a web site and pulls a series of pages into the
database, recording the links between pages.


Enter web url: https://www.apple.com/
['https://www.apple.com']
How many pages:3
1 https://www.apple.com (72438) 125
42 https://www.apple.com/today/camp (235160) 14
28 https://www.apple.com/us/shop/goto/buy_accessories (173515) 172

In this sample run, we told it to crawl a website and retrieve tree 
pages.  If you restart the program again and tell it to crawl more
pages, it will not re-crawl any pages already in the database.  Upon 
restart it goes to a random non-crawled page and starts there.  So 
each successive run of spider.py is additive.

The spider chooses randomly amongst all non-visited links across all
the webs.

== 3RD STEP - Page Ranker ==

If you want to see the number of incoming links, the old page rank, the new page
rank, the id of the page, and the url of the page, dump the contents of the spider.sqlite file with spdump.py as follows:

Mac: python3 webcrawler.pdump.py 

(32, None, 1.0, 1, u'https://www.apple.com')
(28, None, 1.0, 28, u'https://www.apple.com/us/shop/goto/buy_accessories')
(3, None, 1.0, 42, u'https://www.apple.com/today/camp')
(3, 'rows.')

The webcrawler.pdump.py program only shows pages that have at least one incoming link to them.

== 4TH STEP - Page Rank ==

Once you have a few pages in the database, you can run Page Rank on the
pages using the sprank.py program.  You simply tell it how many Page
Rank iterations to run.

Mac: python3 webcrawler.prank.py 

How many iterations:2
1 0.6666666666666666
2 0.6666666666666666
[(1, 1.0), (42, 1.0), (28, 1.0)]

You can dump the database again to see that page rank has been updated:

Mac: python3 spdump.py 

(32, 1.0, 2.0, 1, 'https://www.apple.com')
(28, 1.0, 0.5, 28, 'https://www.apple.com/us/shop/goto/buy_accessories')
(3, 1.0, 0.5, 42, 'https://www.apple.com/today/camp')
3 rows.

You can run sprank.py as many times as you like and it will simply refine
the page rank the more times you run it. If you want to restart the Page Rank calculations without re-spidering the web pages, you can use webcrawler.preset.py

Mac: python3 webcrawler.preset.py

All pages set to a rank of 1.0

Mac: python3 sprank.py 

How many iterations:50
1 0.6666666666666666
....
50 0.6666666666666666
[(1, 2.0), (42, 0.5), (28, 0.5)]


For each iteration of the page rank algorithm it prints the average
change per page of the page rank.   The network initially is quite 
unbalanced and so the individual page ranks are changeing wildly.
But in a few short iterations, the page rank converges.  You 
should run prank.py long enough that the page ranks converge.

== 5TH STEP - Visualization ==

If you want to visualize the current top pages in terms of page rank,
run spjson.py to write the pages out in JSON format to be viewed in a
web browser.

Mac: python3 spjson.py 

Creating JSON output on spider.js...
How many nodes? 20
Open force.html in a browser to view the visualization

Mac: Open force.html

You can view this data by opening the file force.html in your web browser.  
This shows an automatic layout of the nodes and links.  You can click and 
drag any node and you can also double click on a node to find the URL
that is represented by the node.

The project is build based on University of Michigan Python Specialization course available on Coursera. 

This visualization is provided using the force layout from:

http://mbostock.github.com/d3/

If you rerun the other utilities and then re-run spjson.py - you merely
have to press refresh in the browser to get the new data from spider.js.

