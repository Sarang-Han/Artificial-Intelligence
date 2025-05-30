# LSTM

```
==========================================================================================
Layer (type:depth-idx)                   Param #                   Mult-Adds
==========================================================================================
Encoder                                  --                        --
├─Embedding: 1-1                         4,096,000                 4,096,000
├─Dropout: 1-2                           --                        --
├─LSTM: 1-3                              25,190,400                755,712,000
==========================================================================================
Total params: 29,286,400
Trainable params: 29,286,400
Non-trainable params: 0
Total mult-adds (Units.MEGABYTES): 759.81
==========================================================================================
Input size (MB): 0.00
Forward/backward pass size (MB): 0.49
Params size (MB): 117.15
Estimated Total Size (MB): 117.64
==========================================================================================
==========================================================================================
Layer (type:depth-idx)                   Param #                   Mult-Adds
==========================================================================================
Decoder                                  --                        --
├─Embedding: 1-1                         4,096,000                 4,096,000
├─Dropout: 1-2                           --                        --
├─LSTM: 1-3                              25,190,400                730,521,600
├─Dropout: 1-4                           --                        --
├─Linear: 1-5                            4,100,000                 4,100,000
==========================================================================================
Total params: 33,386,400
Trainable params: 33,386,400
Non-trainable params: 0
Total mult-adds (Units.MEGABYTES): 738.72
==========================================================================================
Input size (MB): 0.03
Forward/backward pass size (MB): 1.40
Params size (MB): 133.55
Estimated Total Size (MB): 134.97
==========================================================================================
```


# LSTM with Attention

```
==========================================================================================
Layer (type:depth-idx)                   Param #                   Mult-Adds
==========================================================================================
Encoder                                  --                        --
├─Embedding: 1-1                         4,096,000                 4,096,000
├─Dropout: 1-2                           --                        --
├─LSTM: 1-3                              25,190,400                755,712,000
==========================================================================================
Total params: 29,286,400
Trainable params: 29,286,400
Non-trainable params: 0
Total mult-adds (Units.MEGABYTES): 759.81
==========================================================================================
Input size (MB): 0.00
Forward/backward pass size (MB): 0.49
Params size (MB): 117.15
Estimated Total Size (MB): 117.64
==========================================================================================
==========================================================================================
Layer (type:depth-idx)                   Param #                   Mult-Adds
==========================================================================================
AttnDecoder                              --                        --
├─Embedding: 1-1                         4,096,000                 4,096,000
├─Dropout: 1-2                           --                        --
├─LSTM: 1-3                              25,190,400                25,190,400
├─Attention: 1-4                         --                        --
│    └─Linear: 2-1                       1,048,576                 1,048,576
├─LSTM: 1-5                              (recursive)               25,190,400
├─Attention: 1-6                         (recursive)               --
│    └─Linear: 2-2                       (recursive)               1,048,576
├─LSTM: 1-7                              (recursive)               25,190,400
├─Attention: 1-8                         (recursive)               --
│    └─Linear: 2-3                       (recursive)               1,048,576
├─LSTM: 1-9                              (recursive)               25,190,400
├─Attention: 1-10                        (recursive)               --
│    └─Linear: 2-4                       (recursive)               1,048,576
├─LSTM: 1-11                             (recursive)               25,190,400
├─Attention: 1-12                        (recursive)               --
│    └─Linear: 2-5                       (recursive)               1,048,576
├─LSTM: 1-13                             (recursive)               25,190,400
├─Attention: 1-14                        (recursive)               --
│    └─Linear: 2-6                       (recursive)               1,048,576
├─LSTM: 1-15                             (recursive)               25,190,400
├─Attention: 1-16                        (recursive)               --
│    └─Linear: 2-7                       (recursive)               1,048,576
├─LSTM: 1-17                             (recursive)               25,190,400
├─Attention: 1-18                        (recursive)               --
│    └─Linear: 2-8                       (recursive)               1,048,576
├─LSTM: 1-19                             (recursive)               25,190,400
├─Attention: 1-20                        (recursive)               --
│    └─Linear: 2-9                       (recursive)               1,048,576
├─LSTM: 1-21                             (recursive)               25,190,400
├─Attention: 1-22                        (recursive)               --
│    └─Linear: 2-10                      (recursive)               1,048,576
├─LSTM: 1-23                             (recursive)               25,190,400
├─Attention: 1-24                        (recursive)               --
│    └─Linear: 2-11                      (recursive)               1,048,576
├─LSTM: 1-25                             (recursive)               25,190,400
├─Attention: 1-26                        (recursive)               --
│    └─Linear: 2-12                      (recursive)               1,048,576
├─LSTM: 1-27                             (recursive)               25,190,400
├─Attention: 1-28                        (recursive)               --
│    └─Linear: 2-13                      (recursive)               1,048,576
├─LSTM: 1-29                             (recursive)               25,190,400
├─Attention: 1-30                        (recursive)               --
│    └─Linear: 2-14                      (recursive)               1,048,576
├─LSTM: 1-31                             (recursive)               25,190,400
├─Attention: 1-32                        (recursive)               --
│    └─Linear: 2-15                      (recursive)               1,048,576
├─LSTM: 1-33                             (recursive)               25,190,400
├─Attention: 1-34                        (recursive)               --
│    └─Linear: 2-16                      (recursive)               1,048,576
├─LSTM: 1-35                             (recursive)               25,190,400
├─Attention: 1-36                        (recursive)               --
│    └─Linear: 2-17                      (recursive)               1,048,576
├─LSTM: 1-37                             (recursive)               25,190,400
├─Attention: 1-38                        (recursive)               --
│    └─Linear: 2-18                      (recursive)               1,048,576
├─LSTM: 1-39                             (recursive)               25,190,400
├─Attention: 1-40                        (recursive)               --
│    └─Linear: 2-19                      (recursive)               1,048,576
├─LSTM: 1-41                             (recursive)               25,190,400
├─Attention: 1-42                        (recursive)               --
│    └─Linear: 2-20                      (recursive)               1,048,576
├─LSTM: 1-43                             (recursive)               25,190,400
├─Attention: 1-44                        (recursive)               --
│    └─Linear: 2-21                      (recursive)               1,048,576
├─LSTM: 1-45                             (recursive)               25,190,400
├─Attention: 1-46                        (recursive)               --
│    └─Linear: 2-22                      (recursive)               1,048,576
├─LSTM: 1-47                             (recursive)               25,190,400
├─Attention: 1-48                        (recursive)               --
│    └─Linear: 2-23                      (recursive)               1,048,576
├─LSTM: 1-49                             (recursive)               25,190,400
├─Attention: 1-50                        (recursive)               --
│    └─Linear: 2-24                      (recursive)               1,048,576
├─LSTM: 1-51                             (recursive)               25,190,400
├─Attention: 1-52                        (recursive)               --
│    └─Linear: 2-25                      (recursive)               1,048,576
├─LSTM: 1-53                             (recursive)               25,190,400
├─Attention: 1-54                        (recursive)               --
│    └─Linear: 2-26                      (recursive)               1,048,576
├─LSTM: 1-55                             (recursive)               25,190,400
├─Attention: 1-56                        (recursive)               --
│    └─Linear: 2-27                      (recursive)               1,048,576
├─LSTM: 1-57                             (recursive)               25,190,400
├─Attention: 1-58                        (recursive)               --
│    └─Linear: 2-28                      (recursive)               1,048,576
├─LSTM: 1-59                             (recursive)               25,190,400
├─Attention: 1-60                        (recursive)               --
│    └─Linear: 2-29                      (recursive)               1,048,576
├─Dropout: 1-61                          --                        --
├─Linear: 1-62                           4,100,000                 4,100,000
==========================================================================================
Total params: 34,434,976
Trainable params: 34,434,976
Non-trainable params: 0
Total mult-adds (Units.MEGABYTES): 769.13
==========================================================================================
Input size (MB): 0.14
Forward/backward pass size (MB): 1.64
Params size (MB): 137.74
Estimated Total Size (MB): 139.52
==========================================================================================
```


