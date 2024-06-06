# JMM


## JMM and JVM

JMM �� Java �����(JVM)�ڼ�����ڴ�(RAM)�еĹ�����ʽ��JMM �������̺߳����ڴ�֮��ĳ����ϵ���߳�֮��Ĺ�������洢�����ڴ棨Main Memory���У�ÿ���̶߳���һ��˽�еı����ڴ棨Local Memory���������ڴ��д洢�˸��߳��Զ�/д��������ĸ����������ڴ��� JMM ��һ��������������ʵ���ڡ��������˻��桢д���������Ĵ����Լ�������Ӳ���ͱ������Ż����� JVM ������������ Java ������ڲ��������ṹ��Ĺ�ϵ��

* JVM �ڴ�ṹ�� Java �����������ʱ������أ������� JVM ������ʱ��η����洢�������ݣ��ͱ���˵����Ҫ���ڴ�Ŷ���ʵ����
* Java �ڴ�ģ�ͣ�JMM�� �� Java �Ĳ��������أ��������̺߳����ڴ�֮��Ĺ�ϵ�ͱ���˵�߳�֮��Ĺ����������洢�����ڴ��У��涨�˴� Java Դ���뵽 CPU ��ִ��ָ������ת������Ҫ������Щ�Ͳ�����ص�ԭ��͹淶������ҪĿ����Ϊ�˼򻯶��̱߳�̣���ǿ�������ֲ�Եġ�

## ���治һ������

��� JMM �еı����ڴ�����Ļ��治һ�����������ֽ���������ֱ������߼�����MESI����һ����Э��

### MSI protocol (����һ����Э��)

> MESI ����һ����Э���Ƕ�� CPU �����ڴ��ȡͬһ�����ݵ����Եĸ��ٻ����У������е�ĳ�� CPU �޸��˻���������ݣ������ݻ�����ͬ�������ڴ棬���� CPU ͨ��������̽���ƿ��Ը�֪�����ݵı仯�Ӷ����Լ������������ʧЧ��

�ڲ�������У��������̶߳�ͬһ������������в����ǣ�����ͨ�����ڱ�������ǰ���Ϲؼ���**volatile**,��Ϊ�����Ա�֤�̶߳Ա������޸ĵĿɼ��ԣ���֤�ɼ��ԵĻ����Ƕ���̶߳���������ߡ�����һ���߳��޸��˹�������󣬸ñ���������ͬ�������ڴ棬�����̼߳��������ݱ仯���ʹ���Լ������ԭ����ʧЧ��������read������ȡ���޸ĵı�����ֵ��������֤�˶���̵߳�����һ���ԡ���ʵ�ϣ�**volatile**�Ĺ���ԭ����������� MESI ����һ����Э��ʵ�ֵġ�

## happens-before relationship

> happens-before ԭ�����������ʵ������һ����������������һ��������ǰ�棬��Ȼ��ӳ���Ա�ĽǶ�����˵Ҳ���޴󰭡���׼ȷ����˵�����������������ǰһ�������Ľ�����ں�һ�������ǿɼ��ģ����������������Ƿ���ͬһ���߳��

### details (all 8 tips)

* Program Order Rule: Each action in a thread happens-before every action in that thread that comes later in the program order.  
* Monitor Lock Rule: An unlock on a monitor lock happens-before every subsequent lock on that same monitor lock.  
* Volatile Variable Rule: A write to a volatile field happens-before every subsequent read of that same field.  
* Thread Start Rule: A call to Thread.start on a thread happens-before any action in the started thread.  
* Thread Termination Rule: Any action in a thread happens-before any other thread detects that thread has terminated, either by successfully return from Thread.join or by Thread.isAlive returning false.  
* Transitivity: If A happens-before B, and B happens-before C, then A happens-before C.

## summary

* Java �����糢���ṩ�ڴ�ģ�͵����ԣ�����ҪĿ����Ϊ�˼򻯶��̱߳�̣���ǿ�������ֲ�Եġ�
* CPU ����ͨ���ƶ�����һ��Э�飨���� MESI Э�飩������ڴ滺�治һ�������⡣
* Ϊ������ִ���ٶ�/���ܣ��������ִ�г�������ʱ�򣬻��ָ����������� ����˵����ϵͳ��ִ�д����ʱ�򲢲�һ���ǰ�����д�Ĵ����˳������ִ�С�ָ����������Ա�֤��������һ�£�����û������֤���̼߳������Ҳһ�� �������ڶ��߳��£�ָ����������ܻᵼ��һЩ���⡣
* ����԰� JMM ������ Java ����Ĳ��������ص�һ��淶�����˳������̺߳����ڴ�֮��Ĺ�ϵ֮�⣬�仹�涨�˴� Java Դ���뵽 CPU ��ִ��ָ������ת������Ҫ������Щ�Ͳ�����ص�ԭ��͹淶������ҪĿ����**Ϊ�˼򻯶��̱߳�̣���ǿ�������ֲ�Ե�**��
* JSR 133 ������ happens-before ���������������������֮����ڴ�ɼ��ԡ�

## reference

* �٣�ͬѧ����Ҫ�� Java �ڴ�ģ�� (JMM) ���ˣ�https://xie.infoq.cn/article/739920a92d0d27e2053174ef2
* docs/java/concurrent/jmm.md: https://github.com/Snailclimb/JavaGuide/blob/main/docs/java/concurrent/jmm.md