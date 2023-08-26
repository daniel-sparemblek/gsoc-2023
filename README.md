# GSOC 2023 - CERN

This is a short explanation of a project that took place during summer 2023. I was part of Google Summer of Code with CERN as my organization. The title of the project was: **Extend and improve the CERNBox Notifications platform**. The following document will describe the task I was given, how it was solved, and what can be done in the future.


# Task

The task given was a medium 175 hour long project regarding CERNBox. CERNBox is a sync-and-share collaborative cloud storage solution developed at CERN, used by more than 37K users and storing over 18PB of data.

My task was to improve the notifications system that is in place by implementing a way for users to disable notifications for certain criteria, e.g. disable notifications for files shared in folder XYZ, disable notifications for all files shared by user XYZ.
Firstly, a framework was needed. A UI that is simple to use for the users that is connected to the backend and fetches certain user preferences from the database. With this framework, further additions to the notification settings could bee easily made. The main task was to develop this framework so that new settings could be easily created and managed. A simple example was used to create the aforementied framework - disable all notifications. This would prove to be a useful tool to test how well the pipeline from user interaction, through frontend towards the backend and database functioned.

### Frontend - Typescript (Vue JS)
The current user menu was extended with a simple button that opens a modal with settings. The default or user configured settings are loaded into the modal during the initial page loading. The following is a screenshot of the newly added menu, alongside the opened modal.

The implementation of the frontend was trivial, adding a new modal alongside functions for storing and fetching the notifications settings. This was done using VueJS 3.0's Composition API. The store function makes an HTTP PATCH request to the backend while the fetch function makes a GET request to fetch the XML data.

### Backend - Go (MySQL)

In the backend, the task was not trivial. The first goal was to identify the correct pipeline that will be used for storing and fetching notification preferences from the database. An already existing table for preferences was used. Next up - creating the functions that recieve the aforementioned HTTP requests. These then *forward* the request by querying the database for the needed values. Multiple services, handlers and manager files need to be changed in order to keep the well-managed architectural pattern set in place by CERN's codebase.

## Remarks

Probably the most difficult part was making sure that I have access to enough features of the CERBOX system while still maintaing security by not allowing me direct access to the dev version. This was done through a Docker Compose tool is a tool for defining and running multi-container Docker applications. Making sure that a database works and users are able to recieve notifications within this environment proved to be a challenge.


# Future works

The main work that can be done with this framework in place is further additions to the notification settings. It should be simple to add notifications for certain features, e.g. MIME types, and folder names. I expect a future GSoC student to continue the work here that laid the baseline in creating a working framework for CERNBOX notification settings.
