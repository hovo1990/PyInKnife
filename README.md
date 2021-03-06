# PyInKnife

PYINTERAPH PIPELINE

Welcome to the pipeline for PSN analysis.

This pipeline use PyInteraph along with GROMACS to analyse MD trajectories.

PyInteraph is a software suite designed to identify intramolecular interactions

from protein ensembles and join them in a graph representation, which can be used to identify

pathways of structural communication.

Installation instructions and requirements: 

-PyInteraph. In order to build PyInteraph from source you will need

- Python 2.7

- a C/C++ compiler (e.g. GNU gcc)

- the Python header files (e.g. package python-dev in Debian-based Linux distributions)

- the numpy header files (which are usually pre-installed with the numpy package)

- Cython (optional)



Few open-source libraries are also required:


- MDAnalysis (< 0.8)

- numpy 

- scipy 

- matplotlib 

- networkx 

Follow the installation instructions found in the INSTALL file located in the PyInteraph software.


INSTRUCTIONS

The pipeline will create a folder called 'pyinteraph'. In order to run the script try first to run in the folder where the input files are stored, usually inside the 9-md folder.
NOTE: it will be necessary a local installation of Pyinteraph. You need to paste these lines in your .bashrc file and then enter again into the server:

# These first lines are prepared for a local installation of PyInteraph.
# Pyinteraph
. /usr/local/bin/virtualenvwrapper.sh . 
export WORKON_HOME=/usr/local/envs/
export PYINTERAPH=/usr/local/envs/pyinteraph/pyinteraph/ 
export PATH=$PATH:$PYINTERAPH

---------

How to run the script:
First, it is important to calculate the rmsd.vxg file. We used GROMACS to calculate the RMSD: 
## Example
g_rms -f traj_mol_ur_nojump.xtc -s sim_prot_Mg.tpr -o rmsd.xvg 



## Command line
PyInKnife.py -f traj_mol_ur_nojump.xtc -s sim.tpr -cut_off 5.5 -cut_off2 6 -ff charmm27 -k 3 -hb sc-sc -x rmsd.xvg -jackknife

In this case I am using two cut offs but it will work perfectly with one. 
The script contains the -h help flag.
The script will ask you first to select the protein group (i.e keep the protein group or the system of your interest) and save with q. This will create an index file (with the system that you selected before) necessary for the automation of the pipeline.
The average waiting time depends on the trajectory (nº of frames and the system size).

Once the script is running it will create two folders, one called 'contact' and the other 'h-bond' (For the h-bonds, depending on the selection it will calculate with one or another chain .Option:'all','mc-mc','mc-sc','sc-sc' and "no". If you  use "no" in -hb  it won't calculate the h-bonds). Then, if the -jackknife is defined, it will create folders called "resampling" where a 10% of the ensamble has been removed (the number of the folder indicates the positon of the 10% removed, so 0 its the first one and so on).  Inside the "resampling" directories there will be again two folders, 'contact' and 'h-bond' with the results for the new resampled trajectory. 

------
PLOTS
------
For the R figures (plots) some libraries are also required:
- ggplot2
- ggplot
- lattice


Run the bash script for all the plots for the hubs and connected components distribution: 

- pipeline_distribution_plots.sh 5.5 6 ../rmsd.xvg all sc-sc


1st argument: the 1st cut off

2nd argument: the 2nd cut off

3rd argument: path containing the rmsd.xvg file

4th argument: necessary for the calculation of the connected component and hubs distribution

The last argument is optional, it could be 'mc-mc','mc-sc' or 'sc-sc'.

This script will save the figures inside the contact and h-bond folder. 

Run the heatmap bash scripts to get the heatmap plots for hubs and connected components (cc): 

- heatmap_plots.sh 4.5 4 1 64 65 128

- heatmap_cc_plots.sh 4.5 4 1 64 65 128

1st argument: the 1st cut off

2nd argument: the number used for the heatmap pdf plot created

3rd argument: 1st residue used for the region selected to plot

4th argument 2nd resdieu used for the first frame selected

5th and 6th argument: the numbers used to select a second frame to plot the heatmap

This script needs to be run inside the pyinteraph folder, i.e the name of the pdf files created with the previous command line would be:

  "heatmap_4_1_64.pdf" and "heatmap_4_65_128.pdf" for the hubs. 
  
  "heatmap_cc_4_1_64.pdf" and "heatmap_cc_4_65_128.pdf" for the connected components. 



In case you find an error related to the heatmap,i.e using a cut off lower than 5, you wil need to follow this step: 
  - Run the bash script fix_pdb_error.sh inside the pyinteraph folder to create a new reference_cb.txt file that will be used for the heatmaps. Them it will be neccesary to change in the heatmap R scripts the name of the reference_cb.pdb to reference_cb.txt. 
    

# Publication

*An optimal distance cutoff for contact-based Protein Structure Networks using side chain center of masses*.
Juan Salamanca Viloria, Maria Francesca Allega, Matteo Lambrughi, Elena Papaleo

Computational Biology Laboratory, Danish Cancer Society Research Center, Strandboulevarden 49, 2100, Copenhagen, Denmark

Submitted for publication


