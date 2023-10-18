    

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
```bash
su root
cd /etc/yum.repos.d
cp sandbox.repo /tmp
rm sandbox.repo
cd ~
yum install python-pip -y
pip install google-api-python-client==1.6.4
pip install mrjob==0.5.11
yum install nano -y
wget http://media.sundog-soft.com/hadoop/RatingsBreakdown.py
wget http://media.sundog-soft.com/hadoop/ml-100k/u.data

yum install scl-utils
yum install centos-release-scl
yum install python27
scl enable python27 bash
```

yum clean all
yum makecache
cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup
rm /etc/yum.repos.d/CentOS-Base.repo
