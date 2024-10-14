# Run_NS-2_2_35_network_simulator_with_DiffServ

|Date|Author|Version|Change reference|
|:-- |:--  |:--: |:--
|2024-10-06|Abdeldjalil MAHFOUDI|0.1|Initial document|

## Document presentation

* This document explains how to install NS-2 2.35 network simulator on Ubuntu and run it as DiffServ

## Install NS-2 2.35 network simulator

* Install the required packages

~~~bash
sudo apt install build-essential autoconf automake libxmu-dev libx11-dev xorg-dev libperl4-corelibs-perl
~~~

* Open the file /etc/apt/sources.list using sudo mode and include the following line

~~~bash
sudo nano /etc/apt/sources.list

deb http://in.archive.ubuntu.com/ubuntu bionic main universe
~~~

* Install gcc-4.8 and g++-4.8

~~~bash
sudo apt update
sudo apt install gcc-4.8 g++-4.8
~~~

* Unzip the ns2 packages to home directory

~~~bash
tar zxvf ns-allinone-2.35.tar.gz
cd ns-allinone-2.35/
~~~

* Execute the following commands to configure Makefiles with **gcc-4.8** and **g++-4.8**

~~~bash
sed -i 's/@CC@/gcc-4.8/g' ns-2.35/Makefile.in
sed -i 's/@CPP@/g++-4.8/g' ns-2.35/Makefile.in
sed -i 's/@CXX@/g++-4.8/g' ns-2.35/Makefile.in

sed -i 's/@CC@/gcc-4.8/g' nam-1.15/Makefile.in
sed -i 's/@CXX@/g++-4.8/g' nam-1.15/Makefile.in
sed -i 's/@CPP@/g++-4.8/g' nam-1.15/Makefile.in

sed -i 's/@CC@/gcc-4.8/g' xgraph-12.2/Makefile.in
sed -i 's/@CPP@/g++-4.8/g' xgraph-12.2/Makefile.in
sed -i 's/@CXX@/g++-4.8/g' xgraph-12.2/Makefile.in

sed -i 's/@CC@/gcc-4.8/g' otcl-1.14/Makefile.in
sed -i 's/@CPP@/g++-4.8/g' otcl-1.14/Makefile.in
sed -i 's/@CXX@/g++-4.8/g' otcl-1.14/Makefile.in
~~~

* Open the file ns-2.35/linkstate/ls.h and make the following change

~~~bash
#Change at the Line no 137
void eraseAll() { erase(baseMap::begin(), baseMap::end()); }

#to the following line
void eraseAll() { this->erase(baseMap::begin(), baseMap::end()); }
~~~

* Run the "install" script

~~~bash
./install
~~~

* Export ns2 commands in ~/.bashrc by adding those lines

~~~bash
export PATH=$PATH:/home/<yourusername>/ns-allinone-2.35/bin:/home/<yourusername>/ns-allinone-2.35/tcl8.5.10/unix:/home/<yourusername>/ns-allinone-2.35/tk8.5.10/unix

export LD_LIBRARY_PATH=/home/<yourusername>/ns-allinone-2.35/otcl-1.14:/home/<yourusername>/ns-allinone-2.35/lib
~~~

* Execute the following command to apply changes in ~/.bashrc

~~~bash
source .bashrc
~~~

* To make sure that the installation is correct, you can verify it by executing the "validate" script

~~~bash
./validate
(Validation can take 1-30 hours to run.)
.
.
.
All test output agrees with reference output.
Wed Sep 25 10:36:22 PM CEST 2024
These messages are NOT errors and can be ignored:
    warning: using backward compatibility mode
    This test is not implemented in backward compatibility mode
~~~

## Run NS-2 2.35 as a DiffServ

* Once the installation end validation is correct, now we can run NS-2 2.35 as a DiffServ. Go to diffserve repository

~~~bash
cd ns-2.35/tcl/ex/diffserv

ls
ds-cbr-srtcm.tcl  ds-cbr-tb-PRI.tcl  ds-cbr-tb-RR.tcl  ds-cbr-tb.tcl  ds-cbr-tb-WIRR.tcl  ds-cbr-tb-WRR.tcl  ds-cbr-trtcm.tcl  ds-cbr-TSW3CM.tcl  null.tcl
~~~

* srTCM policer

~~~bash
$ ns ds-cbr-srtcm.tcl

Policy Table(2):
Flow (0 to 5): srTCM policer, initial code point 10, CIR 1000000.0 bps, CBS 2000.0 bytes, EBS 3000.0 bytes.
Flow (1 to 5): srTCM policer, initial code point 10, CIR 1000000.0 bps, CBS 2000.0 bytes, EBS 6000.0 bytes.

