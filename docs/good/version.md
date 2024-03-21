# Version Control

## Summary
Version control is a key part of software development, as it allows you to track changes to your code and collaborate with other developers. It also enables reproducibility and ensures that you can revert to previous versions of your code if needed. Github enables the creation of 

## At the ICR
- an internal gitlab server that you can use to store your code. This is accessible at [gitlab.icr.ac.uk](https://gitlab.icr.ac.uk/) (only available internally or on the VPN).
- A github enterprise account that you can use to store your code. This is accessible at [ICR github](https://github.com/instituteofcancerresearch). To become a member of this github please send a request to schelpdesk@icr.ac.uk.

## Key points
- Use descriptive commit messages
- Create branches for new features or bug fixes
- Merge changes regularly to avoid conflicts
- Use pull requests for code reviews

## DOI with Zenodo
- Zenodo is a service that allows you to create a DOI for your code, which can be used to cite your code in publications. You can create a DOI for your code by creating a release on Github and then linking your Github repository to Zenodo.

Please ask for help at any point! [RSE Group](mailto:schelpdesk@icr.ac.uk)
  
### Link to zenodo
1. Link the github account you use for ICR with zenodo: [Log on to zenodo](https://zenodo.org/login/?next=%2F) and choose to sign in with github
   - Make sure you grant access to the Institute of Cancer Research institution when you grant access via github
   - You can chose to link to your orcid or your github account
   - You will get an email to confirm your account

### Prepare a release
2. Edit the README.md of the repo with your service details.
3. Look at the [RSE Group](https://github.com/ICR-Services/RSE-Group) as a guide or contact the [RSE Group](mailto:schelpdesk@icr.ac.uk) for assistance.
4. Make a release by going to the release pages on the right of github, or from zenodo's [github page](https://zenodo.org/account/settings/github/). When you navigate to the repo page in zenodo take care to only give access to YOUR repo as you make see a list of institution repos

### Find your badge
5. Back on the Zenodo page a badge is automatically created, with a link to a zip of the code, like [![DOI](https://zenodo.org/badge/755024489.svg)](https://zenodo.org/doi/10.5281/zenodo.10638989)

However, be aware that this is a link to the latest release, so this will change with every release. It may be more appropriate to use the explicit most recent version DOI link so that if anyone uses it it refers to the exact version of the services at that time. To do this, find the release specific DOI button and click it. 

Take a copy of the DOI link from this badge for the pinned version (choose link, markdowen etc, I am using markdown in this example).

Finally, the dynamic DOI for this service is this:
[![DOI](https://zenodo.org/badge/755024489.svg)](https://zenodo.org/doi/10.5281/zenodo.10638989)  
```[![DOI](https://zenodo.org/badge/755024489.svg)](https://zenodo.org/doi/10.5281/zenodo.10638989)```

But at the time of writing the latest is v1.0.5: 
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10684363.svg)](https://doi.org/10.5281/zenodo.10684363)  
```[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.10684363.svg)](https://doi.org/10.5281/zenodo.10684363)```

The number will be the same while v1.0.5 is the most recent release.
