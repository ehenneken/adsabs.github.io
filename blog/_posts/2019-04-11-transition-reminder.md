---
layout: blog_post
title: "A Reminder About Transitioning to the New ADS"
author: "Kelly Lockhart"
position: "ADS"
category: blog
label: news
thumbnail: img/import_help_02.png
---

ADS Classic, [online since 1994](https://ui.adsabs.harvard.edu/about/history/), will be retired later this summer. (For more on the technical need for this upgrade, see [this previous blog post](http://adsabs.github.io/blog/technical).) Regular Classic users may have noticed some changes over the last few months, including an increasing number of warnings and pointers to the [new ADS](https://ui.adsabs.harvard.edu/), meant to encourage users to begin routinely using the new interface. As of the date of this post, Classic is still available, though its remaining time is limited. Now is an excellent time for the remaining Classic users to begin to familiarize themselves with the new system and to [contact us](mailto:adshelp@cfa.harvard.edu) if they run into problems. Below, we list a few important things for users to know as they begin to use the new ADS.

## To-Do Quick List
- [Create a user account](https://ui.adsabs.harvard.edu/user/account/register) in the new ADS
- Use the transfer tool to [transfer your libraries](https://adsabs.github.io/help/libraries/legacy-importing) to your new account
- Check your [application settings](https://ui.adsabs.harvard.edu/user/settings/application) to personalize your user experience in the new ADS
- Copy any [custom export formats](https://adsabs.github.io/help/actions/export) you had set up in Classic into your [application settings](https://ui.adsabs.harvard.edu/user/settings/application)
- Note: some Classic features, including myADS settings, are still in progress in the new ADS; more info on these will be announced in the future
- If you need more help, check our [Quick Start guides](https://adsabs.github.io/help/quickstart), come to our live Office Hours (live streams and recordings available on our [YouTube channel](https://www.youtube.com/watch?v=QVewqgjUCJI)), or [email us](mailto:adshelp@cfa.harvard.edu)

## User accounts
The new ADS and Classic have separate user databases, which means that user accounts will not automatically transfer from one system to the other. Users who want an account in the new ADS will need to [create a new one](https://ui.adsabs.harvard.edu/user/account/register). However, just as in Classic, users are not required to have an account to search for papers or perform most other actions. Accounts are only necessary for [saving papers to libraries](https://adsabs.github.io/help/libraries/creating-libraries), more easily accessing paywalled articles through an [institutional login](https://adsabs.github.io/help/userpreferences/library-servers), [personalizing the user experience](http://adsabs.github.io/blog/updates2#application-settings), and [accessing our API](https://github.com/adsabs/adsabs-dev-api). 

## Libraries
As with user accounts, libraries in Classic and the new ADS are stored in different databases and will not be transferred automatically. However, we do offer a tool to help import libraries from the old system into the new one. After creating a new user account and logging in, users can link their old account to their new one and [import their libraries](https://adsabs.github.io/help/libraries/legacy-importing) using the import tool. There are several things to note about the import tool: all libraries in the old account will be imported, though unwanted libraries may be deleted after import. Also, the tool imports libraries, it does not link them, so changes made to libraries in the old system after import will not propagate to the new one. Other guidelines for using this tool can be found on the [help page](https://adsabs.github.io/help/libraries/legacy-importing).

<div class="text-center">
    <img class="img-thumbnail" src="{{ site.baseurl }}/img/import_help_02.png" />
<em>Import Classic libraries into the new ADS</em>
</div>
<br>

## Application settings
Users with an account in the new ADS have access to settings that allow them to personalize their user experience. For example, users who have access to electronic articles through proxy servers hosted by their institutions can set their [library link servers](https://adsabs.github.io/help/userpreferences/library-servers), which enables a “My Institution” link on articles that require a login. Clicking the link redirects users to the login screen for the institutional library they previously selected; a successful login will then redirect the user to the article full text.

In addition to the library link server, users with accounts can set some [application settings](https://ui.adsabs.harvard.edu/user/settings/application). These include setting a default database(s) or collection(s) for searches, such as limiting results to astronomy only by default; automatically hiding the left and right sidebars on the search results page; and defining custom formats for exporting citations.

<div class="text-center">
    <img class="img-thumbnail" src="{{ site.baseurl }}/blog/images/blog_2019_02_13_application-settings.png" />
<em>Application settings</em>
</div>
<br>

## Classic features in progress
The ADS team is hard at work adding the last few features from Classic to the new ADS. Features still in progress are:

- myADS: Setup for new users and a transfer tool for existing users, similar to that used for libraries, will be made available before the myADS email service in Classic is turned off
- Perform actions on selected articles: Second order operations, such as retrieving the references of selected articles, are currently available for individual articles or for the entire set of results from a search. The ability to perform these operations for a subset of articles manually selected from the search results set is currently being added to the website

<div class="text-center">
    <img class="img-thumbnail" src="{{ site.baseurl }}/blog/images/blog_2019_04_11_perform-actions-on-selected.png" />
<em>Performing actions on selected articles in Classic</em>
</div>
<br>

- Classic-style relevance sorting: A new relevance sorting method, similar to the relevance sorting in Classic, is being developed. This new relevance sort ranks more highly articles that both better match search terms and are more popular (i.e., more highly cited and/or read)
- Library set operations: Set operations (union, intersection, difference, etc.) on libraries are currently available via the new ADS API, and an interface to these operations is being developed for the website. This extends the ability in Classic to effectively take the union or the difference between libraries
- Citation helper on a temporary set: The citation helper returns articles that are highly cited by a given set of articles but are not included in the original set. It is currently available within a library and is being considered for use with a given selected set of articles from a set of search results
- Recently read articles: Work is proceeding on allowing a user to see a list of their own recently read articles

## More help
To make it easier to start using the new ADS, the ADS team has prepared a range of supports to help users make the transition:

- The [Quick Start guides](https://adsabs.github.io/help/quickstart) are a set of simple written tutorials to walk users through basic tasks in the new ADS, such as finding a paper using an author name and the publication year
- The [FAQs](http://adsabs.github.io/help/faq/) and the [data and curation FAQs](http://adsabs.github.io/help/data_faq/) answer some frequently asked questions about the new system
- Users who prefer a [short video demo](http://adsabs.github.io/help/) to reading documentation can find that at the front of our help pages
- The ADS team will be hosting a biweekly series of talks throughout the spring, called ADS Office Hours, with the next scheduled for Tuesday, April 23rd. Talks start at 1pm US Eastern time, followed by time for questions or one-on-one demos until 3pm. Users can attend in person at the Center for Astrophysics in Cambridge, MA, USA, or watch the talks via live stream on [our YouTube channel](https://www.youtube.com/user/nasaads). Users wishing to join after the talk for the informal question and demo portion of the session can connect via [Google Hangouts](https://hangouts.google.com/hangouts/_/cfa.harvard.edu/adsofficehours). Recordings of the talks will also be available on our YouTube channel after the conclusion of the talk. The first talk, [an introduction to the new ADS](https://www.youtube.com/watch?v=QVewqgjUCJI), is now available.

And as always, we welcome questions and feedback from our users via [email](mailto:adshelp@cfa.harvard.edu).