Policer Table:
srTCM policer code point 10 is policed to yellow code point 11 and red code point 12.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    14992    12482       99     2411
 10     5000     5000        0        0
 11        9        9        0        0
 12     9983     7473       99     2411

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    29992    24975       99     4918
 10    10000    10000        0        0
 11        9        9        0        0
 12    19983    14966       99     4918

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    44992    37473       99     7420
 10    15000    15000        0        0
 11        9        9        0        0
 12    29983    22464       99     7420

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    59992    49973       99     9920
 10    20000    20000        0        0
 11        9        9        0        0
 12    39983    29964       99     9920
~~~

* Token Bucket PRI policer

~~~bash
$ ns ds-cbr-tb-PRI.tcl

Policy Table(2):
Flow (0 to 5): Token Bucket policer, initial code  point 20, CIR 1000000.0 bps, CBS 10000.0 bytes.
Flow (1 to 5): Token Bucket policer, initial code  point 10, CIR 1000000.0 bps, CBS 10000.0 bytes.

Policer Table:
Token Bucket policer code point 10 is policed to code point 11.
Token Bucket policer code point 20 is policed to code point 21.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    19989    12531     6012     1446
 10     2508     2484       24        0
 11     7486     5201     1272     1013
 20     2508     2431       77        0
 21     7487     2415     4639      433

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    39989    25035    11835     3119
 10     5008     4984       24        0
 11    14986    10203     2586     2197
 20     5008     4931       77        0
 21    14987     4917     9148      922

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    59989    37532    17714     4743
 10     7508     7484       24        0
 11    22486    15203     3936     3347
 20     7508     7431       77        0
 21    22487     7414    13677     1396

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    79989    50030    23564     6395
 10    10008     9984       24        0
 11    29986    20200     5303     4483
 20    10008     9931       77        0
 21    29987     9915    18160     1912
~~~

* Token Bucket RR policer

~~~bash
$ ns ds-cbr-tb-RR.tcl

Policy Table(2):
Flow (0 to 5): Token Bucket policer, initial code  point 20, CIR 1000000.0 bps, CBS 10000.0 bytes.
Flow (1 to 5): Token Bucket policer, initial code  point 10, CIR 1000000.0 bps, CBS 10000.0 bytes.

Policer Table:
Token Bucket policer code point 10 is policed to code point 11.
Token Bucket policer code point 20 is policed to code point 21.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    19989    12527     5880     1582
 10     2508     2480       28        0
 11     7486     3783     2916      787
 20     2508     2493       15        0
 21     7487     3771     2921      795

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    39989    25029    11797     3163
 10     5008     4980       28        0
 11    14986     7536     5869     1581
 20     5008     4993       15        0
 21    14987     7520     5885     1582

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    59989    37533    17699     4757
 10     7508     7480       28        0
 11    22486    11286     8821     2379
 20     7508     7493       15        0
 21    22487    11274     8835     2378

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    79989    50027    23627     6335
 10    10008     9980       28        0
 11    29986    15033    11804     3149
 20    10008     9993       15        0
 21    29987    15021    11780     3186
~~~

* Token Bucket policer

~~~bash
$ ns ds-cbr-tb.tcl

Policy Table(2):
Flow (0 to 5): Token Bucket policer, initial code  point 10, CIR 1000000.0 bps, CBS 3000.0 bytes.
Flow (1 to 5): Token Bucket policer, initial code  point 10, CIR 1000000.0 bps, CBS 10000.0 bytes.

Policer Table:
Token Bucket policer code point 10 is policed to code point 11.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    12494    12494        0        0
 10     5009     5009        0        0
 11     7485     7485        0        0

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    24994    24994        0        0
 10    10009    10009        0        0
 11    14985    14985        0        0

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    37494    37494        0        0
 10    15009    15009        0        0
 11    22485    22485        0        0

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    49994    49994        0        0
 10    20009    20009        0        0
 11    29985    29985        0        0
~~~

* Token Bucket WIRR policer

~~~bash
$ ns ds-cbr-tb-WIRR.tcl

Policy Table(2):
Flow (0 to 5): Token Bucket policer, initial code  point 20, CIR 1000000.0 bps, CBS 10000.0 bytes.
Flow (1 to 5): Token Bucket policer, initial code  point 10, CIR 1000000.0 bps, CBS 10000.0 bytes.

