# docker

## harbor
1. 准备工作
    ```
    1. 安装docker-compose
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

    sudo chmod +x /usr/local/bin/docker-compose

    ```
1. 下载解压harbor(offline或者online)
    ```
    1. curl -L https://github.com/goharbor/harbor/releases/download/v2.2.2/harbor-online-installer-v2.2.2.tgz -o harbor-online-installer-v2.2.2.tgz

    2. tar xzvf harbor-online-installer-version.tgz

    ```
1. 设置https连接到harbor
    ```
    1. 生成认证机构的证书
        生成CA证书的私钥: openssl genrsa -out ca.key 4096
        生成CA证书: 
        openssl req -x509 -new -nodes -sha512 -days 360 \ 
            -subj "/C=CN/ST=Beijing/L=Beijin/O=tr/OU=Persional/CN=harbor.tr" \
            -key ca.key \
            -out ca.crt
    2. 生成服务器证书
        证书私钥: openssl genrsa -out harbor.tr.key 4096
        证书的CSR: 
        openssl req -sha512 -new \                       
            -subj "/C=CN/ST=Beijing/L=Beijin/O=tr/OU=Persional/CN=harbor.tr" \
            -key harbor.tr.key \
            -out harbor.tr.csr
    3. 生成x509 v3的扩展文件
        cat > v3.ext <<-EOF
        authorityKeyIdentifier=keyid,issuer
        basicConstraints=CA:FALSE
        keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
        extendedKeyUsage = serverAuth
        subjectAltName = @alt_names

        [alt_names]
        DNS.1=harbor.tr
        DNS.2=harbor
        DNS.3=base
        EOF
    4. 给harbor主机生成证书(用v3.text)
        openssl x509 -req -sha512 -days 3650 \                        
            -extfile v3.ext \
            -CA ca.crt -CAkey ca.key -CAcreateserial \
            -in harbor.tr.csr \
            -out harbor.tr.crt
    5. 将证书提供给Harbor和Docker
        a. 复制服务器证书和密钥到harbor主机
            cp harbor.tr.crt harbor.tr.key /data/cert/
        b. 转换crt为cert给Docker
            openssl x509 -inform PEM -in harbor.tr.crt -out harbor.tr.cert
        c. 复制服务器证书、key、CA文件到Docker的认证目录
            cp harbor.tr.cert harbor.tr.key ca.crt /etc/docker/certs.d/harbor.tr
        d. 重启Docker引擎
            systemctl restart docker
    ```
    
