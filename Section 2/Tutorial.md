# Hadoop Scripts

Start with `hadoop fs -[Commands]`
List all hadoop commands `hadoop fs`

```bash
hadoop fs -ls
hadoop fs -mkdir ml-100k
wget http://media.sundog-soft.com/hadoop/ml-100k/u.data
hadoop fs -copyFromLocal u.data ml-100k/u.data
hadoop fs -ls ml-100k
```

```python
from mrjob.job import MRJob
from mrjob.step import MRStep

class RatingsBreakdown(MRJob):
	def steps(self):
		return[							 
			MRStep(mapper = self.mapper_get_ratings,
				reducer = self.reducer_count_ratings)
]
def mapper_get_ratings(self, _, line)
	(userID，movieID，rating，timestamp)= line.split('\t')
yield rating, 1

def reducer_count_ratings(self, key, values):
	yield key, sum(values)

if __name__ == '__main__':
	RatingsBreakdown.run()
```

# 17. Notes on MRJob installation
original root password: `hadoop`

## For HDP 2.6
```bash
su root
cd /etc/yum.repos.d
cp sandbox.repo /tmp
rm sandbox.repo
cd ~
yum-config-manager --disable HDP-UTILS-1.1.0.22-repo-1
yum install python-pip -y
pip install google-api-python-client==1.6.4
pip install mrjob==0.5.11
yum install nano -y
wget http://media.sundog-soft.com/hadoop/RatingsBreakdown.py
wget http://media.sundog-soft.com/hadoop/TopMovies.py
wget http://media.sundog-soft.com/hadoop/ml-100k/u.data


yum install scl-utils
yum install centos-release-scl
yum install python27
scl enable python27 bash
```

```bash
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
rm /etc/yum.repos.d/CentOS-Base.repo
wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
cd /etc/yum.repos.d
vi CentOS-Base.repo
:%s/\$releasever/6/g
yum clean all
yum makecache
yum -y groupinstall "Development Tools"
```

[Install Python3 Manully](https://docs.posit.co/resources/install-python-source/)

Execute
```bash
python3 TopMovies.py u.data
python3 TopMovies.py -r hadoop --hadoop-streaming-jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-streaming.jar u.data
```

## (Depreciated)For HDP 2.5

1. remove sandbox.repo

```bash
cd /etc/yum.repos.d
cp sandbox.repo /tmp
rm -f sandbox.repo
```

2. Configure repos

```bash
cd /etc/yum.repos.d
vi HDP-UTILS.repo
:%s/enabled=1/enabled=0/g
:wq
vi HDP.repo
:%s/enabled=1/enabled=0/g
:wq
vi CentOS-Base.repo
:%d # clear
```

<details>
	<summary>CentOS-Base.repo from vault.centos.org(Aliyun is invalid)</summary>
	
[base]
name=CentOS-6 - Base
#mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=os&infra=$infra
baseurl=http://vault.centos.org/6.10/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#released updates 
[updates]
name=CentOS-6 - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=updates&infra=$infra
baseurl=http://vault.centos.org/6.10/updates/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#additional packages that may be useful
[extras]
name=CentOS-6 - Extras
#mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=extras&infra=$infra
baseurl=http://vault.centos.org/6.10/extras/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#additional packages that extend functionality of existing packages
[centosplus]
name=CentOS-6 - Plus
#mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=centosplus&infra=$infra
baseurl=http://vault.centos.org/6.10/centosplus/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

#contrib - packages by Centos Users
[contrib]
name=CentOS-6 - Contrib
#mirrorlist=http://mirrorlist.centos.org/?release=6&arch=$basearch&repo=contrib&infra=$infra
baseurl=http://vault.centos.org/6.10/contrib/$basearch/
gpgcheck=1
enabled=0
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-6

</details>


```bash
yum clean all
yum makecache
yum update -y
yum groupinstall "Development Tools" # for install python3
yum install python-pip -y
# pip install mrjob==0.5.11
```
- change pip mirror repository
```bash
mkdir /root/.pip
cd /root/.pip
vi pip.conf
```

<details>
	<summary>pip.conf e.g. mirrors.aliyun.com </summary>

	[global]
	trusted-host = mirrors.aliyun.com
	index-url = https://mirrors.aliyun.com/pypi/simple

</details>

- pip install
- install python from source code e.g. Python-3.9.0 from huaweicloud
```bash
wget https://mirrors.huaweicloud.com/python/3.9.0/Python-3.9.0.tar.xz
cd Python-3.9.0
chmod 777 configure # make it executable
./configure \
	--prefix=/opt/python/${PYTHON_VERSION} \
	--enable-shared \
	--enable-optimizations \
	--enable-ipv6 \
	LDFLAGS=-Wl,-rpath=/opt/python/${PYTHON_VERSION}/lib,--disable-new-dtags
make
sudo make install

pip install mrjob==0.5.11
```