# Transformer

```
===============================================================================================
Layer (type:depth-idx)                        Param #                   Mult-Adds
===============================================================================================
TransformerModel                              --                        --
├─Embedding: 1-1                              2,048,000                 2,048,000
├─PositionalEncoding: 1-2                     --                        --
├─Embedding: 1-3                              (recursive)               2,048,000
├─PositionalEncoding: 1-4                     --                        --
├─TransformerEncoder: 1-5                     --                        --
│    └─ModuleList: 2-1                        --                        --
│    │    └─TransformerEncoderLayer: 3-1      3,152,384                 2,101,760
│    │    └─TransformerEncoderLayer: 3-2      3,152,384                 2,101,760
│    │    └─TransformerEncoderLayer: 3-3      3,152,384                 2,101,760
│    │    └─TransformerEncoderLayer: 3-4      3,152,384                 2,101,760
├─TransformerDecoder: 1-6                     --                        --
│    └─ModuleList: 2-2                        --                        --
│    │    └─TransformerDecoderLayer: 3-5      4,204,032                 2,102,784
│    │    └─TransformerDecoderLayer: 3-6      4,204,032                 2,102,784
│    │    └─TransformerDecoderLayer: 3-7      4,204,032                 2,102,784
│    │    └─TransformerDecoderLayer: 3-8      4,204,032                 2,102,784
├─Linear: 1-7                                 2,052,000                 2,052,000
===============================================================================================
Total params: 33,525,664
Trainable params: 33,525,664
Non-trainable params: 0
Total mult-adds (Units.MEGABYTES): 22.97
===============================================================================================
Input size (MB): 0.00
Forward/backward pass size (MB): 8.41
Params size (MB): 83.67
Estimated Total Size (MB): 92.08
===============================================================================================
```
