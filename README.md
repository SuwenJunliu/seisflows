  Update
  
   1. I have already update the checkboard test. I added some print statement to debug, so the output of terminal will be mess up. I should recheck all the modify I had made, and rearrange the output.
    
    
   2. I should find some methods to set up the checkboard. Nanqiao told me that we should convert the gll for binary file to ascii file, then add the perburbations on it, but I do not figure out the order of this ascii file. 
  
  
  Quick Fix by Junliu
  
  1. First, the checkboard example is stored in Google Drive. It is a little bit hard for those people who lived in China mainland to download all that files. I will try a way to get those files and upload to BaiduDisk(百度网盘) or other network disk.

  2. There are some mistakes in the main program and parameter file:
  
  - In folder "checkers"(the checkboard example folder)， the Par_file in "specfem2d-d745c542"， the parameter "f0_attenuation" should be changed into "ATTENUATION_f0_REFERENCE".
    
  - After change this parameter, the program still goes wrong. The error information should be found in "checkers/scratch/solver/#NUMBER#/solver.log", the #NUMBER# is the id of sources. SPECFEM2D would tell you the parameter you miss. Add them into the Par_file
    
  - Then the program will suck in “Generating synthetic data”， we could find this situation in terminal. It may cause by different version of SPECFEM2D. We could modify the seisflow code in "seisflows/seisflows/specfem2d.py". Search and subtitude the "unix.rename('single_p.su', 'single.su', src)" into "unix.rename('single_d.su', 'single.su', src)" will fix the problem. Notice that there are two line to change.
    
  - Then the program will suck in “Computing gradient” at the end of "task 25 of 25". The terminal will tell you KeyError: 'MINUSGRADIENT'. After checking the "seisflows/seisflows/workflow/inversion.py", I think the better way to fix it is change the “parameters.py” in "checkers" folder. Add "MINUSGRADIENT=1" of section "#WORKFLOW" in “parameters.py” , and add "MINUSGRADIENT：1" of section "#WORKFLOW" in "parameters.yaml"

My intuition told me there should be more problem during the running of program.
still updating 


SeisFlows
=========

SeisFlows is an open source seismic inversion package that provides

- A customizable waveform inversion workflow

- A framework for research in regional, global, and exploration seismology

SeisFlows differs from previous open-source software in emphasizing both flexibility and HPC portability.  Examples and usage information are available at [readthedocs.org](http://seisflows.readthedocs.org/en/latest/).  Probably the easiest way to learn more is to follow these checkerboard example [instructions](http://seisflows.readthedocs.io/en/latest/instructions_remote.html).

*My HPC account has been partially reactivated and the examples recovered. Two of the smaller examples are now being hosted from GoogleDrive*


- [2D acoustic checkerboard](https://drive.google.com/open?id=1ow3LTYEvNn55yGeIV56sqaW-wXC0MtYW)

- [3D elastic checkerboard](https://drive.google.com/open?id=1fm1pg0QzGW721BHN2tD6lb9FWRLMl6_z)





Design
------
With SeisFlows, the inversion task is abstracted into six components: `solver`, `system`, `nonlinear optimization`, `data preprocessing`, `postprocessing`, and `workflow`.  This design is informed by hands-on experience with different HPC environments and research applications. The source code is structured in a modular way based on these six categories, and users are offered various choices in each one.  To see the choices available for each category, simply browse the source code.  The inversion itself is executed by `seisflows/workflow/inversion.py`, which may be good place to start looking.

Wave simulations are performed by calling an external solver. The ability to interface with outside packages provides flexibility, and the choice of SPECFEM2D/3D/3D\_GLOBE as default options gives optional GPU acceleration and other useful capabilities. Setting up your own inversions, however, can be quite time consuming because it requires familiarity with SPECFEM2D/3D/3D\_GLOBES's idiosyncratic meshing procedure and binary file formats. Alternatively, some users have interfaced with yet other solver packages, but this can also be time consuming.


References
----------
If you find this package useful, please cite:

`Ryan Modrak, Dmitry Borisov, Matthieu Lefebvre, Jeroen Tromp; SeisFlows—Flexible waveform inversion software, Computers & Geosciences, Volume 115, June 2018, Pages 88-95, https://doi.org/10.1016/j.cageo.2018.02.004`

`Ryan Modrak, Jeroen Tromp; Seismic waveform inversion best practices: regional, global and exploration test cases, Geophysical Journal International, Volume 206, Issue 3, 1 September 2016, Pages 1864–1889, https://doi.org/10.1093/gji/ggw202`


See also
--------
The following extension packages are not currently documented, but may still give a sense for the type of research possible within the framework:

- https://github.com/rmodrak/seisflows-research
- https://github.com/rmodrak/seisflows-multiparameter
- https://github.com/rmodrak/seisflows-hpc



[![Build Status](https://travis-ci.org/rmodrak/seisflows.svg?branch=master)](https://travis-ci.org/rmodrak/seisflows)

