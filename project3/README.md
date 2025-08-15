# Project 3:  circom实现poseidon2哈希算法

本项目通过circom实现poseidon2哈希算法的电路，其中参数参照文档https://eprint.iacr.org/2023/323.pdf的table1，(n,t,d)=(256,3,5),RF=8, RP=56。
##  poseidon2哈希算法
本章节负责介绍poseidon2哈希算法的运行原理。由于哈希算法输入只需要考虑一个block，因此这里也按照一个block进行介绍。

### 加轮常量
对block每一个元素都加上轮常量C。

###  过S盒
S盒过程相当于对block中的元素x取幂，这里由于d=5，因此是取x的5次方。这里要区分全轮下的过S盒和部分轮下的过S盒。

- 全轮：block的每一个元素都过S盒

- 部分轮：block的第一个元素过S盒


### 线性混合MDS矩阵
设S为输入向量，那么公式：S←M⋅S

### 哈希算法流程
哈希算法运算的总轮数为 ：R = RF + RP

首先进行RF/2轮的部分轮：加轮常量、过S盒盒MDS变换；

之后进行RP轮的全轮：加轮常量、过S盒盒MDS变换；

最后进行RF/2轮的部分轮：加轮常量、过S盒盒MDS变换。

## 

##  实现过程
首先完成电路的实现poseidon2.circom后，将输入文件input.json准备好，之后运行下面的命令。

```bash
# 1. 编译电路
circom poseidon2.circom --r1cs --wasm --sym

# 2. 生成见证
cd poseidon2_js
node generate_witness.js poseidon2.wasm ../input.json ../witness.wtns

# 3. 验证约束
cd ..
snarkjs wtns check poseidon2.r1cs witness.wtns
```

最后进行验证约束
```bash
lyz@LYZ2022:~/circuits/poseidon2_js$ snarkjs wtns check ../
poseidon2.r1cs ../witness.wtns
[INFO]  snarkJS: WITNESS CHECKING STARTED
[INFO]  snarkJS: > Reading r1cs file
[INFO]  snarkJS: > Reading witness file
[INFO]  snarkJS: ----------------------------
[INFO]  snarkJS:   WITNESS CHECK
[INFO]  snarkJS:   Curve:          bn128
[INFO]  snarkJS:   Vars (wires):   628
[INFO]  snarkJS:   Outputs:        3
[INFO]  snarkJS:   Public Inputs:  0
[INFO]  snarkJS:   Private Inputs: 3
[INFO]  snarkJS:   Labels:         1866
[INFO]  snarkJS:   Constraints:    624
[INFO]  snarkJS:   Custom Gates:   false
[INFO]  snarkJS: ----------------------------
[INFO]  snarkJS: > Checking witness correctness
[INFO]  snarkJS: WITNESS IS CORRECT
[INFO]  snarkJS: WITNESS CHECKING FINISHED SUCCESSFULLY
```

lyz@LYZ2022:~/circuits/poseidon2_js$ snarkjs wtns check ../poseidon2.r1cs ../witness.wtns
[INFO]  snarkJS: WITNESS CHECKING STARTED
[INFO]  snarkJS: > Reading r1cs file
[INFO]  snarkJS: > Reading witness file
[INFO]  snarkJS: ----------------------------
[INFO]  snarkJS:   WITNESS CHECK
[INFO]  snarkJS:   Curve:          bn128
[INFO]  snarkJS:   Vars (wires):   628
[INFO]  snarkJS:   Outputs:        0
[INFO]  snarkJS:   Public Inputs:  0
[INFO]  snarkJS:   Private Inputs: 4
[INFO]  snarkJS:   Labels:         1873
[INFO]  snarkJS:   Constraints:    624
[INFO]  snarkJS:   Custom Gates:   false
[INFO]  snarkJS: ----------------------------
[INFO]  snarkJS: > Checking witness correctness
[INFO]  snarkJS: WITNESS IS CORRECT
现在已经完成：

- 功能完整的Poseidon2电路实现

- 有效的见证生成能力

- 可验证的约束系统
