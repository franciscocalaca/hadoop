# ROTEIRO INSTALACAO HADOOP

## Crie o usuário hduser e o grupo hadoop

```bash
sudo addgroup hadoop
sudo adduser --ingroup hadoop hduser
sudo adduser hduser sudo
```


## Crie as chaves

```bash
su - hduser
ssh-keygen -t rsa -P ""
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```

Teste as chaves

```bash
ssh localhost
```


## Instale os artefatos
  (ambos baixar e instalar)
  Java
  Hadoop
  colocados em /opt
    /opt/java
	/opt/hadoop

## Configure variáveis de ambiente

Edite o arquivo ~/.bashrc e adicione no final

```bash
#Set HADOOP_HOME
export HADOOP_HOME=/opt/hadoop
#Set JAVA_HOME
export JAVA_HOME=/opt/java
# Add bin/ directory of Hadoop to PATH
export PATH=$PATH:$HADOOP_HOME/bin
```

## Crie os diretórios

```bash
mkdir /opt/hdfs
mkdir /opt/hadoop/tmp
```

Mude os proprietários dos diretórios

```bash
chmod -R hduser:hadoop /opt/hadoop
chmod -R hduser:hadoop /opt/hdfs
chmod -R hduser:hadoop /opt/java
```

## Configure os arquivos

$HADOOP_HOME/etc/hadoop/core-site.xml
```xml
<configuration>
   <property>
      <name>hadoop.tmp.dir</name>
      <value>/opt/hadoop/tmp</value>
      <description>Parent directory for other temporary directories.</description>
   </property>
   <property>
      <name>fs.defaultFS</name>
      <value>hdfs://localhost:54310</value>
      <description>The name of the default file system.</description>
   </property>
</configuration>
```

$HADOOP_HOME/etc/hadoop/mapred-site.xml

```xml
<configuration>
   <property>
      <name>dfs.replication</name>
      <value>1</value>
      <description>Default block replication.</description>
   </property>
   <property>
      <name>dfs.datanode.data.dir</name>
      <value>/opt/hdfs</value>
   </property>
</configuration>
```

## Formatação do filesystem

```bash
hdfs namenode -format
```

## Start/Stop

```bash
$HADOOP_HOME/sbin/start-dfs.sh
```

```bash
$HADOOP_HOME/sbin/stop-dfs.sh
```

## Verificação na web

http://localhost:9870/

## Comandos iniciais

hdfs dfs  -ls /

hdfs dfs  -mkdir /exemplo

hdfs dfs -rm -r -f /exemplo



## Mudança de portas da versão 2 para a versão 3


Namenode ports: 50470 --> 9871, 50070 --> 9870, 8020 --> 9820

Secondary NN ports: 50091 --> 9869, 50090 --> 9868

Datanode ports: 50020 --> 9867, 50010 --> 9866, 50475 --> 9865, 50075 --> 9864





