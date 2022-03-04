# 1. Edit feed source locally
vim feeds.conf.default
src-git helloworld https://github.com/fw876/helloworld
src-git passwall https://github.com/xiaorouji/openwrt-passwall

# 2. Download source code locally
cd /lede/package/lean/
git clone https://github.com/jerrykuku/lua-maxminddb.git
git clone https://github.com/jerrykuku/luci-app-vssr.git 

# 3. Second Time Compile locally
git pull
./scripts/feeds update -a && ./scripts/feeds install -a
rm -rf ./tmp && rm -rf .config
make menuconfig

# 4. 编译丰富插件时，建议修改下面两项默认大小，留足插件空间。（ x86/64 ）！！！
Target Images ---> (16) Kernel partition size (in MB)                        #默认是 (16) 建议修改 (256)
Target Images ---> (160) Root filesystem partition size (in MB)         #默认是 (160) 建议修改 (512)

# 5. Save and Copy .config to github action

# 6. Change scripts on github action
#add feed source in diy-part1.sh
echo 'src-git helloworld https://github.com/fw876/helloworld' >>feeds.conf.default
echo 'src-git passwall https://github.com/xiaorouji/openwrt-passwall' >>feeds.conf.default

#add additional package in diy-part2.sh
git clone https://github.com/jerrykuku/luci-app-vssr.git package/luci-app-vssr

#Modify default IP in diy-part2.sh
sed -i 's/192.168.1.1/192.168.10.5/g' package/base-files/files/bin/config_generate
