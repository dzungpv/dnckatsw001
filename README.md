## Ligh Switch base on Samplight Z-Stack Home 1.2.2a for CC2530, 2538 SOC

### Step 1:

Download Z-Stack Home 1.2.2a it is free but require to regiter http://www.ti.com/tool/Z-STACK-ARCHIVE

### Step 2:
Edit file C:\Texas Instruments\Z-Stack Home 1.2.2a.44539\Projects\zstack\HomeAutomation\SampleLight\Source\zcl_samplelight_data.c with those infor:
```
const uint8 zclSampleLight_ManufacturerName[] = { 6, 'D','N','C','K','A','T' };
const uint8 zclSampleLight_ModelId[] = { 11, 'D','N','C','K','A','T','_','S','0','0','1' };
const uint8 zclSampleLight_DateCode[] = { 16, '2','0','1','8','0','6','2','3',' ',' ',' ',' ',' ',' ',' ',' ' };
```

### Step 3:
Add report state code to zclSampleLight_LcdDisplayUpdate:
```
  //report state
  zclSampleLightSeqNumState++;
  zclReportCmd_t rptcmd;
  rptcmd.numAttr = 1;
  rptcmd.attrList[0].attrID = ATTRID_ON_OFF;
  rptcmd.attrList[0].dataType = ZCL_DATATYPE_BOOLEAN;
  rptcmd.attrList[0].attrData = (void *)(&zclSampleLight_OnOff);

  // Set destination address to indirect
  zclSampleLight_DstAddr.addrMode = (afAddrMode_t)Addr16Bit;
  zclSampleLight_DstAddr.addr.shortAddr = 0;
  zclSampleLight_DstAddr.endPoint=1;
  zcl_SendReportCmd(SAMPLELIGHT_ENDPOINT,&zclSampleLight_DstAddr, ZCL_CLUSTER_ID_GEN_ON_OFF, &rptcmd, ZCL_FRAME_SERVER_CLIENT_DIR, true, zclSampleLightSeqNumState );
  ```
  
### Step 4:
Disable ZCL_EZMODE and HOLD_AUTO_START build tag by add 'x' before each.
