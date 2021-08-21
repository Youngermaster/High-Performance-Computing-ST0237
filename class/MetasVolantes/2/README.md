# Meta volante 2 

## Setup

### Pre Configuration

```
sudo yum install epel-release -y
sudo yum install ansible -y
sudo yum install git -y
sudo yum install vim -y
sudo yum install htop -y
sudo yum update -y
```

### Ansible configurations

The `repo` should be placed in `/root/`, then run:

```
ansible-playbook site.yml -vvv
```

*Note:* the `-vvv` flags are to add verbosity to the execution.

### Infiniband (Under development)

*Note:* The following steps should run in all the available computes.

First of all we will need to install these packages:
```
sudo yum install libibverbs-utils librdmacm infiniband-diags -y
```

Then use:

```
vim /etc/modprobe.d/mlx4.conf
```

And at the end add the following line:

```
options mlx4_core log_num_mtt=24 log_mtts_per_seg=1
```

We will need the `Infiniband MAC Address`
```
ip addr | grep -A 2 ib0 | grep 'link/infiniband' | awk '{print $2}'
```

```
nmcli con add type infiniband con-name ib0 ifname ib0 transport-mode connected mtu 65520 infiniband.mac-address 80:00:02:18:fe:80:00:00:00:00:00:00:00:02:c9:03:00:2f:35:11 ipv4.method static ipv4.addresses 10.150.8.102/24 ipv4.gateway 10.150.8.102
```

```
nmcli con add type infiniband con-name ib0 ifname ib0 transport-mode connected mtu 65520 infiniband.mac-address 80:00:02:18:fe:80:00:00:00:00:00:00:00:02:c9:03:00:2f:3a:71 ipv4.method static ipv4.addresses 10.150.8.103/24 ipv4.gateway 10.150.8.102
```


### HPL

Use the following commands:

```
wget http://www.netlib.org/benchmark/hpl/hpl-2.3.tar.gz
gunzip hpl-2.3.tar.gz; tar -xvf hpl-2.3.tar
mv hpl-2.3 hpl
cd hpl

make arch=Linux_Centos
```