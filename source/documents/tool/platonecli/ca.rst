.. _cli-ca:

=================
CA操作 ca
=================

针对CA相关操作

生成密钥对 CA generateKey
============================

**描述**

生成密钥对并保存至本地。

**参数**

- 可选参数:

.. code:: bash

      --private : while target is public, this flag is needed

- 必选参数:

.. code:: bash

      --file: file path and name
      --curve: SM2 or secp256k1
      --target : public, private or pair
      --format : PEM, HEX or TXT 

**操作**

.. code:: bash

      #1. 根据私钥生成公钥（使用SM2，生成PEM格式）
      ./platonecli ca generateKey --curve SM2 --target public --format PEM --private key.PEM --file public.PEM

      #2. 根据给定的算法生成hex格式私钥
      ./platonecli ca generateKey --curve secp256k1 --target private --format HEX --file private.PEM

      #3. 生成公私钥对
      ./platonecli ca generateKey -curve SM2 --target pair --format PEM --file key.PEM

生成证书签名请求文件 CA generateCSR
========================================

**描述**

生成证书签名请求文件并保存至本地。

**参数**

- 必选参数:

.. code:: bash

      -- organization:                organization name
      -- commonName:                  user name
      -- dgst:                        Signature algorithm（sm3 or SHA256）
      -- file:                        file path and name（PEM）
      -- private:                     private key

**操作**

.. code:: bash

      ./platonecli ca generateCSR --organization wxbc --commonName test --dgst sm3 --private key.PEM --file testCSR.PEM

生成自签名证书 CA genSelfSignCert
========================================

**描述**

生成自签名证书文件并保存至本地。

**参数**

- 必选参数:

.. code:: bash

      -- organization:                organization name
      -- commonName:                  user name
      -- dgst:                        Signature algorithm（sm3 or SHA256）
      -- serial:                      uint32
      -- file:                        target file path and name（PEM）
      -- private:                     private key

**操作**

.. code:: bash

      ./platonecli ca genSelfSignCert --organization wxbc --commonName test --dgst sm3 --serial 1 --file testcert.PEM --private key.PEM

根据CSR生成证书 CA generateCA
=================================

**描述**

生成证书文件并保存至本地。

**参数**

- 必选参数:

.. code:: bash

      -- csr:                CSRfile path
      -- dgst:               Signature algorithm（sm3 or SHA256）
      -- serial:             uint32
      -- file:               target file path and name
      -- private:            keyfile
      -- ca:                 ca cert path # 发证方证书

**操作**

.. code:: bash

      ./platonecli ca create --dgst sm3 --serial 2 --ca cacert.PEM --csr testCSR.PEM --file test1cert.PEM --private key.PEM

验证证书 CA verifyCA
=========================

**描述**

验证证书。

**参数**

- 必选参数:

.. code:: bash

      -- ca: organization certificate
      -- cert: certificatefile path(cert)

**操作**

.. code:: bash

      ./platonecli ca verify --ca cacert.PEM --cert test1cert.PEM 