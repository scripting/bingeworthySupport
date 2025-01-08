#### 12/31/24; 9:47:47 AM by DW

We now return the home page for pages that aren't found. This allows things like this to work:

* https://tv.bingeworthy.org/scripting/

* It presents the profile page of the user.

* It's called pageRequested. 

* this feature works, but i punted on using it in the home app

#### 12/25/24; 10:10:48 AM by DW

Best Christmas present in a long time. Bingeworthy now supports WordPress identity. 

This means a lot of things.

1. It can now be used by other people.

2. When we edit comments written by users we can save it to a WordPress post. So BingeWorthy becomes a full featured publishing system and incredible exampleware for the ecosystem I want to start.

3. Unlike Twitter it isn't closed for business. But when I made the change from UserLand identity, which is my backup plan, I made it an option to go back and left all the code as it was debugged, so I'm pretty sure it works fine now, if for some reason WordPress goes through a transformation like the one Twitter went through. 

4. I'm sure I'll think of other things. 

Next up.

0. Roam over the UI looking for breakage. They are there. This was a big change.

1. Start the database from scratch to remove dave.winer@gmail.com as my identity, and start building with scripting as my identity. 

Starting the database from scratch

* drop database bingeworthy3;

* use the commands in the readme to recreate the database

* use the command in sqllog package to create the log table

* run importProgramsForBingeworthy3, importRatingsForBingeworthy3. there are calls to the functions commented out in start function at end of binge3.js.

* not sure about deleteProgramsNotRatedByDave -- it was confusing at first, but here's what it's doing. i must've registered a bunch of programs for people maybe before we had the ability for them to register. but i never watched or rated those programs. so we run this third, so that the initial state of bingeworthy is programs i have rated. 

#### 12/24/24; 9:31:18 AM by DW

Switching over to wordpress identity, instead of userland. Try to preserve the option of going back to userland. 

* big change in getScreenName -- if config.flUserlandForIdentity then we communicate with userland.scripting.com, otherwise wpidentity.scripting.com

* we include both userland and wordpress info in the clientConsts struct in returnServerHomePage. 

#### 12/11/24; 10:13:46 AM by DW

Moved theButtons from the Home app into config, so we can use the definitions when generating the RSS feed. 

#### 12/10/24; 10:57:43 AM by DW

The RSS feed works. 

You can subscribe at https://tv.bingeworthy.org/rss.xml.

Still needs some work to get it happy in the blogroll reader.

#### 12/8/24; 12:03:20 PM by DW

You will see notes here about isRatingInDatabase and -1. Today I learned that when I tried to fix that, not sure what it was fixing, but it badly broke rating of programs. I undid the change. Not sure what other problems this will uncover. 

#### 12/7/24; 10:09:18 AM by DW

Add a log table to the database.

Two events are logged, adding a program and rating a program.

Added a new endpoint for getting recent log items.

Next up --

* work on the UI of the log display in home app

* once that's settled, then add code to generate the RSS feed

#### 12/6/24; 11:04:50 AM by DW

isRatingInDatabase should just return a boolean indicating if the record is there

* it was doing something weird, and returning false if the value is -1

* i want to fix it now, and we'll see what breaks in the client. 

It shouldn't be possible to have two programs with the same title, so I added code to check before we add a program.

* This can happen if there's a page on metacritic for season 1 and one for the show in general. 

* The title in their view is the same. Can't do much with that. 

All the watch list endpoints work. 

#### 12/5/24; 10:01:10 AM by DW

initial database setup

* three routines for initial setup of the database

* importProgramsForBingeworthy3

* importRatingsForBingeworthy3

* deleteProgramsNotRatedByDave

* ran them in order, and that is how we start up the new bingeworthy

#### 12/2/24; 11:16:11 AM by DW

restored my ratings from backup, using new command

getRecentRatings now works, the list shows up in client

importRatingsForApi implemented

wrote getDataFromMetacriticPage, using regex instead of using a package

