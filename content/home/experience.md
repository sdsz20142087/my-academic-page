---
# An instance of the Experience widget.
# Documentation: https://wowchemy.com/docs/page-builder/
widget: experience

# This file represents a page section.
headless: true

# Order that this section appears on the page.
weight: 40

title: Internship
subtitle:

# Date format for experience
#   Refer to https://wowchemy.com/docs/customization/#date-format
date_format: Jan 2006

# Experiences.
#   Add/remove as many `experience` items below as you like.
#   Required fields are `title`, `company`, and `date_start`.
#   Leave `date_end` empty if it's your current employer.
#   Begin multi-line descriptions with YAML's `|2-` multi-line prefix.
experience:
  - title: NLP Algorithm Engineer
    company: Tian Xue Wang
    company_url: ''
    company_logo: org-q
    location: Beijing
    date_start: '2021-03-25'
    date_end: '2021-05-25'
    description: |2-
        Responsibilities include:
        
        * Implemented and upgraded the algorithms to an automatic scoring system for spoken English, which is integrated into the app, covering both PC and mobile.
        * Scored Q&A questions using a machine learning model solution to construct text similarity features between the student's recognition text and multiple reference answers, and then put them into the LGB model together with speech features to conduct regression, boosting accuracy from 75% to 85%.
        * Implemented features and models based on over 30,000 data according to region using Python, and deployed them into the Polly engine using C++.
        * Built a back-end service with Python and Flask for the mock exam, serving over 200,000 users.
        
  - title: Data Analyst
    company: Kuaishou
    company_url: ''
    company_logo: org-a
    location: Beijing
    date_start: '2020-11-05'
    date_end: '2021-02-05'
    description: |2-
        Responsibilities include:
        
        * Set up the department data center from 0 to 1, drove and led data reform and innovation to better meet the internal data needs of the department while applying analytical capabilities to guide the business.
        * Undertook the initial work of data development and met the data requirements of the department: utilized SQL to retrieve data from Hive and completed over 1,500 SQL queries, wrote the SQL template, and created the corresponding visual Kanban to support the unified and efficient data queries and statistics, and standardized data caliber.
        * Provided the strategies support for recommending high-quality works to the operation side, and developed an appropriate public release strategy based on the characteristics of the published works of different secondary vertical classes.
        * Calculated resource allocation for different queues and periods around the internal resource pool of the department, optimized the code and configured a fitted engine.
        * Conducted the quantitative analysis (Gini-Simpson index and Shannon-Wiener index) for the ecological diversity of the primary vertical classes to prevent the recommendation system from being good at working out an optimal local solution that resulted in the ‘Matthew effect’ of distribution.

design:
  columns: '2'
---
