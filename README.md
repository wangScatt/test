#打击板通信协议#
----------

##指令格式##

|字头 |长度 |                            数据                                  |CRC(校验和)|
|:--: |:--:|                  :-----------------------:                      | :------: |
|0xAB | ** |                          data                                   |   CRC    |

----------

##移动指令##

|字头 |长度 |                            数据                                  |CRC(校验和)|
|:--: |:--:|              :-----------------------:                          | :------: |
|0xAB |0x0C|2byte(CH1)+2byte(CH2)+2byte(CH3)+2byte(CH4)+2byte(CH5)+2byte(CH6)|   CRC    |

**描述：**
数据中分别对应遥控器6个通道的值，每个通道的采样返回值为1000~2000，中点値为1500。

----------

##解析函数##

**描述：**
此函数为解析接收数据的参考示例、

**输入：**
`*recevie_data`：已接收到的数据所在数组的首地址，`*data`：保存解析后数据的数组的首地址

**输出：**
返回'0'表示数据接收错误，返回'1'表示数据正确且正常解析。

    byte Decode(uinr8_t *recevie_data,uint16_t *data)
    {
		uint8_t check_sum = 0;
		for(uint8_t i = 0;i < (recevie_data[1] + 3);i++)
		{
			check_sum += recevie_data[i];
		}
		if(check_sum == recevie_data[14]) 
		{
			data[0] = (recevie_data[2] << 8) | recevie_data[3];
            data[1] = (recevie_data[4] << 8) | recevie_data[5];
			data[2] = (recevie_data[6] << 8) | recevie_data[7];
			data[3] = (recevie_data[8] << 8) | recevie_data[9];
			data[4] = (recevie_data[10] << 8) | recevie_data[11];
			data[5] = (recevie_data[12] << 8) | recevie_data[13];
			return 1;
		}
		else
		{
			return 0;
		}
	}		