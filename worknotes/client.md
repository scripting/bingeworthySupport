#### 1/8/25; 11:13:47 AM by DW

Turned off the websockets connection to wpidentity server. It's getting errors, every five minutes -- and we don't use the feature anyway, so this can be dealt with later.

Working on the View Recent Log Items dialog, why were some program names blank? Because the program was just added to the database, and we weren't getting the information locally, it wasn't in our version of the programs list. Added code to reload the program list before displaying the table.

#### 1/7/25; 6:08:01 PM by DW

Added new command to View Recent Log Items. It shows you everything, not just one type of event. Pretty ugly but useful!

#### 1/7/25; 11:08:58 AM by DW

Someone named "hackney" stumbled across the site, and apparently it worked well enough for them. 

So let's give it a whirl. 

I didn't do all the stuff on the 12/31 list.

I did do the custom page, but when it came time to hook it in, I realized I'd have to set up a whole wpidentity server for it.

* it could still use the same database

* but wpidentity.scripting.com was already hooked into blogbrowser.org, which was a very early version of wordland, so i changed it so that it says it's generic app on wordpress.com that i use to register all my little apps, like bingeworthy

* i had remembered wpidentity as having a way to configure more than one app, but that must've been something else, it does not have the capability. 

Wrote a blog post and updated the docs. This is so much more fun if people use it. It's rough, but it works. 

#### 12/31/24; 9:30:44 AM by DW

started to do this work, but i'm overloaded and not motivated

a url system to get to places from outside, most do not require login

https://tv.bingeworthy.org/

sign on page, or if they are signed on the current home page

https://tv.bingeworthy.org/scripting/

the user's profile page, with all the shows they have rated, linked to?

https://tv.bingeworthy.org/hotlist

https://tv.bingeworthy.org/recent/ratings

https://tv.bingeworthy.org/recent/additions

things i want to do now

urls in address bar trick so we can send around links easily

my.bingeworthy functionality

cosmetic stuff, eg the sign on page says userland but we're using wordpress

create its own app on wordpress.com, right now it's using blogbrowser.org.

hook up ratings to a bluesky account

learn how to create a new bluesky account

done

delete feeds i'm not subscribed to

#### 12/11/24; 11:19:23 AM by DW

Added a new "Me" tab that shows my own ratings.

#### 12/8/24; 2:02:15 PM by DW

review menu for commands that can go

* hotlist -- not needed in menu, it's one of the tabs

* similar users and prolific raters reports

since i am the only user right now, these should be commented out in the ui for now until such time as there are more users

#### 12/7/24; 11:42:03 AM by DW

Started work on displaying log items. In next session work some more on that. Added command to main menu to make it easy to preview. 

#### 12/6/24; 11:04:50 AM by DW

Got the watch list working.

If we click on "no opinion" we were previously deleting the rating, but that's not right, now wekeep the rating and understand in other places that the rating could be negative.

#### 12/5/24; 11:54:59 AM by DW

getScreenname was a stub, now it is filled in properly. 

when submitting a new program, we shouldn't have to reload the page to see the new program

removed import ratings command

#### 12/2/24; 4:17:21 PM by DW

Next up

list of programs by when added



My watchlist from the NYT best tv shows of the year

https://www.metacritic.com/tv/fantasmas/ (hbo)

https://www.metacritic.com/tv/say-nothing/ (hulu)

https://www.metacritic.com/tv/my-brilliant-friend/ (hbo)

https://www.metacritic.com/tv/somebody-somewhere/ (hbo)

https://www.metacritic.com/tv/baby-reindeer/ (netflix)

https://www.metacritic.com/tv/the-decameron/ (netflix)

https://www.metacritic.com/tv/the-diplomat-2023/ (netflix)

https://www.metacritic.com/tv/evil/ (paramount)

https://www.metacritic.com/tv/hacks/ (hbo)

https://www.metacritic.com/tv/sunny/ (apple)

https://www.metacritic.com/tv/the-sympathizer/ (hbo)

https://www.metacritic.com/tv/my-lady-jane/ (amazon)

https://www.metacritic.com/tv/3-body-problem/ (netflix)

Next up, finish the submit program user interface.

* must open the program page for the program

* report errors

#### 12/1/24; 11:30:55 AM by DW

I want an import ratings command.

you enter the name of the user you want to read from

we load it in from the github repo

confirm you want to import 1200 ratings (there is no undo!)



i have it working, tested with one time through the loop

next up, run the whole thing, and look to see if everything got through?





The hotlist and user ratings commands in the menu now work. 

#### 11/30/24; 11:59:23 AM by DW

appConsts moves into the HTML source head section, so it can be set on the server.

#### 11/29/24; 5:54:14 PM by DW

started work on logging on and off userland

got to the place where this is the url

http://localhost:1410/#?appkey=i8jo9qxxx76qtdnj&email=dave.winer%40gmail.com

#### 11/27/24; 8:58:38 AM by DW

in servercall, if flAuthenticated, add parameter userlandkey which contains the appkey for userland for this app.

switch the server we're calling to the new binge3server running locally, away from the old binge2server which was tied to twitter, and also has older less efficient sql code.

#### 11/26/24; 4:42:31 PM by DW

This approach worked really well. 

Next up --

* get the identity system functioning so I can start using my email address as identity.

we have to use the new server, not the old server, which is hooked up to Twitter for identity

that's why none of the calls we're making work, we're sending old useless credentials

over on the new server, we will send the user's credentials to the userland server, and it will return an access token just like twitter did

now we have the identity of the user on the server

* get the settings page working too.

* can we rate programs?

* can we add new programs?

* what about the special readouts?

#### 11/26/24; 9:38:51 AM by DW

I'm going to try turning in a new direction.

start with the bingeworthy2home code and simply port it to use userland for id.

i did a lot of work on the UI, and starting over from scratch means a lot of features will be missing, or i'm looking at a full-month or more project.

i moved the three files from that version into a new project called bingeworthyJunk, open it by cmd-/ on the line of code below

nodeEditorSuite.openProjectWindow ("bingeworthyJunk")

#### 11/24/24; 12:33:32 PM by DW

I have the two-tab display working. And code to do many of the other readouts that will come soon enough.

The next steps are to get the right panel converted, and then do some of the other aforementioned readouts. 

#### 11/23/24; 11:36:04 AM by DW

Got the hotlist working, on server and home.

Next up, get the main table working. It has two tabs, A-Z and Hotlist.

And to the right it has the place where you set the rating for shows and can view other people's ratings. 

It's the main display, everything else relates to it. 

#### 11/22/24; 12:17:22 PM by DW

We now have enough server functions implemented to start doing user interface stuff. 

Next session, let's view a list of the user's ratings.

#### 11/21/24; 10:46:36 AM by DW

Started.

