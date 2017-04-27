# Table of contents

* [About Manoa Flea Market](#about-manoa-flea-market)
* [User Guide](#user-guide)
* [Developer Guide](#developer-guide)
* [Application design](#application-design)
  * [Directory structure](#directory-structure)
  * [Import conventions](#import-conventions)
  * [Naming conventions](#naming-conventions)
  * [Data model](#data-model)
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

When you first come to our site, you will be on our landing page:

![](images/LandingPage.png)

In order to use the system, you must be logged in. Once you click to log in, you will be directed to login with your UH username and password:

![](images/LoginPage.png)

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

![](images/EditProfile.png)

All the profiles will be accessible through a profiles list: 



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

This structure separates configuration files (such as the settings files) in the config/ directory from the actual Meteor application in the app/ directory.

The app/ directory has this top-level structure:

```
client/
  lib/           # holds Semantic UI files.
  head.html      # the <head>
  main.js        # import all the client-side html and js files. 

imports/
  api/           # Define collection processing code (client + server side)
    base/
    interest/
    profile/
  startup/       # Define code to run when system starts up (client-only, server-only)
    client/        
    server/        
  ui/
    components/  # templates that appear inside a page template.
    layouts/     # Layouts contain common elements to all pages (i.e. menubar and footer)
    pages/       # Pages are navigated to by FlowRouter routes.
    stylesheets/ # CSS customizations, if any.

node_modules/    # managed by Meteor

private/
  database/      # holds the JSON file used to initialize the database on startup.

public/          
  images/        # holds static images for landing page and predefined sample users.
  
server/
   main.js       # import all the server-side js files.
```

## Import conventions

This system adheres to the Meteor 1.4 guideline of putting all application code in the imports/ directory, and using client/main.js and server/main.js to import the code appropriate for the client and server in an appropriate order.

This system accomplishes client and server-side importing in a different manner than most Meteor sample applications. In this system, every imports/ subdirectory containing any Javascript or HTML files has a top-level index.js file that is responsible for importing all files in its associated directory.   

Then, client/main.js and server/main.js are responsible for importing all the directories containing code they need. For example, here is the contents of client/main.js:

```
import '/imports/startup/client';
import '/imports/ui/components/form-controls';
import '/imports/ui/components/directory';
import '/imports/ui/components/user';
import '/imports/ui/components/landing';
import '/imports/ui/layouts/directory';
import '/imports/ui/layouts/landing';
import '/imports/ui/layouts/shared';
import '/imports/ui/layouts/user';
import '/imports/ui/pages/directory';
import '/imports/ui/pages/filter';
import '/imports/ui/pages/landing';
import '/imports/ui/pages/user';
import '/imports/api/base';
import '/imports/api/profile';
import '/imports/api/interest';
import '/imports/ui/stylesheets/style.css';
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


## Data model

The BowFolios data model is implemented by two Javascript classes: [ProfileCollection](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/profile/ProfileCollection.js) and [InterestCollection](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/interest/InterestCollection.js). Both of these classes encapsulate a MongoDB collection with the same name and export a single variable (Profiles and Interests)that provides access to that collection. 

Any part of the system that manipulates the BowFolios data model imports the Profiles or Interests variable, and invokes methods of that class to get or set data.

There are many common operations on MongoDB collections. To simplify the implementation, the ProfileCollection and InterestCollection classes inherit from the [BaseCollection](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/base/BaseCollection.js) class.

The [BaseUtilities](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/base/BaseUtilities.js) file contains functions that operate across both classes. 

Both ProfileCollection and InterestCollection have Mocha unit tests in [ProfileCollection.test.js](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/profile/ProfileCollection.test.js) and [InterestCollection.test.js](https://github.com/bowfolios/bowfolios/blob/master/app/imports/api/interest/InterestCollection.test.js).

You can run these tests using the following command:

```
meteor npm run test-watch
```

You can see the output by retrieving http://localhost:3100 in your browser. Here is an example run:

![](images/m2-mocha-tests.png)

## CSS

The application uses the [Semantic UI](http://semantic-ui.com/) CSS framework. To learn more about the Semantic UI theme integration with Meteor, see [Semantic-UI-Meteor](https://github.com/Semantic-Org/Semantic-UI-Meteor).

The Semantic UI theme files are located in [app/client/lib/semantic-ui](https://github.com/ics-software-engineering/meteor-application-template/tree/master/app/client/lib/semantic-ui) directory. Because they are located in the client/ directory and not the imports/ directory, they do not need to be explicitly imported to be loaded. (Meteor automatically loads all files into the client that are located in the client/ directory). 

Note that the user pages contain a menu fixed to the top of the page, and thus the body element needs to have padding attached to it.  However, the landing page does not have a menu, and thus no padding should be attached to the body element on that page. To accomplish this, the [router](https://github.com/bowfolios/bowfolios/blob/master/app/imports/startup/client/router.js) uses "triggers" to add an remove the appropriate classes from the body element when a page is visited and then left by the user. 

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

Milestone 1 was implemented as [Manoa Flea Market Github Milestone 1](https://github.com/manoa-flea-market/manoa-flea-market/projects/1)::

![](images/Milestone1.png)

Milestone 1 consisted of ten issues, and progress was managed via the [BowFolio GitHub Project M1](https://github.com/manoa-flea-market/manoa-flea-market/projects/1)::

![](images/Milestone1b.png)

Each issue was implemented in its own branch, and merged into master when completed:

![](images/Milestone1c.png)

Milestone 1 has been deployed through galaxy [Manoa Flea Market Demo](https://manoa-flea-market.meteorapp.com)::

## Milestone 2: Data Model Development

Milestone 2 started on April 13, 2017 and is currently in progress.

The goal of Milestone 2 is to start working on our applications functionality by implementing the data model.

Milestone 2 was implemented as [Manoa Flea Market Github Milestone 2](https://github.com/manoa-flea-market/manoa-flea-market/milestone/2)::

![](images/Milestone2a.png)

Milestone 2 currently has 3 issues and progress will be managed through [Manoa Flea Market Github Milestone 2](https://github.com/manoa-flea-market/manoa-flea-market/milestone/2)::

![](images/milestone2.png)

## Milestone 3

Milestone 3 was implemented as [Manoa Flea Market Github Milestone 3](https://github.com/manoa-flea-market/manoa-flea-market/milestone/3)::

Milestone 3 currently has - issues and progress will be managed through [Manoa Flea Market Github Milestone 3](https://github.com/manoa-flea-market/manoa-flea-market/milestone/3)::
