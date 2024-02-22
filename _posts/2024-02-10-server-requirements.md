---
title:  "Server Requirements and Deployment Solution"
date:   2024-02-10 14:05:03 +0000
categories: development decision
author: Jordan Tallon
layout: post
---


# Recap
So at this point in the project we have established:
1. We will use Django as our full stack web framework
2. The app will connect to a machine learning model created with the PyTorch framework.
3. UniFeed will use PostgreSQL as its database solution.
4. We will use Jenkins for our CI/CD pipeline

# Requirements
Following the recap, we can identify four different aspects that ideally, will run on different servers: 
1. The Django app.
2. The PostgreSQL database.
3. The Jenkins CI/CD pipeline.
4. The machine learning model.

We could run all four of these requirements on the same server, but this is typically avoided in production environments for the following reasons:
- **Resource Allocation**: Different components have unique system requirements. Isolating them allows for us to optimize the server for each requirements specific needs without affecting the others. For example, the machine learning model may require the use of a GPU for inference (or a stronger CPU), while databases typically need more memory.
- **Security Isolation**:  With different requirements on separate servers, If one of them were to be compromised, the others remain protected. This isolation also allows for greater access control and monitoring of each individual component.
- **Scalability**: Independent servers allow for easier scaling of individual components based on demand. For example, we might need to increase the storage or memory for the Database, or the GPU power for the ML. The isolated servers allow us to scale up each individual component without affecting one another.
- **Maintenance and Updates**: With separate servers, we can update, maintain, or restart a server without impacting the availability of others. This will let us keep downtime to the minimum in the live environment.
- **Failure Isolation**: Similar to the security isolation, If one server fails or experiences issues, it won't directly impact the others. This ensures that UniFeed has a greater reliability and continues to operate even if non crucial services fail (i.e. the political bias model could be down but users can still use the app for their news feeds)

# Choosing a Server Provider
After browsing the internet and looking at different service providers, I decided to go with DigitalOcean. The main reason I chose DigitalOcean is not just because they're highly established and popular but because they have an offer of $200 in credits (to be used in 60 days). The $200 and 60 day limitation or more than enough for the timeline and scope of this project. 

DigitalOcean provide relatively cheap PostgreSQL database hosting too, which will allow me to push the database handling off to their service to keep secure and up to date as we focus on developing our app.

In addition to the points above, DigitalOcean have credit and discounts for members of the GitHub Student Developer Pack, which I have applied for in case there's any other resources in it to aid with this project. 

