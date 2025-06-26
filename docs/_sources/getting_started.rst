Getting Started
===============

Welcome to the Getting Started guide!

Contents
--------

- :ref:`dl_field_control`
- :ref:`dl_field_path`
- :ref:`preparing_tutorial`


This tutorial will walk you through the basics of the DL_FIELD file formats and how to 
assign forcefield parameters to an example system. Before getting started, you should 
register to use the software `here <https://www.ccp5.ac.uk/dl_field-registration/>`__.

After downloading the source code, instructions on how to unpack and general guidance can be found
`here <https://dl-sdg.github.io/RESOURCES/dl_field_howto.html>`__. A brief summary of key DL_FIELD files 
is provided below. 

Our beginner-friendly tutorials assume you are working from the **dl_f_4.XX/** (e.g. **dl_f_4.12/**, version dependent)
directory that is created upon unpacking the source code. This directory contains *dl_field.control* (DL_FIELD 
CONTROL file) which controls how input molecular systems are parameterised, for example by defining the forcefield. 
A breakdown of the structure and available features of the control file is provided in :ref:`dl_field_control`.

The paths to the CONTROL file, forcefield library files and output files (amoung others) are located in the *dl_f_path* file. The
paths are defined *relative* to the DL_FIELD home directory (**dl_f_4.XX/**), which users should consider when making changes.

Upon compiling the source code, the DL_FIELD executable (*dl_field*) can be used to run the software from the command line with

.. code-block:: console

    $ ./dl_field


.. note::

    The *dl_f_path* file must be located in the same directory as the *dl_field* executable.


Successful structure conversions result in output files written to the directory specified in *dl_f_path* (**/output** by 
default). For example, the GROMACS *.mdp*, *.top* and *.gro* input files or DL_POLY/LAMMPS equivalents. For further details, consult
the `general guidance <https://dl-sdg.github.io/RESOURCES/dl_field_howto.html>`__ page, `reference manual <_static/dl_f_4.12_manual.pdf>`__, or 
the tutorials in this series.



.. _dl_field_control:

DL_FIELD control file
---------------------

The DL_FIELD CONTROL file contains a set of control options for all the operations DL_FIELD provides. Below is an example file
where we highlight key directives and things to consider in order to run DL_FIELD on a basic system. For more detailed 
information, visit this `page <https://dl-sdg.github.io/RESOURCES/TUTORIALS/dlf_5.html>`__. 

An example CONTROL file is shown below


.. code-block:: text
    :emphasize-lines: 2, 3, 4, 11

    Control file title. For DL_FIELD 4.10 
    1        * Construct DL_POLY output files
    gromacs     * Seconday output files (gromacs, chemshell or none).  
    opls2005  * Type of force field require (see list below for choices).
    kcal/mol  * Energy unit: kcal/mol, kJ/mol, eV, or K.
    normal  * Conversion criteria (strict, normal, loose)
    1        * Bond type (0=default, 1=harmonic , 2=Morse)
    1        * Angle type (0=default, 1=harmonic, 2=harmonic cos)
    none  * Include user-defined information. Put 'none' or a .udff filename
    1       * Verbosity mode: 1 = on, 0 = off     
    PE.xyz  * Configuration file.
    none   * Output file in PDB. Put 'none' if not needed.
    0 0.1 mol/dm^3  15.0 * Solution Maker: on/off, density, unit, cutoff)
    0        * Optimise FIELD output size, if possible? 1=yes  0=no
    2         * Atom display: 1 = DL_FIELD format. 2 = Standard format
    2         * Vdw display format: 1 = 12-6 format   2 = LJ format
    default * Epsilon mixing rule (organic FF only) : default, or 1 = geometric, 2 = arithmatic 
    default   * Sigma mixing rule (organic FF only) : default, or 1 = geometric, 2 = arithmatic 
    1        * Epsilon mixing rule (inorganic FF only) : 1 = geometric   2 = arithmatic
    2        * Sigma mixing rule (inorganic FF only) : 1 = geometric   2 = arithmatic 
    1    * Epsilon mixing rule (BETWEEN different FF) : 1 = geometric   2 = arithmatic
    2     * Sigma mixing rule (BETWEEN different FF): 1 = geometric 2 = arithmatic
    0         * Display additional info. for protein 1=Yes  0=No
    0         * Freeze atoms? 1 = Yes (see below)  0 = No
    0         * Tether atoms? 1 = Yes (see below)  0 = No
    0         * Constrain bonds? 1 = Yes (see below) 0 = No
    0         * Apply rigid body? 1 = Yes (see below) 0 = No
    1        * Periodic condition ? 0=no, other number = type of box (see below)
    120.0 0.0   0.0 * Cell vector a (x, y, z)
    0.0  120.0   0.0  * Cell vector b (x, y, z)
    0.0   0.0  120.0 * Cell vector c (x, y, z)
    default   * 1-4 scaling for coulombic (put default or x for scaling=x)
    default   * 1-4 scaling for vdw (put default or x for scaling=x)
    0  300.0  * Include velocity? 1=yes, 0=no and scaling temperature.
    1         * Position solute at origin? 1 = yes, 0=no 
    none  1.9 default * Solvate model? none or specify solvent (see below), distance criteria and FF.
    0  10.0   * Add counter ions? 1=yes, 0=no,  minimum distance from solute  
    0         * Not use 
    0         * Not use 
    0         * Not use 

    ************* DL_POLY control ******************
    0          * Run DL_POLY program
    DLPOLY.Z   * DL_POLY executable filename
    /home/usr/ * absolute path to DL_POLY program
    4          * Number of processors (mpirun)
    1          * MM calculation 1=on  0=off 
    0   1000   * Structural Relaxation level (0 - off, 1,2 or 3). Total timestep 
    8.0        * cutoff (vdw and electrostatic)
    100000       * Time limit for DL_POLY run (in seconds)

    ************* Gromacs control ******************
    0            * Run Gromacs
    gmx          * Gromacs executable filename
    /usr/bin/gmx * absolute path to Gromacs
    0            * MM single-point calc.

    ************* Lammps control ******************
    0            * Run Lammps
    lmp          * Lammps executable filename
    /usr/bin/ * absolute path to Lammps
    1            * MM single-point calc.
    

    ######################################################## 
    Atom state specification:  type  Molecular_Group   filter   [value]

    FREEZE     ORG1    cp 
    FREEZE     ORG2    CT

    RIGID A

    TETHER     CLY     st     100.0

    CONSTRAIN  SOD1    h-bond

    #########################################################

    Please do not remove those '####' lines.
    All select atom commands must be included within the two '####' lines

    Some remarks...
    ...
    ...
    ...



