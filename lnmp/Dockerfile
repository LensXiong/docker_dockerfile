# 制定基础镜像
FROM centos:centos7

# 作者(维护者)
MAINTAINER xiongwang lensxiong@gmial.com

# Linux 工具
    # 常用工具
RUN yum -y install gcc gcc-c++ wget make vim lrzsz htop git unzip \
    # 创建下载目录
    && mkdir /wdata && mkdir /wdata/server \
    # 清理 yum 缓存
    && yum clean all

# PHP-image
    # 下载PHP编译安装所依赖的libiconv库
RUN cd /wdata/server/ && wget https://ftp.gnu.org/pub/gnu/libiconv/libiconv-1.15.tar.gz \
    # 解压libiconv库
    && tar xvzf libiconv-1.15.tar.gz \
    # 进入解压文件夹
    && cd libiconv-1.15 \
    # 配置、编译、安装libiconv库
    && ./configure && make && make install \
    # 进入安装目录
    && cd /wdata/server/ \
    # 下载 PHP 源码
    && wget http://cn2.php.net/get/php-7.1.15.tar.gz/from/this/mirror \
    # 清理 yum 缓存
    && yum clean all \
    # 解压下载包
    && tar xvzf mirror \
    # 重新命名PHP源码文件
    && mv /wdata/server/php-7.1.15 /wdata/server/php7 \
    # 安装所需的依赖包
    && yum -y install openssl openssl-devel libxml2-devel curl-devel libpng-devel freetype-devel \
    # 进入下载目录
    && cd /wdata/server/php7/ \
    # 设置PHP源码编译参数
    && './configure'  '--prefix=/usr/local/php7' '--with-config-file-path=/usr/local/php/etc' '--with-pdo-mysql=mysqlnd' '--with-mysqli=mysqlnd' '--enable-fpm' '--enable-static' '--enable-maintainer-zts' '--enable-inline-optimization' '--enable-sockets' '--enable-wddx' '--enable-zip' '--enable-calendar' '--enable-bcmath' '--enable-soap' '--with-zlib' '--with-iconv' '--with-gd' '--with-xmlrpc' '--enable-mbstring' '--with-curl' '--with-freetype-dir=/usr/local/freetype' '--with-openssl' '--disable-fileinfo' '--with-iconv=/usr/local/libiconv' '--enable-ftp' '--enable-session' '--with-mysql-sock=/tmp/mysql.sock' \
    # 进行编译和安装
    && make && make install \
    # 删除源码包
    && rm -rf /wdata/server/* \
    # 配置PHP全局环境变量
    && echo 'PATH=/usr/local/php7/bin/:$PATH' >>/etc/profile \
    # 配置PHP-FPM全局环境变量
    && echo 'PATH=/usr/local/php7/sbin/:$PATH' >>/etc/profile \
    # 重新执行环境变量文件
    && source /etc/profile \
    # 复制并重新命名php-fpm配置文件
    && cp /usr/local/php7/etc/php-fpm.conf.default /usr/local/php7/etc/php-fpm.conf \
    # 复制并重新命名www.conf配置文件
    && cp /usr/local/php7/etc/php-fpm.d/www.conf.default /usr/local/php7/etc/php-fpm.d/www.conf  \
    # 启动php-fpm服务
    && php-fpm \


# Nginx-image
    # 安装Nginx编译依赖包 
RUN yum -y install gcc gcc-c++ pcre pcre-devel zlib-devel openssl-devel jemalloc-devel \
    # 清理 yum 缓存
    && yum clean all \
    # 进入下载目录
    && cd /wdata/server/ \
    # 下载 Nginx 源码
    && wget https://nginx.org/download/nginx-1.12.2.tar.gz \
    # 解压源码包
    && tar xzf nginx-1.12.2.tar.gz \
    # 重新命名文件
    && mv nginx-1.12.2 nginx \
    # 删除源码包
    && rm -f nginx-1.12.2.tar.gz \
    # 更新 yum 源
    && yum -y update \
    # 创建 Nginx 的使用用户 www
    && useradd www -s /sbin/nologin -M \
    # 进入 Nginx 源码下载解压的目录
    && cd /wdata/server/nginx \
    # 设置编译参数
    && ./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module  --with-http_sub_module \
    # 编译程序
    && make \
    # 安装
    && make install \
    # Nginx 全局环境变量设置
    && echo 'PATH=/usr/local/nginx/sbin/:$PATH' >> /etc/profile \
    # 重新执行系统环境变量
    && source /etc/profile
