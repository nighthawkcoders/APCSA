---
toc: true
layout: post
description: 
categories: [tri1]
title: Overview Sprint - Java, PBL, and Tools
---

# AP CSA COURSE OUTLINE
- Methodology.  Project Based Learning (PBL)
- Guideline. The 10 Units of AP Computer Science A
- Key Learnings for 2022-2023
    - Collaboratively Build, Deploy and Host Web Pages
    - Programming, Algorithms, Databases, and APIs
    - Team and Individual Project will highlight all 10 units, either academically or in interest fashion
- Establish "Real World” Scrum Team projects
    - Preference is to establish projects with a Customer/Sponsor
    - Projects will be tracked using GitHub README (Time Box), Issues and Projects tools
- Create "GitHub Pages" Individual postings and notebooks
    - Build individual GitHub Pages Web Site to _post Notes and Learnings
- AP testing in May


## "Real World" Projects
You, your pair, and your Scrum Team will collaboratively Build, deploy and host Web Site.  This Web project contains Technicals learned throughout the Trimester.  This will be done in increments.  Each Monday an assignment will be given and the previous will be under Live Review.

- Feature Considerations for Project
    - Project Requirements
        - A fun zone, unique idea(s)
        - Educational zones that capture every key learning objective
        - A location that highlights individuals, jobs, and technical capabilities
    - Project Technicals
        - Managing PBL Requirement for Project (Issues, Scrum Board)
        - Use of "Java Classes" in backend.
        - Use of "Thymeleaf" in frontend
        - Data Structure exchange between frontend and backend

- Individual GitHub Pages considerations for Java Units 1-10
    - Each week we will be studying Java in Tech Talk and/or Test Prep
    - GH Pages should be used like notes to capture PBL and Java learnings each week!
    - Jupyter Notebooks using IJava and Markdown will be required to show Java learnings, Teacher likes running code!


### Establish PBL and Comp Sci attitude
- Attitude.  An Agile mindset is not knowing the answer, making lots of mistakes and performing iteration.  A successful grade is dependent on making mistakes, research, and iteration.
- Suggestion. Please bring a personal laptop to class daily.  The Internet and AP Classroom web site will  be considered text book.  
- Requirements. Everything will be distributed electronically.  All code you develop will be delivered to the Cloud. 
- Grading guidelines. There will be key technical objectives which will require producing tangibles. Essentially, it is impossible to be late with work if you work consistently attend class, work in class and turn in progressive tangibles each week.  Thus, failure to have work will result in a 10% deduction.  Additinally, late work must be defended in office hours.  
    - High "A" is very tough to achieve, something beyond the tangible requirements.  Something that exhibits an unforced desire to Code/Code/Code.
    - Low "A" is consistency in producing tangibles toward Team Project and Individual GH Page according to Issues and Scrum Board plans.   Plans must be consistent with key objectives and technicals.
    - "B" is having flaws in consistency or tangible shortcomings, but mostly on track.  A flaw would be mostly working code.
    - "C" is a lack consistent effort, lacking tangibles. Lack of producing working Code.
    - Below "C" is composed of Slash/Slash/Slash offenses. Lack of attendance, disruptive behaviors, doing work from other classes during class time, paper visible in class, and turning in tangbiles that you can't represent in live review.


## Assignments and Work
- Assignments mostly Due either Friday or Monday at the start of class (canvas is offical record for points)
    - Live Grading that is complemented by Self/Crossover assesments 
    - Always prepare Review Ticket (GH Issue) using canvas as guide
- Trimester start with 5 point seed (highly engaged +, distracted -)
- Less than 100 points per trimester, approximately 30% of points in last few weeks of Trimester.


