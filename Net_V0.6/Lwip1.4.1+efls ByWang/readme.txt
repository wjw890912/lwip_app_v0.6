


QQ:925295580
e-mail:925295580@qq.com
Time:201512
author:����ΰ



       SW

   beta---(V0.1)


1��TCP/IP����Э��ջ
.֧��UDP
.֧��TCP
.֧��ICMP

2����������efs�ļ�ϵͳ
.֧��unix��׼�ļ�API
.֧��SDHC��׼������V1.0��V2.0������SDK��-16G����ѹ�������������ֲο���Դ :-)��
.�����ڴ�ռ�ã�����һ��512�ֽڵĸ��ٻ��������ļ�������

3��֧��1-write DS18B20 	�¶ȴ�����
.֧�ֵ������ϸ�ʱ��
.֧��ROM������������Ҷ�ӣ�����һ�����߹ҽӶ���¶ȴ�����
.�����Զ�ת��ΪHTML�ļ�

4��TCP/IPӦ��
.֧��TFTP����ˣ���������ļ����������񡣣��˲�������GITHUB�����Ӳ���TIMEOUT �¼���tftp -i
.֧��NETBIOS����
.֧��һ��TCP�����������ض˿�8080.	���Է���
.֧��һ��TCP�ͻ��ˣ����ض˿�40096	Զ�˶˿�2301 Զ��IP192.168.0.163	���������ݽ���
.֧��һ��UDP�㲥��  ���ض˿�02	    �㲥��ַ255.255.255.255	  �������¶Ȳɼ������ݷ��㲥
.֧��һ��HTTP������ ���ض˿�80  	http:ip  ����֮	  ����2ֻ18B20�¶ȴ�������ʾ�ڼ򵥵�SD������ҳ��

5��ϵͳ�����
Program Size: Code=51264 RO-data=28056 RW-data=1712 ZI-data=55048  

6����������

ipaddr��192, 168, 0, 253-209
netmask��255, 255, 0, 0
gw��192, 168, 0, 20
macaddress��xxxxxxxxxx


tks for GitHUB

	    HW

 stm32f107+DP83848CVV+ds18B20*2+SDHC card (4GB)

7���޸���һ��SDcrad��BUG����һ���ж����д���ˣ�fix�����֧��2GB��4GB���ϵ����ֿ�Ƭ�ˡ�20160122
8��������һ������main.h��
#define MYSELFBOARD
��������ˣ���ô��ʾʹ����ΰ���ҵĿ�����
����������ѡ�����Լ������ǿ���ӡ�
û��ʲô�������𣬿��һ����ֻ��IO�����иĶ���20160122


 20160123
9������һ��SMTPӦ�ã�����ͨ������USED_SMTP��ʹ�� ��
10�����smtp.c����ֲ�Ͳ��ԡ��������ҵ����䷢���ʼ���������Ҫ���ùر�SSL��������Ҫ�޸�һ��Դ�� �ļ����궨�塣
11���Ѳɼ����¶�����20���ӷ�һ���ʼ������Լ��������С���ɡ�  ������ͨ�������IP�������ȥ����ǽ

12��������һ�·������ʱ��Ϊ5����һ���ʼ���@163��������ƴ�Լ��300���ʼ�


20160402

