---
title:  "Database Solution"
date:   2024-02-04 11:01:03 +0000
categories: development decision
author: Jordan Tallon
layout: post
---

# Introduction
Naturally with user accounts, preferences, and dynamic content, UniFeed requires a Database to store changes and provide persistence for users between sessions.

# SQLite
This is the default database solution that Django comes equipped with. Its very lightweight, easy to set up, and low overhead for a regular student or hobbyist project. For UniFeed, where the goal is to have a production ready deployed app, there are a few issues with SQLite: 
1. **Poor Scalability** 
	* SQLite only allows one write operation at a time, which is a significant bottleneck for handling multiple users simultaneously accessing and modifying the database. This lack of concurrency limits the number of users the website can have, which is *very much* not ideal for a production-level app. There would be major performance and connectivity issues.
2. **Data Fragility** 
	* When working with SQLite, the database file is a single file on disk. In a collaborative environment using Git, this file can easily be overwritten if my partner or I  commit and push the SQLITE file, this can lead to the loss of data or conflicts between the data we are working with.
3. **Migration Challenges** 
	* If UniFeed needs to switch to a more robust database system in the future (i.e. the website traffic grows), migrating from SQLite could present challenges due to differences in SQL data types and database features.

# PostgreSQL
Due to the limitations of SQLite, I decided to migrate the project to PostgreSQL very early on in the development. Some of the reasons I settled on PostgreSQL are: 
1. **Strong Scalability** 
	* PostgreSQL supports multiple read and write operations simultaneously allowing it to handle concurrency much more effectively than SQLite. This on its own would be a crucial reason for picking PostgreSQL over SQLite. A production-level app  should expect a high volume of users to be accessing and updating the database.
2. **Data Integrity**
	* Unlike SQLite, which uses a single file, PostgreSQL operates as a server. This approach enhances data integrity and security. PostgreSQL also includes features like write-ahead logging (WAL) for fault tolerance, and comprehensive support for different types of data integrity constraints [Source](https://www.linkedin.com/pulse/postgresql-practical-guidefeatures-advantages-brainerhub-solutions#:~:text=PostgreSQL%20maintains%20data%20integrity%20by%20enforcing%20constraints%2C%20verifying%20data%20types%2C%20and%20enabling%20referential%20integrity%20through%20foreign%20vital%20regulations.%20In%20order%20to%20reduce%20the%20danger%20of%20data%20loss%2C%20it%20also%20offers%20write%2Dahead%20logging%20(WAL)%20and%20crash%20recovery%20procedures.).
3. **Easier Migration**
	* While migrating from SQLite to PostgreSQL requires some effort, especially in adjusting data types and queries. I avoided the headache thanks to Django's ORM layer and by migrating very early on in the project. If future migrations are required, PostgreSQL's widespread use and support make it a more future-proof choice as it is more likely to grow with evolving needs and standards, especially so since it's open source. 
4. **Better Compatibility with Real World Practices** 
	* PostgreSQL integrates well with professional development tools and practices. It supports complex transaction control, which is essential for high-quality, reliable software development and deployment.

# Conclusion
At this point, I have figured out the testing workflow (TDD), the CI/CD pipeline (Jenkins), and the database solution (PostgreSQL). My next goal is to figure out how I can bring all these development decisions together into a real live production environment. 

My end goal is to have it so we do not need to worry about the deployment of the app and can instead focus on the development up until 'the last minute' whilst following good Git branching practices and having these systems set up to automatically deploy a stable, scalable, future proof live server of the app. In my next step towards that goal, I will be looking at server architecture and solutions next.