### PBL Sprint 0 / Week 0
Learning outcome.  Getting adapted to the Agile mindset used in Computer Science.  Additionally, getting introduced to GitHub and VS Code.   Building a Java/Spring Web Server on your localhost.  Showing personal and running Fastpages/GitHub Pages.
- Wednesday - "Introduction Sprint".  Pick pair share partner, Pick crossover pair, Establish team of four.  Based off of modulo mathematics (remainder) there can only be 3 teams of 5 maximum.   Spend some time talking and getting to know each other.  Consider key roles in Project Teams as Scrum Master (Issues, Scrum Board), DevOps (GitHub, Deploy, POM dependencies), Frontend Developer (HTML, Thymeleaf, Javascript), Backend Developer (Spring, Java)
- Thursday - Review "Tools and Equipment" and "Anatomy of Java".  Bringing your laptop.  Setup GitHub and Tools and push code to your Repo.
- Friday - Review "Roles, Issues, and Scrum Board". Pair Share coding. Spend 30 minutes at keyboard installation and performing Jupyter coding, while Pair Share observes and consults.  Next 30 minutes Pairs reverse roles.


### Equipment, accounts and tools
- A laptop, that you bring to class every day with the Development Tools installed on it.
- GitHub Account, VS Code will be used to push/pull changes. GitHub is where we store and share code in the cloud, think of Google Docs but for Code.
- GitHub Pages will be used to host your personal web site, notes, and experiences.
- Jupyter Notebooks will be used in conjunction with GitHub Pages to build running Java Code in your Technical Notes.
- Slack Account, install App on Laptop, get used to reading announcements. Slack is the tool will use for messaging, we have been averaging 35,000 message per class.
- Java is the key language you will be using in this class.  Spring, Thymeleaf, HTML, CSS, JavaScript are the key supporting technicals you will be using to enhance your learning of Java. 
- VS Code is the code editor we will be using in this class.  VS Code is more than and editor, it is called and Interactive Development Environment (IDE). 
- AWS Account for deployment, this will be provided by Teacher.  AWS Cloud Commputing and Servers will be used to Deploy and Support projects.


### GitHub, VS Code Installation and Setup (Allie to verify and add stuff to this outline)
    - VS Code https://code.visualstudio.com/docs/languages/java
        - Install the Coding Pack for Java
        - Install Extension Pack
        - Spring Boot Extension Pack
    - Start Java GitHub Repo from Template https://github.com/nighthawkcoders/spring_portfolio/generate
        - VS Code Clone new GitHub Project 
        - Run/Play your project from Main.java
        - In Chrome or browser, run localhost:8080
        - Push a minor change and Verifiy on GitHub
    - Start Pages/Fastpages GitHub Repo from Template https://github.com/nighthawkcoders/APCSA/generate
        - Follow FastPage Repo Setup, skip Step #1, start Step #2 Pull Request (PR) https://github.com/fastai/fastpages#setup-instructions
        - Verify GitHub pages is running
        - Install Jupyter Kernel Spec for Java (IJava) on your machine https://github.com/SpencerPark/IJava#install-pre-built-binary
        - VS Code Clone new GitHub Project
        - Run/Play a Jupyter notebook _notebooks/2022-06-14-anatomy.ipynb
        - Push a minor "markdown" change and Verify on GitHub
        - Verify "markdown" change on GitHub Pages
        - Create a new Jupyter notebook and publish to _notebooks directory https://code.visualstudio.com/docs/datascience/jupyter-notebooks


### Extra Credit (Seed +)
- Find the flaw Java/Spring project.  Java Spring project uses Vanta Birds.  When present on screen, the last element in drop down menu can't be activated.  This is often referred to as a 'layering problem'.
- Find the flaw in Java/Spring POM file dependencies.  Switching to using Java 17 SDK an changing to Java 17 in POM file causes a compiler error.  Try to negotiate the POM file incompatibility error.
- Find the flaw in FastPage using VSC and Jupyter.  In my environment when editing Jupyter Notebook (ipynb file), I often receive many Output errors.  They seem benign, but are annoying.


# Posts and Tech Talks
- Overview Sprint - Java, PBL, and Tools
- TPT Unit 00 Anatomy of Java
- TT 1.0.0 Tools and Equipment 
- TT 1.0.1 Roles, Issues, and Scrum Board 