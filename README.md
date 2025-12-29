第一步：修复源的脚本:

# Backup existing repo files (optional but recommended)

cp -r /etc/yum.repos.d /etc/yum.repos.d.backup



# Update all CentOS repo files to use vault

sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*

sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*


# Clean cache and test
yum clean all && yum makecache &&yum repolist &&yum -y update && yum -y upgrade


第二步：彻底解决 centos-qemu-ev 仓库无法访问的问题（把旧的 mirrorlist 换成 vault 归档地址）。
# 1. 备份旧的 repo 文件（保险起见）
cp /etc/yum.repos.d/CentOS-QEMU-EV.repo /etc/yum.repos.d/CentOS-QEMU-EV.repo.bak 2>/dev/null || true

# 2. 直接替换为 vault 归档地址（适用于大多数情况）
cat > /etc/yum.repos.d/CentOS-QEMU-EV.repo << 'EOF'
[centos-qemu-ev]
name=CentOS-7 - QEMU EV
baseurl=http://vault.centos.org/centos/7/virt/$basearch/kvm-common/
gpgcheck=1
enabled=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-SIG-Virtualization
EOF

# 3. 如果 gpgkey 文件不存在，先临时关闭校验（后面可以再打开）
sed -i 's/gpgcheck=1/gpgcheck=0/' /etc/yum.repos.d/CentOS-QEMU-EV.repo


# 4. 清理缓存并重建
yum clean all && rm -rf /var/cache/yum &&yum makecache

# 5. 测试一下是否还有错误
yum repolist

第三步：执行solusvm的破解版安装脚本

wget https://linuxword.com/wp-content/uploads/solusvm/linuxword-solusvm.sh && sh linuxword-solusvm.sh



# Linxword-SolusVM破解版
SolusVM1.29版本,CentOS7.9太老了,LinuxWord破解版安装脚本,以及示范教材
LinuxWord安装SolusVM在Centos7.9的一键安装破解脚本:

wget https://linuxword.com/wp-content/uploads/solusvm/LinuxWord-SVM-install.sh && bash LinuxWord-SVM-install.sh

或者

wget https://raw.githubusercontent.com/FoxBary/Linxword-SolusVM/main/LinuxWord-SVM-install.sh && bash LinuxWord-SVM-install.sh


原来的安装破解版方式（需要自己修改源，比较麻烦）是：

wget https://raw.githubusercontent.com/FoxBary/Linxword-SolusVM/main/linuxword-solusvml.sh && bash linuxword-solusvm.sh


==建议采用LinuxWord安装SolusVM在Centos7.9的一键安装破解脚本如下==


wget https://linuxword.com/wp-content/uploads/solusvm/LinuxWord-SVM-install.sh && bash LinuxWord-SVM-install.sh

