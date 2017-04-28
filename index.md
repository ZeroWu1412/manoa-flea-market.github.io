# Table of contents

* [About Manoa Flea Market](#about-manoa-flea-market)
* [User Guide](#user-guide)
* [Developer Guide](#developer-guide)
* [Application design](#application-design)
  * [Directory structure](#directory-structure)
  * [Import conventions](#import-conventions)
  * [Naming conventions](#naming-conventions)
  * [CSS](#css)
  * [Routing](#routing)
  * [Authentication](#authentication)
  * [Authorization](#authorization)
  * [Configuration](#configuration)
  * [Quality Assurance](#quality-assurance)
    * [ESLint](#eslint)
* [Development history](#development-history)
  * [Milestone 1](#milestone-1)
  * [Milestone 2](#milestone-2)
  * [Milestone 3](#milestone-3)

# About Manoa Flea Market

The Manoa Flea Market is a Meteor application that will offer UHM students a chance to sell or buy student-related goods and services. Similar to Craigslist, this application will: 

- Have students login with their UH credentials to access the system
- Connect buyers and sellers through UH credentials
- Items and services offered on this site will be geared specifically towards UHM students
- Users who violate the terms of use can be locked out of the system through their UH credentials

# User Guide

You can check out the [Manoa Flea Market Demo here](https://manoa-flea-market.meteorapp.com)::

(Note: It's not yet complete)

When you first come to our site, you will be on our landing page:

![](images/Screenshot (52).png)

In order to use the system, you must be logged in. Once you click to log in, you will be directed to login with your UH username and password:

![](images/LoginPage2.png)

Once logged in, users will be directed to the User Home Page: 

![](images/UserHomePage.png) 

Users will also be able to navigate through the top menu:

![](images/TopMenu.png)

However, users who have admin privileges will be directed to the Admin Home Page:

![](images/AdminHomePage.png) 

From either Home Page, you can list an item to sell, making sure you give a description and a photo:

![](images/AddListing.png)

If need be, you may also choose to edit your listing: 

![](images/EditListing.png)

All the items for sale from the users will then be available on a communal sell page:

![](images/market.png)

Users will also have profiles that can be viewed by other users that will list all items that the user is currently selling:

![](images/profile.png)

These profiles can be created or edited through the use of these pages: 

![](images/Screenshot (60).png)

![](images/Screenshot (59).png)

All the profiles will be accessible through a profiles list: 

![](images/Screenshot (58).png)

# Developer Guide

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

# Application Design

## Directory structure

The top-level directory structure contains:

```
app/        # holds the Meteor application sources
config/     # holds configuration files, such as settings.development.json
.gitignore  # don't commit IntelliJ project files, node_modules, and settings.production.json
```

## Import conventions

This system adheres to the Meteor 1.4 guideline of putting all application code in the imports/ directory, and using client/main.js and server/main.js to import the code appropriate for the client and server in an appropriate order.

This system accomplishes client and server-side importing in a different manner than most Meteor sample applications. In this system, every imports/ subdirectory containing any Javascript or HTML files has a top-level index.js file that is responsible for importing all files in its associated directory.   

Then, client/main.js and server/main.js are responsible for importing all the directories containing code they need. For example, here is the contents of client/main.js:

```
import '/imports/startup/client';
import '/imports/ui/layouts';
import '/imports/ui/pages';
import '/imports/ui/stylesheets/style.css';
import '/imports/ui/components/form-controls/';
```

Apart from the last line that imports style.css directly, the other lines all invoke the index.js file in the specified directory.

We use this approach to make it more simple to understand what code is loaded and in what order, and to simplify debugging when some code or templates do not appear to be loaded.  In our approach, there are only two places to look for top-level imports: the main.js files in client/ and server/, and the index.js files in import subdirectories. 

Note that this two-level import structure ensures that all code and templates are loaded, but does not ensure that the symbols needed in a given file are accessible.  So, for example, a symbol bound to a collection still needs to be imported into any file that references it. 
 
## Naming conventions

This system adopts the following naming conventions:

  * Files and directories are named in all lowercase, with words separated by hyphens. Example: accounts-config.js
  * "Global" Javascript variables (such as collections) are capitalized. Example: Profiles.
  * Templates representing pages are capitalized, with words separated by underscores. Example: Contact_Page. The files for this template are lower case, with hyphens rather than underscore. Example: contact-page.html, contact-page.js.
  * Routes to pages are named the same as their corresponding page. Example: Contact_Page.

## CSS

The application uses the [Semantic UI](http://semantic-ui.com/) CSS framework. To learn more about the Semantic UI theme integration with Meteor, see [Semantic-UI-Meteor](https://github.com/Semantic-Org/Semantic-UI-Meteor).

The Semantic UI theme files are located in [app/client/lib/semantic-ui](https://github.com/ics-software-engineering/meteor-application-template/tree/master/app/client/lib/semantic-ui) directory. Because they are located in the client/ directory and not the imports/ directory, they do not need to be explicitly imported to be loaded. (Meteor automatically loads all files into the client that are located in the client/ directory).

## Routing

For display and navigation among its four pages, the application uses [Flow Router](https://github.com/kadirahq/flow-router).

Routing is defined in [imports/startup/client/router.js](https://github.com/manoa-flea-market/manoa-flea-market/blob/master/app/imports/startup/client/router.js).

Manoa Flea Market defines the following routes:

  * The `/` route goes to the home page.
  * The `/profile-page` route goes to the public profile page.
  * The `/edit-profile-page/:_id` route goes to edit the profile page.
  * The `/add-profile-page` route goes to add the profile page.
  * The `/market-page` route goes to the market page.
  * The `/contact-page` route goes to the contact page.
  * The `/listing-page` route goes to the listing page.
  * The `/add-listing-page` route goes to the add listing page.
  * The `/edit-listing-page` route goes to the edit listing page.
  

## Authentication

For authentication, the application uses the University of Hawaii CAS test server, and follows the approach shown in [meteor-example-uh-cas](http://ics-software-engineering.github.io/meteor-example-uh-cas/).

When the application is run, the CAS configuration information must be present in a configuration file such as  [config/settings.development.json](https://github.com/ics-software-engineering/meteor-application-template/blob/master/config/settings.development.json). 

Anyone with a UH account can login and use BowFolio to create a portfolio.  A profile document is created for them if none already exists for that username.

## Configuration

The [config]https://github.com/manoa-flea-market/manoa-flea-market/tree/master/config) directory is intended to hold settings files.  The repository contains one file: [config/settings.development.json](https://github.com/manoa-flea-market/manoa-flea-market/blob/master/config/settings.development.json).

The [.gitignore](https://github.com/manoa-flea-market/manoa-flea-market/blob/master/.gitignore) file prevents a file named settings.production.json from being committed to the repository. So, if you are deploying the application, you can put settings in a file named settings.production.json and it will not be committed.

## Quality Assurance

### ESLint

BowFolios includes a [.eslintrc](https://github.com/manoa-flea-market/manoa-flea-market/blob/master/app/.eslintrc) file to define the coding style adhered to in this application. You can invoke ESLint from the command line as follows:

```
meteor npm run lint
```

ESLint should run without generating any errors.  

It's significantly easier to do development with ESLint integrated directly into your IDE (such as IntelliJ).

# Development History

The development process for the Manoa Flea market follows the ideas given in [Issue Driven Project Management](http://courses.ics.hawaii.edu/ics314s17/morea/project-management/reading-screencast-idpm.html). In a nutshell, development consists of a sequence of Milestones. Milestones consist of issues corresponding to 2-3 day tasks. GitHub projects are used to manage the processing of tasks during a milestone.  

The following sections document the development history of the Manoa Flea Market.

## Milestone 1: Mockup Development

Milestone 1 started on April 4, 2017 and completed April 12, 2017. 

The goal of Milestone 1 is to combine all of the groups idea developed during our own mockup of this project and combine these pages to create an application that is uniform in looks and has the links to the other pages working. In order to meet this goal, the pages will be developed as a Meteor app and FlowRouter will be implemented in order to get the routing to the other pages to work.

Mockups for the following pages were implemented during M1:

<img width="220px" height="140px" src="images/LandingPage.png"/>
<img width="220px" height="140px" src="images/LoginPage.png"/>
<img width="220px" height="140px" src="images/LoginPage2.png"/>
<img width="220px" height="140px" src="images/UserHomePage.png"/>
<img width="220px" height="140px" src="images/TopMenu.png"/>
<img width="220px" height="140px" src="images/AdminHomePage.png"/>
<img width="220px" height="140px" src="images/AddListing.png"/>
<img width="220px" height="140px" src="images/EditListing.png"/>
<img width="220px" height="140px" src="images/market.png"/>
<img width="220px" height="140px" src="images/profile.png"/>
<img width="220px" height="140px" src="images/EditProfile.png"/>

Milestone 1 was implemented as [Manoa Flea Market Github Milestone 1](https://github.com/manoa-flea-market/manoa-flea-market/milestone/1)::

![](images/Screenshot (44).png)

Milestone 1 consisted of ten issues, and progress was managed via the [BowFolio GitHub Project M1](https://github.com/manoa-flea-market/manoa-flea-market/projects/1)::

![](images/Screenshot (50).png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/Milestone1c.png)

## Milestone 2: Data Model Development

Milestone 2 started on April 13, 2017 and ended April 27, 2017.

The goal of Milestone 2 is to start working on our applications functionality by implementing the data model.

Milestone 2 was implemented as [Manoa Flea Market Github Milestone 2](https://github.com/manoa-flea-market/manoa-flea-market/milestone/2)::

![](images/Screenshot (45).png)

Milestone 2 currently has 9 issues and progress will be managed through [Manoa Flea Market Github Milestone 2](https://github.com/manoa-flea-market/manoa-flea-market/milestone/2)::

![](images/Screenshot (51).png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/Screenshot (48).png)

## Milestone 3

Milestone 3 was implemented as [Manoa Flea Market Github Milestone 3](https://github.com/manoa-flea-market/manoa-flea-market/milestone/3)::

![](images/Screenshot (46).png)

Milestone 3 currently has 5 issues and progress will be managed through [Manoa Flea Market Github Milestone 3](https://github.com/manoa-flea-market/manoa-flea-market/milestone/3)::

![](images/Screenshot (49).png)
