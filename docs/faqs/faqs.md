# Frequently Asked Questions Using Alma

## 1. How do I get access to Alma?
Follow the [Getting Started](https://nexus.icr.ac.uk/strategic-initiatives/sc/hpc/Pages/Getting-Started.aspx) tutorial on nexus.

## 2. Who do I ask for help?
Send an email to [Scientific Computing HelpDesk ](mailto:schelpdesk@icr.ac.uk)

## 2. Disk Space Problems

The quote is 10GB. If you are running out of space, you can check your disk usage with the following commands:
```bash
du -sh .conda/
du -sh /home/ralcraft/
ls -al
```

1. ERROR: Could not install packages due to an OSError: [Errno 122] Disk quota exceeded: '/path/to/python3.10'  
Does conda need to be cleaned out? Check for conda cache files:
```bash
du -sh .conda/
conda clean --all
```