.. note::

    Comments in the control file are specified with the ``*`` character.


.. note::

    The order of the control settings is fixed and must not be removed nor rearranged.

Highlighted are key control options required to run a successful DL_FIELD operation. The first two specify the format of the 
output files DL_FIELD creates. The Molecular Dynamics packages DL_POLY, GROMACS and LAMMPS are currently supported, along 
with ChemShell for QM/MM. DL_FIELD always produces DL_POLY files (first line), whereas the second line specifies the format of 
secondary output files.

The third highlighted line provides the forcefield used for the parameter assignment. For a list of available forcefields, consult 
the reference manual.

The final line highlighted is how users can specify the path to their input structure. DL_field currently supports the PDB, xyz and mol2 
file formats. For more information on how to ensure DL_field can read your input structure properly, visit this
`page <https://dl-sdg.github.io/RESOURCES/TUTORIALS/dlf_12.html>`__.

The other operations in the CONTROL file may be changed according to specific user or forcefield requirements. The 
:doc:`tutorials <index>` in this series provide examples of this for various systems.


.. _dl_field_path:

DL_FIELD path file
------------------

As mentioned previously, the *dl_f_path* file contains the *relative* paths to the CONTROL file, output 
files and solvent templates, amoungst others (see `here <https://dl-sdg.github.io/RESOURCES/TUTORIALS/dlf_24.html>`__.
You must not use absolute paths. An example file is printed below:

.. code-block:: text

    # Directory paths for DL_FIELD version 4.10.
    # C W Yong, January 2024

    # paths
    library = lib/
    solvent = solvent/ 
    output  = output/

    # Control file 

    control = tutorial_confs/dl_field.control    

    ##########################
    # optional files
    # Look for CHARMM rtf, and or prm files
    # See Chapters 13.3 and 13.4
    # charmm_rtf = pentanoic_acid.rtf  OPLS2005 
    # charmm_prm = pentanoic_acid.prm  OPLS2005 
    # charmm_psf = example.psf charmm36_prot
    # charmm_pdb = example.pdb charmm36_prot


During the tutorials, change the CONTROL file path as required. 

.. _preparing_tutorial:

Setting up the tutorials
------------------------

To keep track of the tutorial material, create the following two directories in **dl_f_4.XX/**.

.. code-block:: console

    $ mkdir tutorial_confs
    $ mkdir tutorial_controls

Here we will place the input molecule configuration files and corresponding CONTROL file for each tutorial. An example
control file should already be located in **dl_f_4.XX/**, which we can copy to **tutorial_controls/**. 

Finally, we need to update *dl_f_path* with the new CONTROL file location. Comment out the old location and add the highlighted
line.


.. code-block:: text
    :emphasize-lines: 2

    # control = dl_field.control # old location
    control = tutorial_controls/dl_field.control # new location


The file name can be modified from the default *dl_field.control* as required. 