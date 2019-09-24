# Установка и работа с Plumed
Вся основная информация доступна здесь: https://plumed.github.io/doc-v2.4/user-doc/html/index.html
## Установка 
Сначала требуется установить пакетов Plumed. Второй шаг - пропэтчить код Gromacs и собрать его. 
### Plumed
Для компилляции кода исполmуем cmake и intel. Нужно загрузить модули:
```
module load cmake/3.12.3 openmpi/1.10.7-icc intel/15.0.3 mkl/2015.3.187-ilp64 cuda/8.0
```
Ссылка на скачивание: http://www.plumed.org/get-it. Последняя вресия - 2.5.

На Ломоносова можно скопировать архив из загрузок или сразу загрузить с помощью wget. Разархивируем, переходим в папку plumed. В ней выполняем 
```
./configure —prefix=$HOME/software/plumed/
 make
 make install
 ```
### Gromacs
Скачиваем здесь: http://manual.gromacs.org/documentation/2018/download.html. Распаковываем архив (tar xfz gromacs-2018.tar.gz), переходим в папку. Здесь выполняем:
```
plumed patch -p
mkdir build
cd build
cmake .. -DGMX_BUILD_OWN_FFTW=ON -DBUILD_SHARED_LIBS=OFF -DGMX_PREFER_STATIC_LIBS=ON -DGMX_MPI=on -DGMX_GPU=on -DGMX_DOUBLE=off -DCMAKE_INSTALL_PREFIX=$HOME/software/gromacs_plumed/ -DGMX_DEFAULT_SUFFIX=OFF -DGMX_BINARY_SUFFIX=_plumed -DGMX_LIBS_SUFFIX=_plumed 
make
make install
source $HOME/software/gromacs_plumed/bin/GMXRC
```
P.s. Перемнная HOME в моем случае содержала home/kniazevaanastasiia2015_1609. 

Можно устновить только mdrun, используя опцию -DGMX_BUILD_MDRUN_ONLY=on при выполнении cmake. Для реалзации остальных функций Gromacs использовать Gromacs+Plumed не обязательно. 
### Готовый Gromacs_Plumed
Можно установить необходимые модули, скопировать папку и сделать сор:
```
module load openmpi/1.10.7-icc cuda/8.0
scp -r /home/kniazevaanastasiia2015_1609/software/gromacs_plumed $HOME/software/gromacs_plumed
source $HOME/software/gromacs_plumed/bin/GMXRC
```
Надеюсь, это сработает :)
## Работа с Plumed 
Длря работы с Plumed нужно создать некоторый входной файл, содержащий информацию о вычисленияях, которые должны быть произведены. В этом файле нужно задать коллективные переменные (https://plumed.github.io/doc-v2.4/user-doc/html/colvarintro.html) или функцию от них и некоторый анализ (https://plumed.github.io/doc-v2.4/user-doc/html/_analysis.html) или параметры метадинамики (https://plumed.github.io/doc-v2.4/user-doc/html/_bias.html).

Пример файла с параметрами для расчета и записи в файл colvar значений Path CV по готовой траектории (path_plumed):
```
 p1: PATHMSD REFERENCE=selected_traject.pdb  LAMBDA=500.0 NEIGH_STRIDE=4 NEIGH_SIZE=8
 PRINT ARG=p1.sss,p1.zzz STRIDE=1 FILE=colvar FMT=%8.4f
 ```
 в коммандной строке:
 ```
 plumed driver-float --plumed path.plumed --mf_xtc ../md_0_1.xtc 
 ```
 Пример запуска метадинамики с помощью gmx_plumed (файл с параметрами (path_metad):
 ```
  p1: PATHMSD REFERENCE=selected_traject.pdb  LAMBDA=500.0 NEIGH_STRIDE=4 NEIGH_SIZE=8
 PRINT ARG=p1.sss,p1.zzz STRIDE=500 FILE=colvar_metad FMT=%8.4f
 METAD ARG=p1.sss SIGMA=0.2 HEIGHT=0.3 PACE=500 LABEL=restraint FILE=HILLS
 ```
 Запускается метадинамика с помощью gmx_plumed с указыванием файла с параметрами (только прри выполнении mdrun, все остальное, включая создание tpr-файла можно делать с помощью обычного громакса):
 ```
 gmx_plumed mdrun -ntmpi 1 -deffnm md -plumed path_metad
 ```
 Анализ файла, в который были записаны значения коллективной переменной в ходе метадинамики можно проводить напрямую из colvar_metad (функции значения коллективной переменной от времени симуляции). 
 
 Для нахождения профиля энергии суммируются гауссианы, записанные в HILLS,:
 ```
 plumed sum_hills --hills HILLS
 ```
 Эта команда генерирует файл fes.dat, в котором записана зависимость энергии системы от коллективной переменной. 
