# Table of contents

* [About Manoa Flea Market](#about-manoa-flea-market)
* [Installation](#installation)
* [Development history](#development-history)
  * [Milestone 1](#milestone-1)
  * [Milestone 2](#milestone-2)

# About Manoa Flea Market

The Manoa Flea Market is a Meteor application that will offer UHM students a chance to sell or buy student-related goods and services. Similar to Craigslist, this application will: 

- Have students login with their UH credentials to access the system
- Connect buyers and sellers through UH credentials
- Items and services offered on this site will be geared specifically towards UHM students
- Users who violate the terms of use can be locked out of the system through their UH credentials

When you first come to our site, you will be on our landing page which will be a combination of the two pages:

![](images/Screenshot (34).png)

In order to use the system, you must be logged in. Once you click to log in, you will be directed to login with your UH username and password:

![](images/Screenshot (26).png)

![](images/Screenshot (27).png)

Once logged in, users will be directed to the User Home Page: 

![](images/user-page.png)

Users will also be able to navigate through the top menu:

![](images/Screenshot (28).png)

However, users who have admin privileges will be directed to the Admin Home Page:

![](images/admin-page.png)

From either Home Page, you can list an item to sell, making sure you give a description and a photo:

![](images/Screenshot (36).png)

If need be, you may also choose to edit your listing: 

![](images/Screenshot (37).png)

All the items for sale from the users will then be available on a communal sell page:

![](images/Screenshot (35).png)

Users will also have profiles that can be viewed by other users that will list all items that the user is currently selling:

![](images/Screenshot (38).png)

These profiles can be created or edited through the use of these pages: 

![](images/Screenshot (39).png)

All the profiles will be accessible through a profiles list: 

![](images/Screenshot (39).png)

# Installation

First, install [Meteor](https://www.meteor.com/install).

Second, download a copy of The Manoa Flea Market, or clone it using git.
(As of right now, the application is still under construction)
  
Third, cd into the app/ directory and install libraries with:

```
$ meteor npm install
```

Fourth, run the system with:

```
$ meteor npm run start
```

If all goes well, the application will appear at [http://localhost:3000](http://localhost:3000).

# Development History

The development process for the Manoa Flea market follows the ideas given in [Issue Driven Project Management](http://courses.ics.hawaii.edu/ics314s17/morea/project-management/reading-screencast-idpm.html). In a nutshell, development consists of a sequence of Milestones. Milestones consist of issues corresponding to 2-3 day tasks. GitHub projects are used to manage the processing of tasks during a milestone.  

The following sections document the development history of the Manoa Flea Market.

## Milestone 1

Milestone 1 started on April 4, 2017 and continued until April 12, 2017. 

The goal of Milestone 1 is to combine all of the groups idea developed during our own mockup of this project and combine these pages to create an application that is uniform in looks and has the links to the other pages working. In order to meet this goal, the pages will be developed as a Meteor app and FlowRouter will be implemented in order to get the routing to the other pages to work.

So far, we created six mockup pages and created the landing and home page. We also set up the authentication so that only UH students may login.

Here were the mockup pages that were created and the authentication page:

<img width="200px" src="images/Screenshot (34).png"/>
<img width="200px" src="images/Screenshot (35).png"/>
<img width="200px" src="images/Screenshot (36).png"/>
<img width="200px" src="images/Screenshot (37).png"/>
<img width="200px" src="images/Screenshot (38).png"/>
<img width="200px" src="images/Screenshot (39).png"/>
<img width="200px" src="images/Screenshot (40).png"/>
<img width="200px" src="images/Screenshot (27).png"/>

Milestone 1 was implemented as [Manoa Flea Market Github Milestone 1](https://github.com/manoa-flea-market/manoa-flea-market/projects/1)::

<<<<<<< HEAD
Milestone 1 currently has 10 issues and progress will be managed through [Manoa Flea Market Github Milestone 1](https://github.com/manoa-flea-market/manoa-flea-market/projects/1)::
=======
![](images/Screenshot (30).png)

Milestone 1 currently has 10 issues and progress will be managed through [Manoa Flea Market Github Milestone 1](https://github.com/manoa-flea-market/manoa-flea-market/milestone/1)::
>>>>>>> origin/master

![](images/Screenshot (31).png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/Screenshot (41).png)

Milestone 1 has been deployed through galaxy [Manoa Flea Market Demo](https://manoa-flea-market.meteorapp.com)::

## Milestone 2

Milestone 2 started on April 13, 2017 and is currently in progress.

Now that we have an idea of the pages we want, we need to check to make sure everything works in the way that we want.  Thus, we will be need to implement the data model.

Milestone 2 was implemented as [Manoa Flea Market Github Milestone 2](https://github.com/manoa-flea-market/manoa-flea-market/milestone/2)::

![](images/Screenshot (33).png)

Milestone 2 currently has 3 issues and progress will be managed through [Manoa Flea Market Github Milestone 2](https://github.com/manoa-flea-market/manoa-flea-market/milestone/2)::

![](images/Screenshot (32).png)
