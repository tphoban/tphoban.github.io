---
title:  "Test Driven Development and Feature Branching"
date:   2024-01-15 18:01:03 +0000
categories: development decision
author: Jordan Tallon
layout: post
---

# Introduction

Before putting full attention on the Django project I decided to research and set up a comprehensive testing workflow. It would be great to have a structure to follow and establish a CI/CD pipeline before I begin development to ensure adherence to good principles and practice from early on in the development.

We decided to go with a Test Driven Development (TDD) approach when developing UniFeed. The reasons we decided to go with a TDD workflow for this project is to:
1. Force us out of our comfort zone and think more critically about our code.
2. Prioritize creating unit tests to ensure that they are not an afterthought.
3. Guarantee that our end project is comprehensively tested with a high code coverage.
4. Identify any potential issues or bugs early on in development.

To compliment the TDD workflow and future integration measures, we also decided to go with a feature branching strategy. The benefits to feature branching are:
1. Parallel collaboration - We can both work on two different features at the same time, in two isolated branches, allowing us to avoid interfering with each others work. 
2. Code isolation - We can experiment and make changes without affecting the main codebase, allowing for easy reversions or scrapping of any failed ventures.
3. Code reviewing - Each feature can be reviewed for issues or improvements. We can guarantee that there is sufficient code coverage with unit tests and that the code style is consistent across the project.

# Praxis

To put the TDD principles and feature branching into practice, the workflow we settled on is as follows:
1. When adding a new feature or fixing a bug, create a new branch inside the GitLab repo. The branch should be named in a way that makes the purpose obvious, for example: "feature-user-authentication" or "fix-user-auth-error".
2. After creating a new feature branch, the first step is to create a new Django app for the feature, for example `manage.py startapp accounts` when working on the "feature-user-authentication" branch. The app name should be descriptive and closely aligned to the functionality of the feature.
3. Django comes packed with a unit test library which we will take advantage of for creating `tests.py` files for each app, where the unit tests for the given feature should be implemented. We will use this to follow the TDD approach by creating tests prior to implementing the feature. 
4. We will develop each app on its respective branch until all app unit tests are passed. Afterwards, we will review the code for the new feature before creating a merge request back to the main branch for further integration (more details in a future blog). 

# Example of a UniFeed unit test

```python
class LoginTest(TestCase):
	def test_login_with_valid_user(self):
	    user_data = {
            'username': self.username,
            'password': self.password
        }
        # Login POST request using the user_data
        response = self.client.post(reverse('login'), user_data)
        # Assert that the user is logged in
        self.assertTrue('_auth_user_id' in self.client.session)
        # The login page redirected to the home page after successful login
        self.assertRedirects(response, reverse('home'))
```

# Conclusion
Now that we have established a workflow for how we are going to develop and test the Django project, I am going to move onto researching continuous integration and deployment. Since we are using feature branching, CI/CD will perfectly compliment our workflow and ensure that we are following good unit testing practices, regression testing is being performed, and that only stable changes are pushed to the live production server. 