Policer Table:
Token Bucket policer code point 10 is policed to code point 11.
Token Bucket policer code point 20 is policed to code point 21.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    19989    12534     6077     1378
 10     2508     2390      118        0
 11     7486     1384     5885      217
 20     2508     2508        0        0
 21     7487     6252       74     1161

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    39989    25038    12126     2825
 10     5008     4890      118        0
 11    14986     2631    11915      440
 20     5008     5008        0        0
 21    14987    12509       93     2385

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    59989    37537    18175     4277
 10     7508     7390      118        0
 11    22486     3883    17950      653
 20     7508     7508        0        0
 21    22487    18756      107     3624

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    79989    50024    24192     5773
 10    10008     9890      118        0
 11    29986     5132    23948      906
 20    10008    10008        0        0
 21    29987    24994      126     4867
~~~

* Token Bucket WRR policer

~~~bash
$ ns ds-cbr-tb-WRR.tcl

Policy Table(2):
Flow (0 to 5): Token Bucket policer, initial code  point 20, CIR 1000000.0 bps, CBS 10000.0 bytes.
Flow (1 to 5): Token Bucket policer, initial code  point 10, CIR 1000000.0 bps, CBS 10000.0 bytes.

Policer Table:
Token Bucket policer code point 10 is policed to code point 11.
Token Bucket policer code point 20 is policed to code point 21.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    19989    12535     6055     1399
 10     2508     2389      119        0
 11     7486     1384     5871      231
 20     2508     2508        0        0
 21     7487     6254       65     1168

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    39989    25040    12106     2843
 10     5008     4889      119        0
 11    14986     2630    11894      462
 20     5008     5008        0        0
 21    14987    12513       93     2381

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    59989    37540    18189     4260
 10     7508     7389      119        0
 11    22486     3884    17924      678
 20     7508     7508        0        0
 21    22487    18759      146     3582

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    79989    50026    24222     5741
 10    10008     9889      119        0
 11    29986     5131    23951      904
 20    10008    10008        0        0
 21    29987    24998      152     4837
~~~

* trTCM policer

~~~bash
$ ns ds-cbr-trtcm.tcl

Policy Table(2):
Flow (5 to 0): trTCM policer, initial code point 10, CIR 1000000.0 bps, CBS 2000.0 bytes, PIR 2000000.0 bps, PBS 3000.0 bytes.
Flow (5 to 1): trTCM policer, initial code point 10, CIR 1000000.0 bps, CBS 2000.0 bytes, PIR 1000000.0 bps, PBS 3000.0 bytes.

Policer Table:
trTCM policer code point 10 is policed to yellow code point 11 and red code point 12.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    14992    12492      173     2327
 10     5000     5000        0        0
 11     2500     2500        0        0
 12     7492     4992      173     2327

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    29992    24986      298     4708
 10    10000    10000        0        0
 11     5000     5000        0        0
 12    14992     9986      298     4708

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    44992    37485      451     7056
 10    15000    15000        0        0
 11     7500     7500        0        0
 12    22492    14985      451     7056

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    59992    49986      532     9474
 10    20000    20000        0        0
 11    10000    10000        0        0
 12    29992    19986      532     9474
~~~

* TSW3CM policer

~~~bash
$ ns ds-cbr-TSW3CM.tcl

Policy Table(2):
Flow (0 to 5): TSW3CM policer, initial code point 10, CIR 100000.0 bps, PIR 500000.0 bytes.
Flow (1 to 5): TSW3CM policer, initial code point 10, CIR 400000.0 bps, PIR 1000000.0 bytes.

Policer Table:
TSW3CM policer code point 10 is policed to yellow code point 11 and red code point 12.


Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    14992    12475      118     2399
 10     1386     1386        0        0
 11     2686     2686        0        0
 12    10920     8403      118     2399

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    29992    24969      118     4905
 10     2579     2579        0        0
 11     5279     5279        0        0
 12    22134    17111      118     4905

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    44992    37462      118     7412
 10     3815     3815        0        0
 11     7709     7709        0        0
 12    33468    25938      118     7412

Packets Statistics
=======================================
 CP  TotPkts   TxPkts   ldrops   edrops
 --  -------   ------   ------   ------
All    59992    49951      118     9923
 10     5103     5103        0        0
 11    10238    10238        0        0
 12    44651    34610      118     9923
~~~

## Sources
https://sourceforge.net/projects/nsnam/files/allinone/ns-allinone-2.35/
https://stackoverflow.com/questions/17229432/ns2-validate-overall-report-some-tests-failed
https://www.nsnam.com/2020/06/installation-of-ns2-ns-235-in-ubuntu.html
https://www.isi.edu/websites/nsnam/ns/doc/ns_doc.pdf
