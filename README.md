# MD-simulation
from pymol import cmd # pymol на минималках
from IPython.display import Image
cmd.reinitialize()
cmd.load("1L2Y.pdb")
cmd.select('state 1') # выбрать одно состояние, поскольку это ЯМР структура
cmd.orient("1L2Y")
cmd.ray(700)
cmd.png("1L2Y.png")
Image(filename = "1L2Y.png", unconfined=True)


! gmx pdb2gmx -f -o 
# отступ в ячейке белка
! gmx editconf -f  -o  -d 1.5
# добавление молекул растворителя
! gmx solvate -cp newbox.gro -cs spc216.gro -p topol.top -o solv.gro
# добавление ионов
! gmx grompp -f ions.mdp -c solv.gro -p topol.top -o ions.tpr -maxwarn 1
# добавление ионов part 2
! gmx genion -s ions.tpr -o -p topol.top -pname NA -nname CL -neutral -nconc 0.15
# минимизация энергии в системе
! gmx grompp -f em.mdp -c  -p -o em.tpr -maxwarn 1
# запуск движка
! gmx mdrun -v -deffnm em
# уравновешивание "утряска" молекул воды
! gmx grompp -f pr.mdp -c em.gro -r em.gro -p topol.top -o nvt.tpr

! gmx mdrun -v

! gmx grompp -f md.mdp -c nvt.gro -t nvt.cpt -p topol.top -o p.tpr
# объединение файлов в tpr
! gmx mdrun -v