13�������з������ڴ�й¶�����,ͨ�������ڴ����Ϣͨ��TCP����� 2301�˿�debug����
  �쳣������
 ���ڴ������ֽڣ���6144
 �����ڴ棨�ֽڣ���5816
 ʣ���ڴ������ֽڣ���328
 ʹ�ñ�ʾ��1
 ����������
 ���ڴ������ֽڣ���6144
 �����ڴ棨�ֽڣ���20
 ʣ���ڴ������ֽڣ���6124
 ʹ�ñ�ʾ��1
 ��Ȼmemallocʹ�ڴ�������Ҵ�����Ϊ����SMTPӦ�ó���ʹ��malloc������������ʹ�õ������
 ���Կ϶���SMTP������
 ��һ����������ΪSMTP��smtp_send_mail������
 smtp_send_mail_alloced(struct smtp_session *s)
 ����ʹ�õ�
 s = (struct smtp_session *)SMTP_STATE_MALLOC((mem_size_t)mem_len);
 ������һ���ڴ�û�����������ͷš�
 ��������
 �������յ������Ӧ�ô��벻����������һ�������� mem_le��С���ڴ���һֱ������328�ֽڵ�ʣ���ڴ档
 �����յ�������������mem��Ӧ�ó���ȫ����ȡ�����㹻���ڴ�顣�����ֵ��ڴ������
 �������� �ͷŵ��ڴ���  (struct smtp_session *) s
 ���ּ�������

 1����������ֹ	�����ա�
   if (smtp_verify(s->to, s->to_len, 0) != ERR_OK) {
    return ERR_ARG;
  }
  if (smtp_verify(s->from, s->from_len, 0) != ERR_OK) {
    return ERR_ARG;
  }
  if (smtp_verify(s->subject, s->subject_len, 0) != ERR_OK) {
    return ERR_ARG;
  }
  ����û�ж�  smtp_send_mail_alloced ���������ж���������˴����ػ���ɺ�������������ֹ
  Ҳ�ͻᵼ�� (struct smtp_session *) s	û�л����ͷţ���Ϊ�ڲ�������ֹʱ���ں��洦��ģ�
  ���ǿ��ǵ�Դ�����ǹ̶��Ĵ�Ƭ��flash��ȡ�õģ����ּ��ʼ���û�С����Ǵ��ڷ��ա�����ͳһ��Ϊ
  if (smtp_verify(s->to, s->to_len, 0) != ERR_OK) {
    	 err = ERR_ARG;
     goto leave;
  }
  if (smtp_verify(s->from, s->from_len, 0) != ERR_OK) {
    	 err = ERR_ARG;
     goto leave;
  }
  if (smtp_verify(s->subject, s->subject_len, 0) != ERR_OK) {
    	 err = ERR_ARG;
     goto leave;
  }

  2����������TCP���ӣ���Ҫԭ��
  ԭ���ĺ���Ϊ��
  if(tcp_bind(pcb, IP_ADDR_ANY, SMTP_PORT)!=ERR_OK)
	{
	return	ERR_USEl;
  	   
	}
  ��Ȼ����ͬ���Ļ����malloc �����˵���û�б����ã��޸�Ϊ
  if(tcp_bind(pcb, IP_ADDR_ANY,SMTP_PORT)!=ERR_OK)
  {
	err = ERR_USE;
    goto leave;		   
  }

   ����	  leave�оͻ��Զ������ͷŵ������������ֹ�Ķ���ɵ��ڴ��������⡣
   leave:
  smtp_free_struct(s);
  return err;

 ��������һ�����⡣�Ǿ��Ǳ��뱣֤malloc ��free �ɶԳ��֡�



 14��NETBIOS ���ַ���������lwipopts.h������
 #define NETBIOS_LWIP_NAME "WJW-BOARD"
 ��ȷ������
 ��������ʹ�����¸�ʽ�ҵ����ӵ�IP��ַ
 ping 	wjw-board 
 ������ָ��IP��ַ
 20160410



 /*�����з��ֳ�ʱ�����к�SMTP����ֹͣ������������ڴ�����������Ѿ���������波���޸Ľ��н��������������-��17��*/
  20160427

 15���޸�SNMTP��timout��ʱʱ��ͳһΪ2���ӣ���Ϊ�ҵ��ʼ��ط�ʱ��Ϊ3���ӡ�Ĭ�ϵ�10����̫�������޸�֮��������Ӱ��ġ�fix

 /** TCP poll timeout while sending message body, reset after every
 * successful write. 3 minutes def:(3 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATABLOCK  ( 1 * 60 * SMTP_POLL_INTERVAL / 2)
/** TCP poll timeout while waiting for confirmation after sending the body.
 * 10 minutes def:(10 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATATERM   (1 * 60 * SMTP_POLL_INTERVAL / 2)
/** TCP poll timeout while not sending the body.
 * This is somewhat lower than the RFC states (5 minutes for initial, MAIL
 * and RCPT) but still OK for us here.
 * 2 minutes def:( 2 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT            ( 1 * 60 * SMTP_POLL_INTERVAL / 2)
 20160427
 
  16�����Ӽ��SMTP TCP ���ֵı�������
   smtp_Tcp_count[0]//tcp new count 
    smtp_Tcp_count[1]//bind count
	 smtp_Tcp_count[2]connect count 
	 smtp_Tcp_count[3]bind fail save the all pcb list index number
	 that all use debug long time running on smtp .
 20160427

17�����ֲ���SMTP�������ƺ�����������ˣ������޸�����15������ȫ��Ϊ2���� ��30*4*0.5S=1MIN *2=2min 

/** TCP poll interval. Unit is 0.5 sec. */
#define SMTP_POLL_INTERVAL      4
/** TCP poll timeout while sending message body, reset after every
 * successful write. 3 minutes def:(3 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATABLOCK  30*2
/** TCP poll timeout while waiting for confirmation after sending the body.
 * 10 minutes def:(10 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT_DATATERM   30*2
/** TCP poll timeout while not sending the body.
 * This is somewhat lower than the RFC states (5 minutes for initial, MAIL
 * and RCPT) but still OK for us here.
 * 2 minutes def:( 2 * 60 * SMTP_POLL_INTERVAL / 2)*/
#define SMTP_TIMEOUT
20160429            30*2
18���ӳ���KEEPALIVBEʱ��Ϊ
	pcb->so_options |= SOF_KEEPALIVE;
   pcb->keep_idle = 1500+150;// ms
    pcb->keep_intvl = 1500+150;// ms
   pcb->keep_cnt = 2;// report error after 2 KA without response
 20160429


