# MD-simulation
! gmx pdb2gmx -f -o 
## Генерируем файл топологии системы в силовом поле amber99sb-idln и файл с координатами в формате Gromacs

! gmx editconf -f  -o  -d 1.5
#отступ в ячейке белка
! gmx solvate -cp newbox.gro -cs spc216.gro -p topol.top -o solv.gro
 #добавление молекул растворителя
! gmx grompp -f ions.mdp -c solv.gro -p topol.top -o ions.tpr -maxwarn 1
 #добавление ионов
! gmx genion -s ions.tpr -o -p topol.top -pname NA -nname CL -neutral -nconc 0.15
 #добавление ионов part 2
! gmx grompp -f em.mdp -c  -p -o em.tpr -maxwarn 1
 #минимизация энергии в системе
! gmx mdrun -v -deffnm em
 #запуск движка
! gmx grompp -f pr.mdp -c em.gro -r em.gro -p topol.top -o nvt.tpr
 #уравновешивание "утряска" молекул воды

 #запуск движка
! gmx grompp -f md.mdp -c nvt.gro -t nvt.cpt -p topol.top -o p.tpr
 #объединение файлов в tpr
! gmx mdrun -v 