* bingeworthy 2 used open-graph-scraper, but the latest version doesn't run in Node on my desktop machine. 

* tried to figure out why, but decided it'd be easier to write my own scraper. it was. 

* in the next session i'll get the submitting process wired in again

#### 12/1/24; 10:30:05 AM by DW

next up

submitting a program

restore ratings from backup

decisions made

keep the programs table, toss the ratings

make it easy to restore ratings from backup

start the hotlist threshhold at 1 rating, it was 25 before

restart ratings table

drop table ratings;

create table ratings ( id int auto_increment primary key,  idProgram varchar (255) not null default '', screenname varchar (255) not null default '',  rating tinyint not null default 0, whenCreated datetime not null default current_timestamp,  whenModified datetime not null default current_timestamp on update current_timestamp,  comment text not null, unique key (screenname, idProgram) ); 

test rating things and there are problems, not surprising with this level of shakeup

select * from ratings where idprogram = 'https://www.metacritic.com/tv/30-rock' and screenname = 'davewiner';



https://userland.scripting.com/getuseridentity?token=xxx&clientkey=fmds8ootnzgfjqvnlnofee4dfhj6jb89xs0wjcjanl4klgdxfidtvhioumjtehna

select * from ratings where idprogram = 'https://www.metacritic.com/tv/30-rock' and screenname = 'dave.winer@gmail.com';

#### 11/30/24; 12:10:40 PM by DW

moved appConsts from code.js in the client, to index.html.

logging on and off userland now works



next up -- it's time to figure out how to start the database

* remove all programs?

* remove all ratings?



next up -- settings

* choose a name for yourself, so we don't have to show your email address to identify yourself

* make the email address they used to sign on a tool tip so it's easy to see



next up -- get my.bingeworthy to work off the new database



next up -- watchlist is just a boolean in the ratings record



next up -- a way to write comments on a rating, there's space there for it



next up -- go through the menu and implement each of the commands



next up -- look for broken images



next up -- review and smooth out userland.scripting.com user interface



#### 11/28/24; 9:52:40 AM by DW

getScreenName is now implemented for real, calling the userland server when it needs to know the user's screenname (now an email address)

binge3home is served from handleHttpRequest, as with all my recent apps. now we can transmit config values to the client from the server. no need to maintain values in two places.

#### 11/27/24; 9:13:45 AM by DW

added stubs for addToWatchlist, deleteFromWatchlist, isInWatchlist, getWatchlist.

in all the higher-level code in a rating.id is the url of a metacritic page, and there is no concept of a numeric id. in the database, id is the numeric id assigned by sql. 

so now when we convert a rating rec to return to the user interface code, 

we set the value of rating.id to idProgram from the SQL record, 

and set the value of rating.serialnum to id from the SQL record.

#### 11/24/24; 12:35:01 PM by DW

Two support routines for the tab display in the client, getHotlistWithUserRating and getAllProgramsWithUserRating.

#### 11/21/24; 1:05:18 PM by DW

Today I converted getAllPrograms, getUserRatings, getRating and started to convert setRating, and wrote calls to each in binge3Home.

Next up -- get setRating working, and continue converting. 

#### 11/20/24; 9:53:49 AM by DW

next up, converting all the old bingeworthy2 functions to the version 3 database.

initially we were writing this to this path on S3

* /scripting.com/code/userland/demos/binge/

but this is really the new bingeworth server so

* /scripting.com/code/binge3Server/

queries

* select id, screenname, title from programs;

* select id, rating, idProgram from ratings where screenname = 'davewiner';

* select r.rating, p.title from ratings r inner join programs p on r.idprogram = p.idprogram where r.screenname = 'davewiner' order by r.rating desc;

#### 11/19/24; 12:41:53 PM by DW -- bingeworthy2 database

The bingeworthy2 database is documented <a href="https://github.com/scripting/binge2?tab=readme-ov-file#sql-to-create-tables">here</a>. 

#### 11/18/24; 4:53:00 PM by DW

Started.

