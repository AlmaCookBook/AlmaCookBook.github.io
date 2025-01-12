![GreenLogo](assets/rse_green.png "GreenLogo") 
# Sustainable Computing at the ICR
This is a collection of resources and advice on sustainable computing at the ICR. The advice comes from a collaboration between groups at the ICR engaged with the [GreenDiSC initiative for sustainable computing](https://www.software.ac.uk/GreenDiSC).  

- Breast Cancer Research - Data Science Team, contact **Santiago Madera**.  
- Research Software Engineering Team, contact **Rachel Alcraft**.  
- Data Science Team, contact **Jacob Househam**.  

For any help or questions email [scientific computing](mailto:schelpdesk@icr.ac.uk) and ask for the RSE team.  

## A quick summary of practical actions

### What can you do to be more sustainable?
1. **Measure your carbon footprint**  The application developed by the RSE team [green-alma](https://software-internal.icr.ac.uk/app/green-alma) is a web utility that measures the carbon cost of HPC jobs using the [green-algorithms library](https://www.green-algorithms.org/). It is available on the ICR network with the password and username available on request from the RSE team - contact the schelpdesk to ask. You can use this to measure your jobs and carbon use over a given period of time or job id.  
2. **Write efficient code** - The resources below will help you to write more efficient code and include internal training at the ICR. If your code is more efficient then it will cost less in terms of time, money and carbon.  
-- [Efficient R programming](https://csgillespie.github.io/efficientR/)  
-- [Profiling R](https://support.posit.co/hc/en-us/articles/218221837-Profiling-R-code-with-the-RStudio-IDE)  
-- [Top 10 tips to maximise Python Code Performance](https://www.geeksforgeeks.org/tips-to-maximize-your-python-code-performance/)  
-- [Profiling Python](https://realpython.com/python-profiling/)
3. **Test your code** - If your code fails then it will need to be corrected and re-run which will cost. Running tests on your code will help to reduce the number of times you need to run it. Using continuous integration built into GitHub or GitLab can help with this. Do consider that the tests themselves cost, so an efficient balance is needed for how often the tests are run.  
-- [testthat for R](https://testthat.r-lib.org/)  
-- [pytest for python](https://docs.pytest.org/en/stable/)  
Ask the RSE team for help on setting this up with continuous integration for your projects.  
4. **Reuse code** - Using open source community tested libraries will mean that you are using code that has been tested and is efficient. Internally built shared and tested libraries will also be an efficient use of time and resources.  
4. **Check your job efficiency on the HPC** - Your HPC jobs will use resources whether they are running optimally or idle. Check the efficiency of your jobs and make sure that they are running as efficiently as possible.  
Check efficiency of resources: `seff <jobid>`  
Check profile of a job: `scontrol show jobid -dd <jobid>`  
More info on job scheduling on [nexus](https://nexus.icr.ac.uk/strategic-initiatives/sc/hpc/userguides/Pages/Job-Scheduling.aspx)  
5. **Developer time is expensive** The time you spend optimising is a cost that should be considered. That time is also a carbon cost and a financial cost, and so you should consider the balance between the two. The cost of developer time is generally now greater than CPU time.  
6. **Your work is more important** - the carbon footprint of your code is important, but, as a researcher working on generally small isolated projects where code efficiency has a minimal impact on the wider world, the work you are doing is more important.
7. **Storage is a big win** - It may be that your biggest win in terms of carbon footprint is in storage, so consider how you can reduce the amount of data you are storing on an ongoing basis. Monitor and reduce continuously, and work with your team to embed this practice. The storage on RDS can be monitored on an interal dashboard:  [RDS dashboard](http://sjane.icr.ac.uk:3000/d/Higfsdhmz/rds-fileset-information?orgId=1) (VPN only) will show the data used and how it changes over time. Consider how you communicate with the test of your team and collaborators on data uses and storage as managing the data with an agreed plan will be less painful than trying to understand who needs what later down the line. In conversations with your colleagues and collaborators the message about the environmental impact of data storage can be highlighted.  
8. **Co-benefits** - Remember that there are usually multiple advantages to anything, and these can help with your motivation. For example a more efficient algorithm will run faster and cost less financially as well as in carbon terms.  The [green-alma](https://software-internal.icr.ac.uk/app/green-alma) app shows the financial cost as well as the carbon cost of your jobs.  
9. **Record what you are doing** - keep a record of what you are doing and how you are reducing your carbon footprint. This will help you to see the impact of your changes and to communicate this to others.  

### What can Scientific Computing do to be more sustainable?
1. The **efficiency of the data centres** we use has a large impact on the carbon footprint of our work.  
2. The ability to **time-schedule jobs** at periods of low energy use is important.  
Currently we do not have an automatic procedure for time scheduling jobs, nor do we have out of hours support to look after jobs if they were scheduled in this way. Here we need to balance the carbon footprint of our work with the practicalities of our work and this is in progress.  
3. The ability to **measure the carbon footprint of our work** is important.  
We have created the [green-alma](https://software-internal.icr.ac.uk/app/green-alma) app (VPN only) to monitor the carbon footprint of all jobs on Alma.  

---  

## Training and Resources

### Internal training at the ICR
These are courses run internally at the ICR, to sign up please visit the pages linked. If there are any courses without dates, please sign on to the waiting list, and also feel free to contact the RSE team for more information.  

- [IEMA Sustainability at the ICR](https://training.icr.ac.uk/coursed.php?course=1046)  
Audience: All staff. As a world-leading research centre, promoting health is at the heart of what we do. We embrace opportunities to advance social, economic and environmental health in our activities and make the ICR a more sustainable organisation. Every member of staff at the ICR plays a part in helping us to achieve these goals.
We’ve taken some major steps towards this already, like developing science-based targets to bring down our carbon emissions and using the UN Sustainable Development Goals as a framework for how we holistically approach sustainability.
We’re inviting all members of staff to participate in these sustainability training courses. They’ll provide opportunities to develop actions and initiatives to create change in your area of work, and to learn what the ICR is doing towards sustainability.
These courses lead to a professional certificate from IEMA, (the Institute of Environmental Management and Assessment). 
- [Software carpentries](https://training.icr.ac.uk) 
At the ICR, the RSE team, in collaboration with the Data Science team, deliver caprentry courses in Python, R, Git and Unix. These are designed to help you to be more efficient in your coding and to work more effectively with others, and by using best practices in coding you can reduce the carbon footprint of your work. Where we can, we use the carpentry materials from the [Software Carpentry](https://software-carpentry.org/) organisation.  
  -- [Python](https://training.icr.ac.uk/coursed.php?course=1200)  
  -- [R](https://training.icr.ac.uk/coursed.php?course=1218)  
  -- [Bash](https://training.icr.ac.uk/coursed.php?course=1215)  
  -- [Git](https://training.icr.ac.uk/coursed.php?course=1216)  
  -- [Image processing with python](https://training.icr.ac.uk/coursed.php?course=1217)  
  -- [Genomics](https://training.icr.ac.uk/coursed.php?course=1219)  
  -- Python optimisation - to be announced soon  
- Induction on computational sustainability - this is to be announced soon

### Resources
- [Green Software for practitioners](https://trainingportal.linuxfoundation.org/learn/course/green-software-for-practitioners-lfc131/introduction/welcome-to-lfc131?courseId=e924bf7e-306a-449e-8a05-b09ffe963faa?courseId=e924bf7e-306a-449e-8a05-b09ffe963faa) by the Linux Foundation, a comprehensive online training course with certifcate on succesful completetion. 
- [The Turing Way](https://book.the-turing-way.org/) a guide to sustainable software development practices.  
- [Reproducible research with python](https://coderefinery.github.io/reproducible-python/) from Code refinery. 
- Some slurm docs - [internal nexus doc](https://nexus.icr.ac.uk/strategic-initiatives/sc/hpc/userguides/Pages/Use-Cases.aspx)  and [additional FAQs](https://support.ceci-hpc.be/doc/_contents/SubmittingJobs/SlurmFAQ.html#q05-how-do-i-create-a-parallel-environment)  
- Recycling of our equipment by the [StoneGroup](https://www.stonegroup.co.uk/)  
- The [Slough Data Centre](https://virtusdatacentres.com/locations/uk/london/slough-campus)  

---  

## An overview of sustainable practices in computing

Much of the advice comes from the book `Building Green Software` by Anne Currie, Sarah Hsu and Sara Bergman ([O'Reilly](https://www.oreilly.com/library/view/building-green-software/9781098150617/) ).  

### Overview of goals
The lifecycle of a software product includes the energy used in the production of the hardware, the energy used in the software development, the energy used in the operation of the software, and the energy used in the disposal of the hardware. The goal of green software is to reduce the carbon footprint of software. 
Greenhouse gases (GHGs) are primarily carbon dioxide (CO2), methane, nitrous oxide, and fluorinated gases, with CO2 being the most problematic human emmission. To measure them in a single comparable unit we use carbon dioxide equivalent (CO2e), which we refer to as 'carbon'.

### Measuring carbon
The carbon footprint is at all points in the lifecycle of a software project, but here we are talking about what **you** can do. The disposal of hardware, the purchasing and maintanenace and upgarde of the servers, the choice of data centres the energy used in those data centres are a central decision. Engagement in these decisions is welcomed and encouraged, but here we are talking about the carbon footprint of the code you write and the jobs you run.  

To measure these you can keep an inventory of what you use, measure the carbon cost of your Alma jobs using green-alma and monitor your storage.  

Remember that this is a small piece of the puzzle, and everything you do contributes to the overall carbon footprint of the ICR and of your own activities. How you choose to travel to work is just as important as how you write your code. 

### Code efficiency
Developer time is a very expensive resource, and the efficiency of code has to be balanced against that time; the readbility of code for others; and the maintainability of the code. A pragmatic approach is recommended where awareness of the development quality is balanced against the competing demands of time and output. Reuse, testing, design and profiling are all important in this process. Please attend training courses at the ICR, read the resources and ask the RSE team for help. Use version control systems and don't be afraid that anyone is inspecting your code, this is a collective effort to try to reduce the carbon footprint of our work not a judgement on your coding skills.  

### Being carbon aware
Some of the big cloud providers are leading the way on carbon awareness, with Microsoft having implemented a carbon-aware Windows Update process. Carbon awareness is the understanding that not all energy is equivalent, and the time of day and the source of the energy are important. If you can move your jobs around data centres in different lcoations and at different times you have an opportunity to choose to use energy from renewable sources and at times when the energy is less carbon intensive. These different techniques are called demand shifting, and specfically time shifting and location shifting. 

However, we cannot control all these pieces as ICR users. We can control the jobs we choose to run, the efficiency of that code, the efficiency of the resource use, how well we monitor it and how we clean up the logs. We can put pressure on the ICR to adopt carbon-aware practices and to make the data centres more efficient. We can also choose to use the cloud providers that are more carbon aware. 

### Operational efficiency
Rightsizing is a buzzworrd, it means not overprovisioning resources, and is one of the cheapest green actions that we can take. In Alma terms it can mean we don't ask for more resources than we need. CLuster scheduling, as done on slurm, means that jobs can be sumbitted and run when tehre is space - this build an element of right-sizing in to our standard practices. Operational efficiency is improved but not having always-on servers but by using resources needed on demand, we do not use cloud services at the moment, but this could be a future way to improve operational efficiency by expanding and contracting resources as needed.

### Hardware efficiency
The HPC cluser is managed and upgraded on a schedule based on standard practices and the carbon cost of production and disposal need to be factored into that relatively short time period of equipement use. Extending the life of hardware is one of the best ways to reduce the carbon footprint, there is a balance to be struck between the cost of maintenance and the cost of disposal and production. 

### Machine learning and AI
Improvements in hardware have been an enabler for AI and machine learning, but the carbon cost of these models is high. The size and use of models is growing, and they carry ethical implications. Generally ML models are not time critical so they can benefit from demand shifting. Also, there are many pre-trained models available so using an existing model is more efficient than training a new one. If there is not a model for your needs, transfer learning will take an existing model and adapt it to your data.

### Measuring and monitoring
When talking about the carbon costs we often talk about scopes 1-3, where scope 1 is the direct emissions from the activities of the organisation, scope 2 is the indirect emissions from the energy used by the organisation, and scope 3 is the indirect emissions from the supply chain and the use of the products. 

There is little clarity for what this means for software creation. But, from your persepctive as a user of the HPC facilities, your main responsibity is for the costs of your compute time and storage on Alma and RDS. These can be measured and monitored with green-alma and sjane respectively. You can take action to monitor these to understand the approirateness of the costs. It is not necessarily the case that you are using an inappropriate amount of resources, but it is important to be aware of the costs and to be able to justify them. 

### Co-benefits
Eco-friendly practices have co-benefits that can help motivate you, and also sell the idea to colleagues and management.  

- Money saving - the cost of energy is a significant part of the cost of running a data centre, and so saving energy will save money.  
- Time saving - if the code is more efficient it will run faster and so save time.  
- Better code - the code will be more efficient and better tested so more reliable, easier to maintain and understand.  
- Better collaboration - the code will be more readable and so easier to work with others.  
- Better science - clearer code will be more reliable, reusable and reproducible.  

---  