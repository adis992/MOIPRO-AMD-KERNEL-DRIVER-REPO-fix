
########################################################################################################################
NE RADITI YUM UDPATE ZBOG KERNELA VERZIJE I KOMPLIKACIJA KOJE CES IMATI
AKO PRILIKOM INSTLAAACIJE IMAS PROBLEM SA STARIM KERNEL VERZIJAMA JEDAN PO JEDAN BRISES I INSTALIRAS FINO
ZNACI TESTIRANO JE CENTOS 7, UBUNTU A DRAJVERE I KERNEL KOJE SAM TESTIRAO RADI NA TBS6205-quad tuner TBS6910SE TBS6904SE-quad tuner
########################################################################################################################

HOW TO REMOVE ANY file, folder or progr. install

yum remove kernel-ml-3*
ovo je za dell

HOW TO INSTALL(succesfully) ANY file, folder or progr. install
1.Download all file from repository and login to putty and puf *.rpm file to /root  folder
2.yum install kernel-ml-4*.rpm       insalira sve odjednom ili ides 1 po 1

ispravis permisije chmod 777 *  prije pokretanja zbog mogucih greski
########################################################################################################################




######################################IMAS VEC PREUZETO U OVOM REPO 4.14 I kompatibilan je od 4.14-4.19#############

Ako nemas preuzeto, ovdje mozes i neki drugi kernel.

http://193.49.22.109/elrepo/kernel/el7/x86_64/RPMS/
preuzimanje kernela ili ml ili lt  ili obicnog ali gldaj uvijek ml verziju-mainline

preuzimas sljedece

kernel-ml
kernel-ml-devel
kernel-ml-headers
kernel-ml-tools
kernel-ml-tools-libs


skidas sve iste verzije ovog iznad npr  5.4.0-1.el7. u nazivu logika kaze da moras istu sliku i za dev ili headers itd. :)


########################################################################################################################



#######################################           START INSTALL FULL      ################################################


yum install yum-utils
yum install dnf


sudo yum install centos-release-scl
sudo yum install devtoolset-8-gcc*
scl enable devtoolset-8 bash
mv /usr/bin/gcc /usr/bin/gcc-bkk         ///ako postoji
mv /usr/bin/g++ /usr/bin/g++-bkk      ///ako postoji
ln -s /opt/rh/devtoolset-8/root/usr/bin/gcc /usr/bin/gcc
ln -s /opt/rh/devtoolset-8/root/usr/bin/g++ /usr/bin/g++

obavezno provjeri instalacije GCC,MAKE jer ovo je OPEN SOURCE DRIVER LINUX MEDIA 
minimalna verzija gcc za open source za GCC je 8, inace na centos 7 povlaci 4.8.5 moras nadograditi ili remove pa install verziju 8 ili vecu

provjera installacije npr za gcc
gcc -v  ili gcc --version

yum install -y  git make ncurses-devel bison flex elfutils-libelf-devel openssl-devel


eh pa ides na kloniranje linux media repo-a

git clone https://github.com/tbsdtv/media_build.git
git clone --depth=1 https://github.com/tbsdtv/linux_media.git -b latest ./media


cd media_build
make dir DIR=../media
make allyesconfig
sed -i -r 's/(^CONFIG.*_RC.*=)./\1n/g' v4l/.config
sed -i -r 's/(^CONFIG.*_IR.*=)./\1n/g' v4l/.config
make -j4
sudo make install

####################################  FIRMWARE ZA TBS-KARTICE  #############################################################


#
wget http://www.tbsdtv.com/download/document/linux/tbs-tuner-firmwares_v1.0.tar.bz2
sudo tar jxvf tbs-tuner-firmwares_v1.0.tar.bz2 -C /lib/firmware/

#6909
wget http://www.tbsdtv.com/download/document/linux/dvb-fe-mxl5xx.fw 
sudo cp dvb-fe-mxl5xx.fw /lib/firmware/

#24117
wget http://www.tbsdtv.com/download/document/common/tbs-linux-drivers_v130901.zip
unzip -p tbs-linux-drivers_v130901.zip linux-tbs-drivers.tar.bz2 | tar jxOf - linux-tbs-drivers/v4l/tbs6981fe_driver.o.x86_64 | dd bs=1 skip=10144 count=55486 of=dvb-fe-cx24117.fw



sudo reboot

Kad se upali provjeris i kernel i driver, ma da kernel provjeravas kada ga i instaliras jer moras reboot i njega 
ucitati ako nisi postavio grub default name na njegobvo ime.


Nakon instalacije kernela radis driver.

 ls /dev/dvb ili lsdvb ako je isntaliran dvb-apps



########################################################################################################################

MOGUCI PROBLEMI PRILIKOM INSTALACIJE DRAJVERA ILI OSTECENJA KERNELA I SLICNO KOJEG SAM I J ASAM IMAO I MORAS RUCNO DODATI
KERNEL FAJL, UGLAVNOM CITAS PROBLEM-ERROR DA BIH GA ZNAO RIJESITI STO JE LOGICNO :)

ZNACI LICNO SAM IMAO PROBLEM U KERNELU I TO SAM PRIMIJETIO KADA SAM KRENUO INSTALIRATI TBS DRAJVERE PA SAM UNIO RUCNO I KREIRAO FAJL
nospec.h

prjvera da li postoji nospec.h fajl u dir. kernela

ls /lib/modules/$(uname -r)/build/include/linux/nospec.h #

ako ne postoji kreiraj je rucno 

vi /lib/modules/$(uname -r)/build/include/linux/nospec.h

mozes sa neta copy/paste a imas u repo cijeli fajl i nakon ovog jer si u pola instalacije drivera prekinuo ides u media_build folder pa 
make distclean 
make -j4 
make install i onda tek imas uspjesnu instalaciju drivera ukoliko nemas drugi problema

vi nospec.h     za unos rucno ili preuzmi fajl sa neta ili iz ovog repo i dodas na odgovarajuce mjesto

########################################################################################################################

