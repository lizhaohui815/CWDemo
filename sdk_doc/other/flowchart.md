



```graph

graph TD;
    A[AsyncParseSFZ api=new AsyncParseSFZ]-->B[api.openIDCardSerialPort];
    B-->C[api.readSFZ];
    B-->D[api.setOnReadSFZListener];
    C-->E[api.closeIDCardSerialPort];

```


```graph

graph TD;
    A[SoftDecodingAPI api=new SoftDecodingAPI]-->B[api.openBarCodeReceiver];
    B-->C[api.scan];
    B-->D[api.setOnBarCodeDataListener];
    C-->E[api.closeBarCodeReceiver];

```


```graph

graph TD;
    A[Start]-->B[BeiDouAPI.getInstance];
    B-->C[api.open];
    C-->D[api.XTZJ];
    C-->DD[api.ICJC]
    C-->DDD[api.DWSQ]
    C-->DDDD[api.TXSQ]
    D-->E[api.close];
    DD-->E[api.close];
    DDD-->E[api.close];
    DDDD-->E[api.close];
    E-->AA[End]

```

```graph

graph TD;
    A[UHFHXAPI api = new UHFHXAPI]-->B[api.openHXUHFSerialPort];
    B-->C[api.open];
    C-->D[api.startAutoRead2C];
    C-->E[......];
    C-->F[api.startAutoRead2A];
    D-->G[api.close]
    E-->G
    F-->G
    G-->H[api.closeHXUHFSerialPort]

```




- #### 打开并初始化指纹模块

```graph

graph TD;
    start[Start]-->AA[USBFingerManager.getInstance.setDelayMs]
    AA-->A{USBFingerManager.getInstance.openUSB}
    A--Success-->B[SyOTG_Key msyUsbKey = new SyOTG_Key];
    A--Failure-->C[Error Check Machine];
    B-->E[msyUsbKey.SyOpen]

```

- #### 采集指纹

```graph

graph TD;
    start[Start]-->A[等待指纹录入]
    A-->B[录入:msyUsbKey.SyEnroll]
    B-->C{第一次录入}
    C--Yes-->A
    C--No-->D{msyUsbKey.SyEnroll == PS_OK}
    D--Yes-->E[上传特征:msyUsbKey.SyUpChar]
    D--No-->F[指纹采集失败]
    E-->End

```
- #### 搜索指纹

```graph

graph TD;
    start[Start]-->A[等待指纹录入]
    A-->B{搜索:msyUsbKey.SySearch == PS_OK}
    B--Yes-->C[搜索成功,返回指纹id]
    B--No-->D[搜索指纹失败]
    C-->End
```






- ##### 打开并初始化指纹模块

```graph

graph TD;
    start[Start]-->AA[USBFingerManager.getInstance.setDelayMs]
    AA-->A{USBFingerManager.getInstance.openUSB}
    A--Success-->B[ID_Fpr mLiveScan = new ID_Fpr];
    A--Failure-->C[Error Check Machine];
    B-->E[mLiveScan.LIVESCAN_Init]

```

- ##### 采集指纹图像

```graph

graph TD;
    start[Start]-->A[mLiveScan.LIVESCAN_GetFPBmpData]

```




- ##### 采集指纹

```graph

graph TD;
    S[Start]-->A[mLiveScan.LIVESCAN_GetFPBmpData]
    A-->B[mLiveScan.LIVESCAN_GetFPRawData]
    B-->D[mLiveScan.LIVESCAN_FPRawDataToBmp]
    D-->E[mLiveScan.LIVESCAN_GetQualityScore]
    E-->F{bScore >= THRESHOLD}
    F--Yes-->H[mLiveScan.LIVESCAN_FeatureExtract]
    F--No-->S
    H-->End

```

- ##### 1:1比对

```graph

graph TD;
    S[Start]-->A[mLiveScan.LIVESCAN_GetFPBmpData]
    A-->B[mLiveScan.LIVESCAN_GetFPRawData]
    B-->D[mLiveScan.LIVESCAN_FPRawDataToBmp]
    D-->E[mLiveScan.LIVESCAN_GetQualityScore]
    E-->F{bScore >= THRESHOLD}
    F--Yes-->H[mLiveScan.LIVESCAN_FeatureExtract]
    F--No-->S
    H-->I{mLiveScan.LIVESCAN_FeatureMatch}
    I--Yes-->J[Success]
    I--No-->K[Failure]
    K-->End
    J-->End
```


- ##### 1:N比对

```graph

graph TD;
    S[Start]-->A[mLiveScan.LIVESCAN_GetFPBmpData]
    A-->B[mLiveScan.LIVESCAN_GetFPRawData]
    B-->D[mLiveScan.LIVESCAN_FPRawDataToBmp]
    D-->E[mLiveScan.LIVESCAN_GetQualityScore]
    E-->F{bScore >= THRESHOLD}
    F--Yes-->H[mLiveScan.LIVESCAN_FeatureExtract]
    F--No-->S
    H-->I{mLiveScan.LIVESCAN_FeatureSearch}
    I--Yes-->J[Success]
    I--No-->K[Failure]
    K-->End
    J-->End

```