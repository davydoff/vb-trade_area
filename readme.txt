vBulletin addon with extra functions for buy/sell forums 

- Adds "topic closed" background image for finished items
- Adds mini-help for new topic
- Adds mini-help for new replies
- Change edit time + merge time for posts

Additional settings to use this mod:

1. Install "Double Post Prevention Plus" addon
2. Set allowed edit time to ~60 minutes (global)
3. Set defaul tmerge time to ~60 minutes (global)
4. Disable users to reply other threads in buy/sell forums
5. Change button behavior and add close/open buttons to threads
Not released at vborg.
Not polished

=== Delete tags from trade area ===

1. Show tags from trade area:

SELECT t.tagtext, th.title
FROM
     tag t,
     tagcontent tc,
     contenttype ct,
     thread th
WHERE 
     t.tagid=tc.tagid AND
     tc.contenttypeid=ct.contenttypeid AND
     ct.class='Thread' AND
     th.threadid=tc.contentid AND
     th.forumid IN (4,5,6) -- your trade area forumid

2. Delete tags from thread table

UPDATE thread
SET taglist = NULL
WHERE forumid in (4,5,6) AND -- your trade area forumid
      taglist IS NOT NULL

3. Delete from tagcontent table

DELETE FROM tc
USING
     tagcontent tc,
     contenttype ct,
     thread th
WHERE 
     tc.contenttypeid=ct.contenttypeid AND
     ct.class='Thread' AND
     th.threadid=tc.contentid AND
     th.forumid IN (4,5,6) -- your trade area forumid

4. Delete from tag table (all the tags that are not used anywhere)

DELETE FROM t
USING tag t, 
(SELECT t.tagid, 
        count(tc.tagid) tagcount
FROM tag t 
LEFT JOIN tagcontent tc ON tc.tagid=t.tagid
GROUP BY tc.tagid) tg
WHERE t.tagid=tg.tagid AND
      tg.tagcount=0