19�����Ӽ���SMTP����ı���	smtp_Tcp_count[10]	upsize 10 dword

20�����Ӽ��SMTP TCP ���ֵı�������
   smtp_Tcp_count[0]//tcp new count 
    smtp_Tcp_count[1]//bind count
	 smtp_Tcp_count[2]connect count 
	 smtp_Tcp_count[3]bind fail save the all pcb list index number
add smtp send result 
              smtp_Tcp_count[4]|= (smtp_result);  
			  smtp_Tcp_count[4]|= (srv_err<<8);
			  smtp_Tcp_count[4]|= (err<<24);

			  if(err==ERR_OK){smtp_Tcp_count[5]++;}	//smtp�ɹ�����ͳ��

			   if(arg!=(void*)0)
			   {
				smtp_Tcp_count[6]=0xAAAAAAAA ;	 //�в���
			   }
			   else
			   {
			   
			   smtp_Tcp_count[6]=0x55555555 ;	//û�в���
			   }
20160430
21��

 ���ϲ����з������е�9�����Ҿͻ᲻��ִ��SMTP���뷵���������£� ���ֽ���ǰ--���ֽ��ں�
 ��Receive from 192.168.0.253 : 40096����
5D 11 00 00     5D 11 00 00    58 11 00 00    00 00 00 00     04 00 00 F6  48 11 00 00  55 55 55 55 

��������ݿ�֪��
tcp new count=0x115d
bind count=0x115d
connect count=0x1158
bind fail  number=0
smtp_result=  4��SMTP_RESULT_ERR_CLOSED��
srv_err=00
tcp err=0xf6�Ǹ�����ҪNOT +1=(-10) �������Ϊ  ERR_ABRT	  Connection aborted

�������ݶ��񣬲��ڱ仯��˵�������TCP baind û�й�ϵ����TCP new��֮ǰ�ĵ������⣬���Լ���������������ҡ�

20160513

22�����ٶȵ��죬10����һ��SMTP ���ӡ��޸�SMTP Ӧ�ó���ĳ�ʱʱ��Ϊ8����ͬʱ����
smtp_Tcp_count[8]++;�������ܵĵ���SMTP�Ĵ���
smtp_Tcp_count[7]++;������SMTP �õ�TCP new֮ǰ�Ĵ������ų�һ��TCP new�����⣡
����������һֱ�仯�������û�б仯��֤��TCP ��new������֮����ǰ�ƣ�ֱ���������ĵط�һ����ų���

����׷�����ֹͣTCP ���ӵ����⡣
20160513


23����TCP��IP��ַ�ĵ������Ƶ�IP��ַ���˿�ʹ��2302.�������������ϵ�Զ�̲��ԡ�����IP��ַ��Ϊ192.168.254.250�Թ�ܵ�ǰ���ع�˾����IP���ơ�
��������汾������2����֧һ�����������֧����һ����TCP2modbus�ķ�֧��������ص����TCP��飬�Ǹ��ص����TCPת485��顣
���������22��׷���Ѿ���ɣ��������   smtp_Tcp_count[8]++;��������ݿ��Ա����ã�������������������ˢ����˵��������ڴ�SMTP��TCP������顣
�������£�



5D 11 00 00 smtp_Tcp_count[0]
5D 11 00 00 smtp_Tcp_count[1]
29 10 00 00 smtp_Tcp_count[2]
00 00 00 00 smtp_Tcp_count[3]
06 17 02 F7 smtp_Tcp_count[4]
ED 0F 00 00 smtp_Tcp_count[5]
55 55 55 55 smtp_Tcp_count[6]
5D 11 00 00 smtp_Tcp_count[7]
EB 16 00 00	smtp_Tcp_count[8]

�������ݷ�����֪my_smtp_test(char *str)�������ִ��������SMTP.C�ļ��е�smtp_Tcp_count[7];֮��ĳ�ʽû�б�ִ�У�
��˵����ͳ����� smtp_Tcp_count[8]�� smtp_Tcp_count[7]֮��ĳ������棬��������ͱ���С�ˡ�
����Ҿ��Լ�����������������ء�������������һ���ط��쳣������
����������ҵ�����Ȼ��Ҫʱ�䳤����Ϊ����һ�����ж�����֮��Ż���ֵ�BUG�����Ѳ����������
ͬʱ����ο�Ŀ���TCP���ر���û�г����⡣�������Ǹ�����Ϣ��

����

smtp_Tcp_count[9] - smtp_Tcp_count[19] ���	  smtp_Tcp_count[7]  smtp_Tcp_count[8]֮�������Ĳ���
�ٹ���������ҷ������ݼ���ָ���������������


 �����޸���TCPsent ����ʹ��start���ʼ�Զ�����TCP���ݵ�������һ��2048�ֽڡ����1S,��Ҫ���Ե���MTU���ݿ鴫�䡣

										
20160626




