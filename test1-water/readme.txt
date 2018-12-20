See https://barnett.science/tutorials/1_tip4pew_water/


first, we create a topology


----------- topol.top ------------

#include "oplsaa.ff/forcefield.itp"
#include "oplsaa.ff/tip4pew.itp"

[ System ]
TIP4PEW

[ Molecules ]

----------------------------------


then, we add water molecules by gmx solvate

$ gmx solvate -cs tip4p -o conf.gro -box 2.3 2.3 2.3 -p topol.top



-----

then, just do it. first we do energy minimization

$ gmx grompp -f mdp/min.mdp -o min -pp min -po min
$ gmx mdrun -deffnm min

$ gmx grompp -f mdp/min2.mdp -o min2 -pp min2 -po min2 -c min -t min
$ gmx mdrun -deffnm min2

check the Potential

$ gmx energy -f min.edr -o min-energy.xvg
$ gmx energy -f min2.edr -o min2-energy.xvg



-----

then we do NVT equilibration

$ gmx grompp -f mdp/eql.mdp -o eql -pp eql -po eql -c min2 -t min2
$ gmx mdrun -deffnm eql

check the Temperature

$ gmx energy -f eql.edr -o eql-temp.xvg 



-----

then we do NPT equilibration

$ gmx grompp -f mdp/eql2.mdp -o eql2 -pp eql2 -po eql2 -c eql -t eql
$ gmx mdrun -deffnm eql2

check the Pressure

$ gmx grompp -f mdp/eql2.mdp -o eql2 -pp eql2 -po eql2 -c eql -t eql
$ gmx mdrun -deffnm eql2




-----

production run

$ gmx grompp -f mdp/prd.mdp -o prd -pp prd -po prd -c eql2 -t eql2
$ gmx mdrun -deffnm prd


