# nmap
Binary nmap file which is working in Lambda environment.

If you want to build your own NMAP for Lambda, then run following commands in Amazon Linux instance:

```bash
sudo yum install git gcc gcc-c++ glibc-static pcre-static

cd /tmp
git clone https://github.com/nmap/nmap.git
cd nmap

export CFLAGS="-march=core2 -O2 -fomit-frame-pointer -pipe -fPIC"
export CXXFLAGS="-march=core2 -O2 -fomit-frame-pointer -pipe -fPIC"

./configure --without-subversion \
            --without-liblua \
            --without-zenmap \
            --with-pcre=/usr \
            --with-libpcap=included \
            --with-libdnet=included \
            --without-ndiff \
            --without-nmap-update \
            --without-ncat \
            --without-liblua \
            --without-nping \
            --without-openssl

sed -i".bak" -e 's_ libpcre/libpcre.a _ _' \
             -e 's_\(..CXX. ..LDFLAGS. -o .. ..OBJS. main.o ..LIBS.\)_\1 /usr/lib64/libpcre.a_' \
             Makefile

make
```

After compilation, you can grap your binary in `/tmp/nmap/nmap`
