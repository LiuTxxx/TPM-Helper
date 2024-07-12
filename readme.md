
This repo consists of two TPM tools:
## 1. Counter Creation
ENGRAFT use TPM counters and assume counters are already created. This repo helps to create TPM counters in a proper way so that the created counters can be used by ENGRAFT directly.


### Compilation
Make sure tss2 is installed (https://tpm2-tools.readthedocs.io/en/stable/INSTALL/):
```
git clone https://github.com/tpm2-software/tpm2-tss.git
cd tpm2-tss
./bootstrap
./configure --prefix=/usr
make -j5
sudo make install
```
The "oe_sdk" docker images hosted in "registry.cn-shenzhen.aliyuncs.com/openenclave_sdk/oesdk_v0_17_0:2024_06_30" (version 2024_06_30 or later) is equipped with the necessary dependencies (including tss2) to build the project.
So if using these images, just use the following command to build the project:
```
mkdir build
cd build
cmake ..
make -j
```
### Run
To run the project, we need to have a TPM2.0 envrionment (/dev/tpm0 and /dev/tpmrm0 should be aveilable).
If you are using the "oe_sdk" docker images, you can run the project with the following command:

```
cd build
./tpm_main
# this will create 6 counter for 3 engraft node with 2 counter each
```

Also note that we can run "oe_sdk" docker images with the following command to enable the TPM2.0 device and SGX device:
```
docker run --network=host --name oe_sdk_vechain --device /dev/tpm0:/dev/tpm0 --device /dev/tpmrm0:/dev/tpmrm0 --device /dev/sgx_enclave:/dev/sgx/enclave --device /dev/sgx_provision:/dev/sgx/provision -i -t registry.cn-shenzhen.aliyuncs.com/openenclave_sdk/oesdk_v0_17_0:2024_06_30
```

## 2. EK Retrieval
An ENGRAFT use the TPM EK (endorsement key) to verify the identity of the TPM, and the "endorsement_key" sub-directory in this repo helps to retrieve the EK from the TPM (see endorsement_key/readme.txt for more details).
Once we get the PEM-encoded EK (rsa_ek_pub.pem), we need to update the "TPM_ENDORSEMENT_KEY" variable in the ENGRAFT source code (https://github.com/LiuTxxx/ENGRAFT-TPM/blob/main/src/sgxbutil/state_cont/tpm_counter.cpp)
