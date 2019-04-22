[����](��̫��ͨ��Э��SDK "wikilink")

# ����

*   API�ṩ���Ѻõ��û��ӿڣ�����������INNFOSִ�������д��ڻ�����̫��ͨ�ţ��Ƽ������ܣ���ͬʱ�Զ��ִ��������ָ����߻�ȡִ����״̬�Ͳ�����Ϣ
*   ������νӴ�API���û������Ķ�SDK�е�examples��

# SDK���ؼ�Ŀ¼˵��

# ����

*   ���ʸ�����[download link](https://github.com/innfos/ActuatorController_SDK.git)����SDK����ļ�����ֱ��ִ����������

```sh
    $ git clone https://github.com/innfos/ActuatorController_SDK.git
```
    
# API����ĵ�

*   ע�������Լ����˵���ĵ������[document link](http://innfos.com/doc/index.html)

# ִ��������

![���ڰ�����ͼ](ִ��������ͼ  ����.png "fig:���ڰ�����ͼ")

����ͼ���Ӻ��豸���ٽ�ͨ��Դ��

���棺�������°β��Ͻ���������������������豸��

* * *

![��̫��������ͼ](2222222(1).gif "fig:��̫��������ͼ")

���϶�ͼΪINNFOSִ�����ļ��ֲ�ͬ���ӷ�ʽ��

���棺�������°β��Ͻ���������������������豸��

# �������ú�ʾ�������������

## linuxƽ̨

### ��������

* cmake ��װ�����ն���������
    <pre>$ sudo apt-get install cmake</pre>

* ip ��ַ���ã����ն����� ifconfig���鿴��������

[none|thumb|400px](file:020.png "wikilink") [none|thumb|400px](file:021.png "wikilink")

* ʾ��������������������enp0s25,��������
```sh
sudo ifconfig enp0s25 static 192.168.1.111
```
������ɺ�����ifconfig���ɿ������óɹ����ip��ַ

### ʾ���������

* ���ն˽��롭\exampleĿ¼����Ŀ¼����CMakeLists.txt��

		$ cmake CMakeLists.txt
		$ make

*   ��������ִ����ɺ��ڸ�Ŀ¼�»�����һ��bin�ļ��У���Ŀ¼��������ɵ�ʾ������
*   ȷ��ִ������ȷ���Ӳ������Ժ�ִ�������л�ɫָʾ����˸����ʱ���Բ���ʾ�����롣

### ʾ���������

*   ȷ��ִ������ȷ���Ӳ������Ժ�ִ�������л�ɫָʾ����˸����ʱ���Բ���ʾ������

#### ���������ӵ�ִ����

*   ���ն˲�����binĿ¼����������

```sh
	$./lookupActuators -e
```
*   �˴��ڻ���ʾ��ǰ�����ӵ�ִ��������������ctrl+c��������

    * * *

    [none|thumb|400px](file:022.png "wikilink")

**����˵��**

*   ���ݲ�ͬ�Ĳ���ѡ��ͬ��ͨ�ŷ�ʽ��Ĭ��Ϊ��̫��ͨ�š�ע�⣺�����ȳ�ʼ�������������ܽ�����������

```cpp
    //��ʼ��������
   if(strcmp(argv[1],"-s")==0)
        ActuatorController::initController(Actuator::Via_Serialport);
   elseif(strcmp(argv[1],"-e")==0)
        ActuatorController::initController();
```
*   ������������źţ�����û������ɹ��Ժ󣬻ᴥ�����źţ����ݲ�ͬ��operationType��������Ӧ�����������л����Զ�ʶ����ɺ󣬴�ӡ��ʶ�𵽵�ִ����������

```cpp
    //�����������Ĳ����ź�
   int nOperationConnection = pController->m_sOperationFinished->s_Connect([=](uint8_t nDeviceId,uint8_t operationType){
       switch (operationType) {
       case Actuator::Recognize_Finished://�Զ�ʶ�����
           if(pController->hasAvailableActuator())
            {
                vector<uint8_t> idArray = pController->getActuatorIdArray();
                cout <<"Number of connected actuators:" << idArray.size() << endl;
            }
           break;
       default:
           break;
        }
    });
```
*   ������Ӧ�ñ���Ҫ���������źţ��Ա���ִ�����ڲ���������ʱ��ʱ�յ�������������Ӧ����nDeviceIdΪ0ʱ����������ض���ִ����������δ���ӵĴ���

```cpp
    //���������ź�
   int nErrorConnection = pController->m_sError->s_Connect([=](uint8_t nDeviceId,uint16_t nErrorType,string errorInfo){
       if(nDeviceId==0)
        {
            cout <<"Error: " << (int)nErrorType <<" " << errorInfo << endl;
        }
       else
        {
            cout <<"Actuator " << (int)nDeviceId <<" " <<"error " << (int)nErrorType <<" " << errorInfo << endl;
        }
    });
```

*   �����ñ�Ҫ���ź��Ժ󣬿��Խ�����Ӧ��������һ���Ĳ�������ʶ�������ӵ�ִ������

```cpp
    //�Զ�ʶ��������ִ����
    pController->autoRecoginze();
```

*   �¼�ѭ���Ǳ�֤sdk�ڲ�ͨѶ���еı�Ҫ���裬���Ҫ��֤�¼�ѭ������������sdk���ܴ��������źš�

```cpp
    //ִ�п������¼�ѭ��
   while (!bExit)
    {
        ActuatorController::processEvents();
    }
```

*   ����ڳ������ǰ���Ͽ��������źŵĹ���

```cpp
    //�Ͽ��ź�����
    pController->m_sOperationFinished->s_Disconnect(nOperationConnection);
    pController->m_sError->s_Disconnect(nErrorConnection);

```

#### ���ִ����״̬

*   ���նˣ�����example/binĿ¼����������

```sh
    $./monitorActuator -e
```

*   ����Actuator IDΪִ����id,attribute IDΪ����ִ��������Id��attribute valueΪ��
    Ӧ������ֵ������ctrl+c��������

    [none|thumb|400px](file:023.png "wikilink")

    **����˵��**

*   �Զ�ʶ��ɹ����Զ���������ִ������ÿ��ִ���������ɹ��󶼻ᴥ��Actuator::Launch_Finished�źţ�������ִ�����������Ժ󣬿�ʼ�Զ�ˢ�£���ȡִ�������ݡ�

```cpp
   int nLaunchedActuatorCnt =0;
   //�����������Ĳ����ź�
   int nOperationConnection = pController->m_sOperationFinished->s_Connect([&amp;](uint8_t nDeviceId,uint8_t operationType){
       switch (operationType) {
       case Actuator::Recognize_Finished://�Զ�ʶ�����
           if(pController->hasAvailableActuator())
            {
                vector<uint8_t> idArray = pController->getActuatorIdArray();
            cout <<"Number of connected actuators:" << idArray.size() << endl;
               for (uint8_t id: idArray) {
                   if(pController->getActuatorAttribute(id,Actuator::ACTUATOR_SWITCH)==Actuator::ACTUATOR_SWITCH_OFF)
                    {//���ִ�������ڹػ�״̬������ִ����
                        pController->launchActuator(id);
                    }
                   else
                    {
                        ++ nLaunchedActuatorCnt;
                       if(nLaunchedActuatorCnt == pController->getActuatorIdArray().size())//����ִ���������������
                        {
                            autoRefresh();
                        }
                    }
                }
            }
           break;
       case Actuator::Launch_Finished:
           if(++nLaunchedActuatorCnt == pController->getActuatorIdArray().size())//����ִ���������������
            {
                autoRefresh();
            }
           break;
       default:
           break;
        }
    });
```

*   Ϊ�˼��ִ���������Ա仯��������ź�m_sActuatorAttrChanged�����û������ȡִ���������Ժ󣬳ɹ����ػᴥ�����ź�
    //�������������Ƶ�ִ�������Ա仯�ź�
	
```cpp
   int nAttrConnection =pController->m_sActuatorAttrChanged->s_Connect([=](uint8_t nDeviceId,uint8_t nAttrId,double value){
        cout <<"Actuator ID: " << (int)nDeviceId << endl;
        cout <<"atribute ID: " << (int)nAttrId << endl;
        cout <<"atribute value: " << value << endl;
        cout <<"----------------------------"<<endl;
    });
```

#### ����ִ����

*   ���նˣ�����example/binĿ¼����������

```sh
   	 $./operateActuator -e
```

    
	
[none|thumb|400px](file:024.png "wikilink")

*   ��ʾִ�����Ѿ��ҵ�����������l 0����������������������ӵ�ִ��������������ɹ���ִ����������ɫָʾ����˸����ʾ�Ѿ������ɹ����ն˴�������ʾ

    [none|thumb|400px](file:025.png "wikilink")

*   ��ʱ�ɼ���ִ������Ӧģʽ���������� a 6���Լ���profile positionģʽ��������p 5��

    ִ������ת����5Ȧ��λ�ã�����a 7���Լ���profile velocityģʽ��������v 500�� ִ��������500RPM���ٶ�ת����ֹͣת������v 0,������a 1���Լ������ģʽ�������� c 0.6��ִ�������Ժ㶨0.6A�ĵ���ת�������ִ��������������������ת��һ��ִ�������� ����ctrl+c�Ժ���ctrl+d����������Ϊ�ж��̵߳ȴ��������룩 [none|thumb|400px](file:026.png "wikilink")

    **����˵��**

*   �ɹ�����ִ�����󣬿ɶ�ִ�������в�����getActuatorIdArray�ɻ�ȡ����ִ�����Ķ�id���û�����ָ����������id�����в�����ִ�������ٶȡ�������λ�õȶ���ģʽ��Actuator::ActuatorMode���������ȼ����Ӧ��ģʽ���ܽ�����Ӧ������

```cpp
   vector<uint8_t> idArray = controllerInst->getActuatorIdArray();
   switch (directive)
    {
   case'a'://����ִ����ָ��ģʽ��ָ���ʽ��a ģʽid��Actuator::ActuatorMode��
        controllerInst->activeActuatorMode(idArray, Actuator::ActuatorMode((int)value));
       break;
   case'p'://ָ��ִ����λ�ã�ָ���ʽ��p Ȧ����-127��127��
       for (int i =0; i < idArray.size(); ++i)
        {
            controllerInst->setPosition(idArray.at(i), value);
        }
       break;
   case'c'://ָ��ִ����������ָ���ʽ��c ����ֵ��A��
       for (int i =0; i < idArray.size(); ++i)
        {
            controllerInst->setCurrent(idArray.at(i), value);
        }
       break;
   case'v'://ָ��ִ�����ٶȣ�ָ���ʽ��v �ٶ�ֵ��RPM��
       for (int i =0; i < idArray.size(); ++i)
        {
            controllerInst->setVelocity(idArray.at(i), value);
        }
       break;
   case'l'://����ָ��ִ������ָ���ʽ��l ִ����id��idΪ0��������ִ������
       if(uint8_t(value)==0)
        {
            controllerInst->launchAllActuators();
        }
       else
        {
            controllerInst->launchActuator(uint8_t(value));
        }
       //cout << "launch"<<endl;
       break;
   case's'://�ر�ָ��ִ������ָ���ʽ��l ִ����id��idΪ0��������ִ������
       if(uint8_t(value)==0)
        {
            controllerInst->closeAllActuators();
        }
       else
        {
            controllerInst->closeActuator(uint8_t(value));
        }
       //cout << "close"<<endl;
       break;
   default:
       break;
    }
```

#### ��������������

*   ���նˣ�����example/binĿ¼����������

```sh
    $./tuneActuator -e
```

*   ��ʾ�������Զ�����ִ��������λ�û��������Ϊ3000RPM,�ٶȻ��ĵ���������Ϊ16.5A,

    ���ʹ��profile positionģʽת��ִ������ִ����������ٶȲ��ᳬ��3000RPM;��� ʹ��profile velocityģʽת��ִ������ִ�������������ᳬ��16.5A������ctrl+c�������� [none|thumb|400px](file:027.png "wikilink") **����˵��**

*   ��ʾ�������Զ�����ִ�����������ɹ���ɵ���ִ�������ԣ��ٶȻ���������ɵ����ٶȻ��µ�ִ����Ť�أ�λ�û��ٶ�����ɵ���λ�û����ٶȵĴ�С�����SCA����ʹ��˵����
    //ִ�������Ե���,�����ɹ����ᴥ��ִ�������Ա仯�ź�
	
```cpp
   void tuneActuator()
    {
        ActuatorController * pController = ActuatorController::getInstance();
        vector<uint8_t> idArray = pController->getActuatorIdArray();
       for (uint8_t id: idArray) {
           //����ִ�����ٶȻ���С�������
            pController->setMinOutputCurrent(id,-10);
           //����ִ�����ٶȻ����������
            pController->setMaxOutputCurrent(id,10);
           //����ִ����λ�û���С�ٶ����
            pController->setMinOutputVelocity(id,-2000);
           //����ִ����λ�û�����ٶ���������ֵҪ������Сֵ
            pController->setMaxOutputVelocity(id,2000);
           //����ִ����Mode_Profile_Pos������ٶȣ�RPM��
            pController->setActuatorAttribute(id,Actuator::PROFILE_POS_MAX_SPEED,1000);
        }
    }
```

#### ִ��������

*   ���նˣ�����example/binĿ¼����������

```sh
$./homingActuator -e
```


*   ��ʾ�Ѿ���ִ������ǰλ������Ϊ��λ����Χ��-9.5R��9.5R�����ҿ�����λ�����ƣ����profile positionģʽ�£�����˷�Χ֮���λ�ã�ִ��������ת��������ctrl+c�������� [none|thumb|400px](file:028.png "wikilink")

**����˵��**

*   ִ�����Զ�������ɺ�setHomingPosition�Ὣ��ǰλ��getActuatorAttribute(id,Actuator::ACTUAL_POSITION)���ó�0λ��</br>setMaxPosLimit��setMinPosLimit������������С��λ�����ƣ�setActuatorAttribute(id,Actuator::POS_OFFSET,0.5)���ü���ƫ�ơ�
    //ִ����0λ����λ����
	
```cpp
   void setActuatorLimitation()
    {
        ActuatorController * pController = ActuatorController::getInstance();
        vector<uint8_t> idArray = pController->getActuatorIdArray();
       for (uint8_t id : idArray) {
           //��ִ������ǰλ�ñ��0λ��������С�����λ�÷ֱ�����Ϊ-10,10,ƫ������Ϊ0.5��ִ�������˶���Χ��ɣ�-9.5,9.5��
            pController->setHomingPosition(id,pController->getActuatorAttribute(id,Actuator::ACTUAL_POSITION));
            pController->setMinPosLimit(id,-10);
            pController->setMaxPosLimit(id,10);
            pController->setActuatorAttribute(id,Actuator::POS_OFFSET,0.5);
        }
        bSetLimitation =true;
    }
```

*   ����������ɣ�������������򣬹ػ��Ժ�������ý�ȫ������

```cpp
pController->saveAllParams(nDeviceId);
```

#### ִ��������id

*   ���ն˲�����binĿ¼����������

```sh
$./longIdAndByteId -e
```

*   ���Խ��г���id�Ļ�ȡ�Լ��໥ת�������ҿ���ͨ����id��ȡͨ��ip��ַ��

**����˵��**

*   �źű�����L��β�ı�ʶ���źŹ�������ִ������id������ͨ����id����ͨ�š�����id���������ڳ�id������ͨ�ŵ�ַ�Ͷ�id�����ҿ����໥ת���������ͬ��ip��ַ������ͬ��id��ִ��������idת����id�����ת������һ��ip��ַ�µ�һ����id����
    //������������longId�����ź�

```cpp
   int nOperationConnection = pController->m_sOperationFinishedL->s_Connect([&amp;](uint64_t nDeviceId,uint8_t operationType){
       switch (operationType) {
       case Actuator::Recognize_Finished://�Զ�ʶ�����
           if(pController->hasAvailableActuator())
            {
               //��ȡlongId����
                vector<uint64_t> longIdArray = pController->getActuatorLongIdArray();
               //��ȡbyteid����
                vector<uint8_t> idArray = pController->getActuatorIdArray();
               for (uint64_t id: longIdArray) {
               //��ȡ��id�е�ͨ��ip��ַ
                    cout <<"Communication IP is " << pController->toString(id) << endl;
                   //longIdת����byteId
                    cout <<"Long id " << id <<" convert to byte id " << (int)pController->toByteId(id) << endl;
                }
               for(uint8_t id : idArray)
                {
                   //byteIdת����longId
                    cout <<"Byte id " << (int)id <<" convert to long id " << pController->toLongId(id) << endl;
                }
            }
           break;
       default:
           break;
        }
    });
```

#### ͬ����Ӧ

* ���ն˲�����binĿ¼����������

```sh
$./feedback_sync -e
```

*   ������Ӧ�źţ��ڻص��н��в��������첽��Ӧ������������ǰ����ͬ����Ӧ����������ǰ����ֱ��sdk���ؽ������Ƚ϶��ԣ�ͬ����Ӧ�÷��򵥵���Ч��ƫ�ͣ���Ϊ��Ҫ�ȴ�ִ������Ӧ������ִ�������ֲ���û��ͬ����Ӧ����������λ�á��ٶȡ������ȣ�,�����Ч��Ҫ��Ƚϸߣ��Ƽ�ʹ���첽��Ӧ��

**����˵��**

*   getActuatorAttributeWithACK��setActuatorAttributeWithACK��Ŀǰsdk�ṩ������ͬ����Ӧ�Ľӿڣ���Ӧ��getActuatorAttribute��setActuatorAttribute������ͬ����ȡ��������ִ���������ԣ�����getActuatorAttributeWithACK��setActuatorAttributeWithACK��������ǰ����ֱ���н�����أ����ܳɹ�����ʧ�ܣ���

```cpp
ActuatorController * pController = ActuatorController::getInstance();
    vector<uint8_t> idArray = pController->getActuatorIdArray();
   for (uint8_t id: idArray) {
       bool bSuccess =false;
       //��ȡ��ǰ���������ȴ����أ������ȡʧ�� bSuccess��ֵΪfalse
       double current = pController->getActuatorAttributeWithACK(id,Actuator::ACTUAL_CURRENT,&amp;bSuccess);
       if(bSuccess)
            cout <<"current is " << current << endl;
       //�����ٶȻ���������������ȴ����أ��������ʧ�ܣ�bSuccess��ֵΪfalse���������ٶȡ�λ�á���������ʹ�ô˽ӿڣ�
        bSuccess = pController->setActuatorAttributeWithACK(id,Actuator::VEL_OUTPUT_LIMITATION_MAXIMUM,10);
       if(bSuccess)
            cout <<"Set VEL_OUTPUT_LIMITATION_MAXIMUM to 10A" << endl;
    }
```

## windowsƽ̨

### ��������

*   SDK��Ҫwin7 sp1���ϵ�64λwindows����ϵͳ
*   ���а�װtools�ļ����е�vcredist_x64.exe����װvs2015����ʱ��
*   ��װcmake����Cmake����https://cmake.org/download/�������°汾Cmake��װ
*   ������Է���ǽ�ѿ������򿪿�����壬ѡ��ϵͳ�Ͱ�ȫ����Windows Defender ����ǽ��������߼����ã������վ����

[none|thumb|400px](file:001.png "wikilink")

*   �½�����

[none|thumb|400px](file:002.png "wikilink")

*   ѡ��˿ڣ�Ȼ������һ��

[none|thumb|400px](file:003.png "wikilink")

*   ѡ��udp����ѡ�����б��ض˿ڣ������һ��

[none|thumb|400px](file:004.png "wikilink")

*   �������ӣ������һ����

[none|thumb|400px](file:005.png "wikilink")

*   Ĭ�Ϲ�ѡ�������ã������һ��

[none|thumb|400px](file:006.png "wikilink")

*   ��д�Զ������֣���ɼ���

[none|thumb|400px](file:007.png "wikilink")

*   ip��ַ���ã�

*   �򿪿�����壬ѡ�������Internet,��ѡ������͹������ģ���ѡ��������������ã��Ҽ�������̫����ѡ������

[none|thumb|400px](file:008.png "wikilink")

*   ѡ��TCP/IPv4,Ȼ��ѡ�����ԣ�

������ͼ��

*   ����ip��ַ�е�192.168.1.119�е�119�����滻��100~200֮�������������������ɵ��ȷ��

[none|thumb|400px](file:009.png "wikilink")

### ʾ���������

*   ����cmake-gui �������ҽ��棺
*   ����Դ��·������Ŀ¼�ṹ�еġ�\example���ڵ�·������Ŀ¼�°�����CMakeLists.txt�ļ�������·�������ж��壬�������ɹ����ļ�����·��������ɺ���Generate��ť�������½���

[none|thumb|400px](file:011.png "wikilink")

*   �����ɫ���ڲ���64λ������������������ǣ�ѡ��64λ��������Ȼ����Finish��ť�����ɳɹ����������Visual Studio�Ĺ����ļ�������Visual Studio�򿪱��롣�������������̣��ڹ���Ŀ¼�»�����һ��binĿ¼��������Debug����Release�ļ��У���Ӧ�ڱ���İ汾������Ŀ¼�ṹ�еġ�\sdk\lib\windows_x64\debug��\sdk\lib\windows_x64\release�е��ļ����Ƶ���Ӧ�汾��bin�����Debug����ReleaseĿ¼�У�˫����Ŀ¼�е�exe�Ϳ���������ʾ�������ˡ�

[none|thumb|400px](file:012.png "wikilink")

### ʾ���������

#### ���������ӵ�ִ����

*   ȷ��ִ������ȷ���Ӳ������Ժ�ִ�������л�ɫָʾ����˸����ʱ���Բ���ʾ�����롣

�������д��ڲ�����binĿ¼����������./lookupActuators.exe -e [none|thumb|400px](file:013.png "wikilink")

[none|thumb|400px](file:014.png "wikilink")

#### ���ִ����״̬

*   �������д��ڲ�����binĿ¼����������./monitorActuator.exe -e

[none|thumb|400px](file:015.png "wikilink")

#### ����ִ����

*   �������д��ڲ�����binĿ¼����������

```sh
./operateActuator.exe -e
```
��ʾִ�����Ѿ��ҵ�����������l 0����������������������ӵ�ִ��������������ɹ���ִ����������ɫָʾ����˸����ʾ�Ѿ������ɹ���cmd����������ʾ

*   ��ʱ�ɼ���ִ������Ӧģʽ���������� a 6���Լ���profile positionģʽ��������p 10��ִ������ת����10Ȧ��λ�ã�����a 7���Լ���profile velocityģʽ��������v 500��ִ��������500RPM���ٶ�ת����ֹͣת������v 0,������a 1���Լ������ģʽ��������c 0.6��ִ�������Ժ㶨0.6A�ĵ���ת�������ִ��������������������ת��һ��ִ������������ctrl+c��������

[none|thumb|400px](file:016.png "wikilink")

[none|thumbb|400px](file:017.png "wikilink")

#### ִ��������������

*   �������д��ڲ�����binĿ¼����������

```sh
./tuneActuator.exe -e
```

*   ��ʾ�������Զ�����ִ��������λ�û��������Ϊ3000RPM,�ٶȻ��ĵ���������Ϊ16.5A,���ʹ��profile positionģʽת��ִ������ִ����������ٶȲ��ᳬ��3000RPM;���ʹ��profile velocityģʽת��ִ������ִ�������������ᳬ��16.5A������ctrl+c��������

[none|thumb|400px](file:018.png "wikilink")

#### ִ��������

*   �������д��ڲ�����binĿ¼����������

```sh
./homingActuator.exe -e
```

*   ��ͼ�����ʾ�Ѿ���ִ������ǰλ������Ϊ��λ����Χ�� -9.5R �� 9.5R�����ҿ�����λ�����ƣ���� profile

position ģʽ�£�����˷�Χ֮���λ�ã�ִ��������ת�������� ctrl+c ��������

[none|thumb|400px](file:LongId_w.png "wikilink")

#### ִ��������id

*   �������д��ڲ�����binĿ¼����������

```sh
./longIdAndByteId.exe -e
```

���Խ��г���id�Ļ�ȡ�Լ��໥ת�������ҿ���ͨ����id��ȡͨ��ip��ַ��

[none|thumb|400px](file:Feedback_w.png "wikilink")

#### ͬ����Ӧ

*   �������д��ڲ�����binĿ¼����������

```sh
./feedback_sync.exe -e
```

*   ����feedback_sync.exe��������Ӧ�źţ��ڻص��н��в��������첽��Ӧ������������ǰ����ͬ����Ӧ����������ǰ����ֱ��sdk���ؽ������Ƚ϶��ԣ�ͬ����Ӧ�÷��򵥵���Ч��ƫ�ͣ���Ϊ��Ҫ�ȴ�ִ������Ӧ������ִ�������ֲ���û��ͬ����Ӧ����������λ�á��ٶȡ������ȣ�,�����Ч��Ҫ��Ƚϸߣ��Ƽ�ʹ���첽��Ӧ��

# SDKʹ��˵��

## ����

*   ��SDK�ṩ����ִ����ͨ�ŵĽӿ�,��ͨ�����ڻ�����̫�����Ѿ����Ӻõ�ִ�������в��ҡ�״̬��ѯ�����Ե������Զ�����ơ����������˽�sdk�������ݺ�ʹ�÷���,��鿴example/src�е���ش���

## ��Ŀ��ʹ��sdk

*   �� sdk ��ѭ c++11 ��׼�������ڹ�����Ŀ֮ǰ��ȷ�ϱ���ѡ��֧�� c++11������ gcc ��ʹ�� -std=c++11�� ;
*   �� sdk ���ɵ���Ŀ�еĻ������裨����Ȳο� example �е� CMakeLists.txt�� :
*   �� sdk/include�� sdk/include/asio ���뵽��Ŀ�İ���Ŀ¼�����ڹ���������еķ��� ;
*   �����ļ�Ŀ¼ sdk/lib/linux_x86_64��windows Ŀ¼Ϊ sdk/lib/debug �� sdk/lib/release�����Ա��ִ���ļ������ӵ�����⣬����֤����ʱ�ܹ�����������⣻
*   ����Ҫ��Ԫ�ؼ��뵽���������У����� CMake �е� target_link_libraries��

## �����ռ�

* ��../sdk/include/actuatordefine.h�����������ռ�Actuator,����ö����sdk�������õ������ͺ�����ֵ��

<table >
<thead><tr><th colspan="2" style=background:PaleTurquoise>����״̬������ִ������CAN������״̬�ж�[ConnectStatus]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>NO_CONNECT,</td><td>������</td></tr>
 <tr><td>CAN_CONNECTED=0x02,</td><td>CANͨ�����ӳɹ�</td></tr>
 <tr><td>ACTUATOR_CONNECTED=0x04,</td><td>ִ�������ӳɹ�</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th  colspan="2" style=background:PaleTurquoise>ͨ��ID,���ڱ�ʶִ����ͼ�����ݵ�ͨ������[Channel_ID]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>channel_1=0,</td><td>ͼ������1ͨ��,������������</td></tr>
 <tr><td>channel_2,</td><td>ͼ������2ͨ����ʵ�ʵ�������</td></tr>
 <tr><td>channel_3,</td><td>ͼ������3ͨ����ʵ���ٶ�����</td></tr>
 <tr><td>channel_4,</td><td>ͼ������4ͨ����ʵ��λ��</td></tr>
 <tr><td>channel_cnt</td><td></td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th  colspan="2" style=background:PaleTurquoise>�������Ͷ��壬������ִ�����ڲ������ӵȴ������[ErrorsDefine] </th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>ERR_NONE = 0,</td><td>�޴���</td></tr>
 <tr><td>ERR_ACTUATOR_OVERVOLTAGE=0x01,</td><td>ִ������ѹ����</td></tr>
 <tr><td>ERR_ACTUATOR_UNDERVOLTAGE=0x02,</td><td>ִ����Ƿѹ����</td></tr>
 <tr><td>RR_ACTUATOR_LOCKED_ROTOR=0x04,</td><td>ִ������ת����</td></tr>
 <tr><td>ERR_ACTUATOR_OVERHEATING=0x08</td><td>ִ�������´���</td></tr>
 <tr><td>enum OnlineStatus{</td><td>ִ������д����</td></tr>
 <tr><td>ERR_ACTUATOR_MULTI_TURN=0x20,</td><td>ִ������Ȧ��������</td></tr>
 <tr><td>ERR_INVERTOR_TEMPERATURE_SENSOR=0x40,</td><td>ִ����������¶�������</td></tr>
 <tr><td>ERR_CAN_COMMUNICATION=0x80,</td><td>ִ�����¶ȴ���������</td></tr>
 <tr><td>ERR_ACTUATOR_TEMPERATURE_SENSOR=0x100,</td><td>ִ���� CAN ͨ�Ŵ���</td></tr>
 <tr><td>ERR_DRV_PROTECTION=0x400,</td><td>ִ���� DRV ����</td></tr>
 <tr><td>ERR_ID_UNUNIQUE=0x800</td><td>ִ���� ID ��Ψһ����</td></tr>
 <tr><td>ERR_ACTUATOR_DISCONNECTION=0x801,</td><td>ִ����δ���Ӵ���</td></tr>
 <tr><td>ERR_CAN_DISCONNECTION=0x802,</td><td>CAN ͨ��ת����δ���Ӵ���</td></tr>
 <tr><td>ERR_IP_ADDRESS_NOT_FOUND=0x803,</td><td>�޿��� ip ��ַ����</td></tr>
 <tr><td>ERR_ABNORMAL_SHUTDOWN=0x804,</td><td>ִ�����������ػ�����</td></tr>
 <tr><td>ERR_SHUTDOWN_SAVING=0x805,</td><td>ִ�����ػ�ʱ�����������</td></tr>
 <tr><td>ERR_UNKOWN=0xffff</td><td>δ֪����</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>����״̬�����ڱ�ʶִ�����Ƿ�������״̬[OnlineStatus]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>Status_Online=0x00,</td><td>ִ��������</td></tr>
 <tr><td>Status_Offline=0x01,</td><td>ִ��������</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>����״̬����ʶִ�����Ŀ��ػ�״̬[SwitchStatus]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>ACTUATOR_SWITCH_OFF=0,</td><td>ִ�����ѹػ�</td></tr>
 <tr><td>ACTUATOR_SWITCH_ON=1,</td><td>ִ�����ѿ���</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>ͼ���أ����ڱ�ʶִ����ͼ���ܵĿ�����ر�[ChartSwitchStatus]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>CHART_SWITCH_OFF=0,</td><td>ͼ���ܹرգ��������ͼ������</td></tr>
 <tr><td>CHART_SWITCH_ON=1,</td><td>ͼ���ܿ���������ͼ����ֵ�����ͼ������</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>������ͼ�����������ڱ�ʶ����ͼ����IQֵ����IDֵ[CurrnetChart]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>IQ_CHART=0,</td><td>ͼ������2ͨ����ʵ�ʵ���IQ����</td></tr>
 <tr><td>ID_CHART=1,</td><td>ͼ������2ͨ����ʵ�ʵ���ID����</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>ͨ�ŷ�ʽ����ͨ����̫�����ߴ������ַ�ʽ��ִ����ͨ�ţ���ʼ��ִ����������ʱ��Ҫָ����ʽ��Ĭ��Ϊ��̫��ͨ��[CommunicationType]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>Via_Ethernet,</td><td>��̫��ͨ��</td></tr>
 <tr><td>Via_Serialport,</td><td>����ͨ��</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>������ʶ����ʶ������ɣ��������ж�ִ������������ָ��ִ��״̬[OperationFlags]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>Recognize_Finished</td><td>ִ����������ɣ�������ӵ��Ƕ��ִ�������ᴥ�������������źţ�</td></tr>
 <tr><td>Launch_Finished</td><td>ִ�����ر���ɣ�������ӵ��Ƕ��ִ�������ᴥ����ιر�����źţ�</td></tr>
 <tr><td>Close_Finished</td><td>ִ��������������ɣ�������ӵ��Ƕ��ִ�������ᴥ����β�����������źţ�</td></tr>
 <tr><td>Save_Params_Finished</td><td>ִ������������ɹ�</td></tr>
 <tr><td>Save_Params_Failed</td><td>ִ������������ʧ��</td></tr>
 <tr><td>Attribute_Change_Finished</td><td>��δʵ��</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>ִ����ģʽ����ʶ��ǰִ������ģʽ[ActuatorMode]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>Mode_None</td><td>&nbsp;</td></tr>
 <tr><td>Mode_Cur</td><td>����ģʽ</td></tr>
 <tr><td>Mode_Vel</td><td>�ٶ�ģʽ</td></tr>
 <tr><td>Mode_Pos</td><td>λ��ģʽ</td></tr>
 <tr><td>Mode_Teaching</td><td>��δʵ��</td></tr>
 <tr><td>Mode_Profile_Pos=6</td><td>profileλ��ģʽ���Ƚ���λ��ģʽ����ģʽ�м��ټ��ٹ���</td></tr>
 <tr><td>Mode_Profile_Vel</td><td>profile�ٶ�ģʽ���Ƚ����ٶ�ģʽ����ģʽ�м��ټ��ٹ���</td></tr>
 <tr><td>Mode_Homing</td><td>����ģʽ</td></tr>
</tbody></table>

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th colspan="2"style=background:PaleTurquoise>ִ�������ԣ���ʶ��ִ���������������[ActuatorAttribute]</th></tr></thead><tbody>
 <tr><td>ָ���</td><td>˵��</td></tr>
 <tr><td>Cur_iq_setting</td><td>����IQֵ</td></tr>
 <tr><td>Cur_proportional</td><td>��������</td></tr>
 <tr><td>Cur_integral</td><td>��������</td></tr>
 <tr><td>Cur_id_setting</td><td>����IDֵ</td></tr>
 <tr><td>Cur_minimum</td><td>Ԥ��</td></tr>
 <tr><td>Cur_maximum</td><td>Ԥ��</td></tr>
 <tr><td>Cur_nominal</td><td>Ԥ��</td></tr>
 <tr><td>Cur_output</td><td>Ԥ��</td></tr>
 <tr><td>Cur_maxspeed</td><td>����������ٶ�</td></tr>
 <tr><td>Actual_current</td><td>��ǰ����ֵ</td></tr>
 <tr><td>Vel_setting</td><td>�ٶ�����</td></tr>
 <tr><td>Vel_proportional</td><td>�ٶȱ���</td></tr>
 <tr><td>Vel_integral</td><td>�ٶȻ���</td></tr>
 <tr><td>Vel_output_limitation_minimum</td><td>�ٶȻ������С��������</td></tr>
 <tr><td>Vel_output_limitation_maximum</td><td>�ٶȻ��������������</td></tr>
 <tr><td>Actual_velocity</td><td>�ٶ�ֵ</td></tr>
 <tr><td>Pos_setting</td><td>λ������</td></tr>
 <tr><td>Pos_proportional</td><td>λ�ñ���</td></tr>
 <tr><td>Pos_integral</td><td>λ�û���</td></tr>
 <tr><td>Pos_differential</td><td>λ��΢��</td></tr>
 <tr><td>Pos_output_limitation_minimum</td><td>λ�û������С�ٶȱ���</td></tr>
 <tr><td>Pos_output_limitation_maximum</td><td>λ�û��������ٶȱ���</td></tr>
 <tr><td>Pos_limitation_minimum</td><td>��Сλ������</td></tr>
 <tr><td>Pos_limitation_maximum</td><td>���λ������</td></tr>
 <tr><td>Homing_position</td><td>����λ��</td></tr>
 <tr><td>Actual_position</td><td>��ǰλ��</td></tr>
 <tr><td>Profile_pos_max_speed</td><td>profile position ģʽ����ٶ�</td></tr>
 <tr><td>Profile_pos_acc</td><td>profile position ģʽ���ٶ�</td></tr>
 <tr><td>Profile_pos_dec</td><td>profile position ģʽ�����ٶ�</td></tr>
 <tr><td>Profile_vel_max_speed</td><td>profile velocity ģʽ����ٶ�</td></tr>
 <tr><td>Profile_vel_acc</td><td>profile velocity ģʽ���ٶ�</td></tr>
 <tr><td>Profile_vel_dec</td><td>profile velocity ģʽ�����ٶ�</td></tr>
 <tr><td>Chart_frequency</td><td>ͼ��Ƶ��</td></tr>
 <tr><td>Chart_threshold</td><td>ͼ����ֵ</td></tr>
 <tr><td>Chart_switch</td><td>ͼ�񿪹�</td></tr>
 <tr><td>Pos_offset</td><td>λ��ƫ��</td></tr>
 <tr><td>Voltage</td><td>��ѹ</td></tr>
 <tr><td>Pos_limitation_switch</td><td>������ر�λ������</td></tr>
 <tr><td>Homing_cur_maximum</td><td>����������</td></tr>
 <tr><td>Homing_cur_minimum</td><td>������СС����</td></tr>
 <tr><td>Current_scale</td><td>����������ֵ</td></tr>
 <tr><td>Velocity_scale</td><td>�ٶ�������ֵ</td></tr>
 <tr><td>Filter_c_status</td><td>�������˲��Ƿ���</td></tr>
 <tr><td>Filter_c_value</td><td>�������˲�ֵ</td></tr>
 <tr><td>Filter_v_status</td><td>�ٶȻ��˲��Ƿ���</td></tr>
 <tr><td>Filter_v_value</td><td>�ٶȻ��˲�ֵ</td></tr>
 <tr><td>Filter_p_status</td><td>λ�û��˲��Ƿ���</td></tr>
 <tr><td>Filter_p_value</td><td>λ�û��˲�ֵ</td></tr>
 <tr><td>Inertia</td><td>����</td></tr>
 <tr><td>Lock_energy</td><td>��ת��������</td></tr>
 <tr><td>Actuator_temperature</td><td>ִ�����¶�</td></tr>
 <tr><td>Inverter_temperature</td><td>������¶�</td></tr>
 <tr><td>Actuator_protect_temperature</td><td>ִ���������¶�</td></tr>
 <tr><td>Actuator_recovery_temperature</td><td>ִ�����ָ��¶�</td></tr>
 <tr><td>Inverter_protect_temperature</td><td>����������¶�</td></tr>
 <tr><td>Inverter_recovery_temperature</td><td>������ָ��¶�</td></tr>
 <tr><td>Calibration_switch</td><td>Ԥ��</td></tr>
 <tr><td>Calibration_angle</td><td>Ԥ��</td></tr>
 <tr><td>Actuator_switch</td><td>ִ�������ػ�</td></tr>
 <tr><td>Firmware_version</td><td>ִ�����̼��汾</td></tr>
 <tr><td>Online_status</td><td>ִ�����Ƿ�����</td></tr>
 <tr><td>Device_id</td><td>ִ���� Id</td></tr>
 <tr><td>Sn_id</td><td>ִ���� SN ��</td></tr>
 <tr><td>Mode_id</td><td>ִ������ǰģʽ</td></tr>
 <tr><td>Error_id</td><td>�������</td></tr>
 <tr><td>Reserve_0</td><td>Ԥ��</td></tr>
 <tr><td>Reserve_1</td><td>Ԥ��</td></tr>
 <tr><td>Reserve_2</td><td>Ԥ��</td></tr>
 <tr><td>Reserve_3</td><td>Ԥ��</td></tr>
 <tr><td>Data_cnt</td><td>��������</td></tr>
 <tr><td>Data_chart</td><td>Ԥ��</td></tr>
 <tr><td>Data_invalid</td><td>�Ƿ�����ֵ</td></tr>
</tbody></table>

# �汾��Ϣ

<table class="tableizer-table">
<thead><tr class="tableizer-firstrow"><th>�汾</th><th>����</th><th>�޸�����</th></tr></thead><tbody>
 <tr><td>V2.0.0</td><td>2019-03-09</td><td>�ڶ��汾</td></tr>
 <tr><td>V1.0.0</td><td>2018-04-17</td><td>��һ�汾</td></tr>
</tbody></table>

