---
layout: post
title: Test 2 Discussion
---

+ ER -> Relational
+ 3rd Normal form
+ Indexes
	+ Inserting into a B+ tree
+ MongoDB
+ NOSQL
	+ Transactions
+ Cheat sheet
	+ 8 1/2 x 11

## Review of HW6
+ #6
	+ For all type Member Events that have an actor id < 1200000, list its actor id and repo name.

+ 200,000 tuples in DB
	+ 50 tuples per page
		+ 4,000 pages
	+ How many pages are accessed?
		+ 4000 if no index
		+ If there is an index
			+ B+ tree index
				+ clustered
					+ leaf nodes are pointers to pages we want to receive
					+ Let's say the selectivity is 1/200
						+ will select 1000 rows
						+ 100 index entries per page
							+ Have to traverse tree and find the correct leaf node
							+ ![](https://www.simple-talk.com/iwritefor/articlefiles/610-image002.jpg)
							+ 3 non-leaf nodes + 1000/100 + 100/50 = 33 pages
				+ non-clustered
					+ assume the worst case where they're all on a seperate page
					+ 3 non-leaf nodes + 1000/100 + 10000 = 1013 pages
