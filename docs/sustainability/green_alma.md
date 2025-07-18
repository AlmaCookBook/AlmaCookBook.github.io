# Calculating carbon footprint
**Green Alma** is a carbon footprint tracking tool developed at the **Institute of Cancer Research (ICR)**. This internal web application was built by the ICR Research Software Engineering team — Rachel Alcraft, Mira Sarkis, and Stacy Shcherbakova — with support from Santiago Madera and Fabian Groll.

Green Alma runs securely on the ICR VPN and requires either an on-site login or a VPN connection. You can access it [here](https://software-internal.icr.ac.uk/app/green-alma).

## Purpose
Green Alma is designed to help individuals and research groups at the ICR monitor the environmental impact of their use of the Alma cluster. It provides estimates of the carbon emissions associated with computational workloads.

The platform uses the **Green Algorithms 4 HPC** algorithm to calculate emissions based on job usage data from Alma. While still a work in progress, the tool supports ICR efforts to track and reduce their carbon footprint — and may also assist in applying for **Green DiSC** certification.

## Methodology
All carbon estimates are based on the methods described in:

> Lannelongue, L., Grealey, J., Inouye, M.  
> _Green Algorithms: Quantifying the Carbon Footprint of Computation_.  
> *Advanced Science* (2021), 2100707.  
> [https://doi.org/10.1002/advs.202100707](https://doi.org/10.1002/advs.202100707)

Keep in mind that calculations are based on this specific algorithm alone — results are indicative rather than precise.

## Running the Algorithm with Green Alma Notebooks
Green Alma Notebooks provide an alternative way for running carbon analysis interactively. You can explore your carbon footprint, run the Green Algorithms calculation on the server or locally, and visualise carbon emissions with plots using simple Jupyter Notebooks. You can find the link to the repository [here](https://github.com/ICR-RSE-Group/green-alma-notebooks)