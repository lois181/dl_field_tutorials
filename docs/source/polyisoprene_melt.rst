Tutorial for an OPLS/2005 polyisoprene chain
============================================

Contents
--------

- :ref:`poly_control`
- :ref:`dl_poly`
- :ref:`gromacs`
- :ref:`lammps`


.. note::
    The following tutorial assumes you have compiled DL_field and are working from the **dl_f_4.XX** directory. Please visit
    the :doc:`getting started <getting_started>` page before starting.

This tutorial provides a walk-through for users to assign parameters to an all-atom 32mer cis-(1,4)-polyisoprene chain using the
OPLS/2005 forcefield. To begin, download the following `polyisoprene.xyz <_static/polyisoprene.xyz>`__ file, which contains the 
polymer structure, to the directory **dl_f_4.XX/tutorial_confs/**. 

.. _poly_control:

Polyisoprene CONTROL file
-------------------------

We will now create an appropriate CONTROL file and use it to generate DL_POLY and 
GROMACS input files for our system. We include some helpful notes for new users. 

The CONTROL file for this system is printed below

.. code-block:: text

    Control file for a cis-PI chain
    1        * Construct DL_POLY output files
    gromacs        * Seconday output files (gromacs, chemshell or none). 
    opls2005  * Type of force field require (see list below for choices).
    kJ/mol  * Energy unit: kcal/mol, kJ/mol, eV, or K.
    normal  * Conversion criteria (strict, normal, loose)
    1        * Bond type (0=default, 1=harmonic , 2=Morse)
    1        * Angle type (0=default, 1=harmonic, 2=harmonic cos)
    none  * Include user-defined information. Put 'none' or a .udff filename
    1       * Verbosity mode: 1 = on, 0 = off
    tutorial_confs/PI.xyz  * Configuration file.
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
    0        * Periodic condition ? 0=no, other number = type of box (see below)
    60.0 0.0   0.0 * Cell vector a (x, y, z)
    0.0  60.0   0.0  * Cell vector b (x, y, z)
    0.0   0.0  60.0 * Cell vector c (x, y, z)
    default   * 1-4 scaling for coulombic (put default or x for scaling=x)
    default   * 1-4 scaling for vdw (put default or x for scaling=x)
    0  300.0  * Include velocity? 1=yes, 0=no and scaling temperature.
    1         * Position solute at origin? 1 = yes, 0=no
    none  1.9 * Solvate model? none or specify solvent (see below) and distance criteria.
    0  10.0   * Add counter ions? 1=yes, 0=no,  minimum distance from solute
    0        * MM energy calculation. 1=Yes, 0=No
    20.0     * Cut off for electrostatic energy calculation (angstrom)
    20.0      * Cut off for vdw energy calculation (angstrom)

    ************* DL_POLY control ******************
    0          * Run DL_POLY program
    DLPOLY_AMD.Z   * DL_POLY executable filename
    /home/user_pc/DL_POLY/  * absolute path to DL_POLY program
    8          * Number of processors (mpirun)
    0          * MM calculation 1=on  0=off
    3   5000   * Structural Relaxation level (0 - off, 1,2 or 3). Total timestep
    8.0        * cutoff (vdw and electrostatic)
    100000       * Time limit for DL_POLY run (in seconds)

    ************* Gromacs control ******************
    0            * Run Gromacs
    gmx          * Gromacs executable filename
    /usr/bin/ * absolute path to Gromacs
    1            * MM single-point calc.


    ########################################################
    Atom state specification:  type  Molecular_Group   filter   [value]

    #########################################################

    Please do not remove those '####' lines.
    All select atom commands must be included within the two '####' lines

    Some remarks...
    ...
    ...
    ...

This instructs DL_FIELD to produce GROMACS and DL_POLY input files as outputs, controlled by the first 
two lines of the CONTROL file: 


.. code-block:: text

    1        * Construct DL_POLY output files
    gromacs        * Seconday output files (gromacs, lammps chemshell or none). 


Successfully running the program (from the **dl_f_4.XX/** home directory) with the command 

.. code-block:: console

    $ ./dl_field

will produce the following files in the **output/** directory:

.. code-block:: none

    readme.txt*
    dl_field.output
    dl_poly.CONFIG
    dl_poly.CONTROL
    dl_poly.FIELD
    dlf_notation.output
    gromacs.gro
    gromacs.mdp
    gromacs.top
    gromacs1.itp

From which we can see a set of standard output files provided for DL_POLY and GROMACS. For helpful notes 
regarding these output files, consult the following sections in this tutorial.

DL_FIELD also produces up to two .output files. The dlf_notation.output file describes the 
chemical identity of every atom in the system. dl_field.output contains information on the 
conversion process of the user's specific input files. For more information, consult the reference manual. 

.. _dl_poly:

For DL_POLY Users & General Use
-------------------------------

DL_FIELD produces the CONFIG, CONTROL and FIELD files required to run a DL_POLY simulation. The following 
section contains useful information, which DL_POLY users may find helpful.

The line:

.. code-block:: text

    kJ/mol  * Energy unit: kcal/mol, kJ/mol, eV, or K.

controls the energy units of the FIELD file. For FFs using the LJ potential, parameters can either be 
expressed in 12-6 format, or directly with their σ, ε values. The format can be controlled via the line

.. code-block:: text 

    2         * Vdw display format: 1 = 12-6 format   2 = LJ format

There are some options in the CONTROL file which are FF dependent, and so it is recommended they are kept 
to their default settings. For example, bond types, angle types and 1-4 scaling factors for vdW and 
coulombic interactions:

.. code-block:: text 

    0        * Bond type (0=default, 1=harmonic , 2=Morse)
    0        * Angle type (0=default, 1=harmonic, 2=harmonic cos)
    ...
    default   * 1-4 scaling for coulombic (put default or x for scaling=x)
    default   * 1-4 scaling for vdw (put default or x for scaling=x)   

In the case of the bond type and angle type options, they will default to 0 unless the DREIDING potential 
scheme is selected. 

The CONTROL file produced by DL_FIELD is generic, and so should be adjusted manually for your requirements. 

.. _gromacs:

For GROMACS Users
-----------------

DL_FIELD produces .mdp, .top, .gro and .itp files required to run a GROMACS simulation. Unlike DL_POLY, 
GROMACS only supports energy units of kJ/mol, therefore the units option

.. code-block:: text

    kJ/mol  * Energy unit: kcal/mol, kJ/mol, eV, or K.

is produces the same .top and .itp files regardless of its setting.

The .gro file format is strict. In C format:

.. code-block:: text

    "%5d%-5s%5s%5d%8.3f%8.3f%8.3f%8.4f%8.4f%8.4f"

DL_FIELD does not check if this is violated, for example if the atom labels are more than 5 characters 
in length. 

Finally, the .mdp file produced is generic and should be adjusted based on user requirements. For instance, 
GROMACS offers automatic bond constraints via the *constraints* flag in the .mdp file. It is also possible 
to freeze atoms via *freeze-grps*. Users wishing to use these features should manually edit the DL_FIELD 
.mdp file or provide their own. 

.. _lammps:

For LAMMPS Users
----------------

DL_FIELD will produce the LAMMPS input file *lammps.in* and one or more data files with the structures and 
topologies, *lammpsX.data* (X = 1, 2, 3...), depending on the number of molecular species. The *lammps.in* file 
follows a pre-defined set of rules to ensure consistent FF definitions are maintained, and so is generic and 
should be modified according to user requirements. See the reference manual for an explanation of the settings chosen.

There are also some restirctions for LAMMPS users applicable to DL_FIELD ver. 4.12, printed here for consideration. These 
may be removed in future versions of the software.

1. Only applicable to xyz and PDB input configurations. 

2. Multiple potential setting is not available to LAMMPS.

3. FF schemes, such as CHARMM for proteins, that make use of CMAP feature, is not 
available to LAMMPS.

4. The current version (4.12) does not support the following FF schemes: MISC_FF 
and INORGANIC FFs. DL_FIELD will return an error when one of these schemes 
are selected. 

5. Imposing FREEZE and TETHER states for atoms are not available to LAMMPS. 




 



