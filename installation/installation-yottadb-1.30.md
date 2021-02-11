## YottaDB v1.30 Installation Manual

##### Component

- machine with OS RedHat 7
- yottadb.tgz
- ydb_dist_r130_rhel.tgz
- path /data; /app; /global/ydbdir_db; /global/ydbdir_jnl

### 1.) Create User for Yotta DB
- Run as root

Exec command:
```
groupadd -g 1001 ydbadm
useradd -u 1001 -g 1001 ydbadm
echo -e "ydbadm\nydbadm" | passwd ydbadm

```

### 2.) Extract YottaDB 1.28
- Run as root
- Assume the file are yottadb.tgz and ydb_dist_r130_rhel.tgz are on path /Source

Exec command:
```
tar -xzvf yottadb.tgz
cd yottadb
./install_yottadb.sh
chmod -R 755 /data/yottadb
chown -R ydbadm:ydbadm /global
chown -R ydbadm:ydbadm /ydb
chown -R ydbadm:ydbadm /ydbdir
chown -R ydbadm:ydbadm /data/yottadb
chown root:root -Rh /data/yottadb/ydb/ydb_dist_r128
chmod 4555 /data/yottadb/ydb/ydb_dist_r128/gtmsecshr
```

### 3.) Upgrade YottaDB 1.30
- Run as root
Exec command:
```
cd /Source/
tar -xvzf ydb_dist_r130_rhel.tgz
mv ydb_dist_r130 /data/yottadb/ydb
vi /ydbdir/ydbenv
export ydb_dist=/ydb/ydb_dist_r130
vi /ydbdir/ydbenv_local
export ydb_dist=/ydb/ydb_dist_r130
cp -p /ydb/ydb_dist_r130/yottadb.pc /usr/share/pkgconfig
chown ydbadm:ydbadm /ydbdir/ydbenv
chown ydbadm:ydbadm /ydbdir/ydbenv_local
```

### 4.) Extend Lenght on YottaDB
- Run as ydbadm
Exec command:
```
cd /ydbdir
. ./ydbenv_local
mupip set -rec=300000 -reg DATA
mupip set -key=500 -reg DATA
mupip set -null_subscripts=TRUE -region DATA
```

### 5.) Functional Test
- Run as ydbadm
Exec command:
```
cd /ydbdir
 . ./ydbenv_local
ydb
YDB> set ^NAME(0)="First Name|Last Name|Nickname"
YDB> set ^NAME(1)="JUDHO|PERWIRO|JIPI"
YDB> zwr ^NAME
^NAME(0)="First Name|Last Name|Nickname"
^NAME(1)="JUDHO|PERWIRO|JIPI"
YDB>h
Octo
OCTO> CREATE TABLE `NAME` (`First_Name` VARCHAR(25) PIECE 1, `Last_Name` VARCHAR(25) PIECE 2, `SEQ` INTEGER PRIMARY KEY PIECE 3) GLOBAL "^NAME(keys(""SEQ""))" DELIM "|";
OCTO> select * from NAME;
First Name|Last Name|0
JUDHO|PERWIRO|1
```

### 6.) Cleanup
- Run as ydbadm
Exec command:
```
cd /ydbdir
. ./ydbenv
Ydb
YDB>k ^NAME
YDB>zwr ^NAME
%YDB-E-GVUNDEF, Global variable undefined: ^NAME
OCTO> DROP TABLE NAME;
OCTO>select * from NAME;
OCTO>
```
