# Application

��Java�����У�������Ը��������л����Ͳ���ʽ��Ϊ��ͬ���͡�������Java�������Ͱ���StandaloneӦ�ó���WebӦ�ó�����ҵ��Ӧ�ó�����ƶ�Ӧ�ó���ȡ������Ƕ���Щ���͵���ϸ���ͣ�

## Standalone Ӧ�ó���

### ����
StandaloneӦ�ó��򣨶���Ӧ�ó�����ָ�ܹ��������е�Java���򣬲���Ҫ������Web��������Ӧ�÷��������������ͨ��ͨ�������л�ͼ���û����棨GUI�����û�������

### �ص�
- **������**�������������������֧�ּ������С�
- **���͵���ڵ�**������һ��`main`������Ϊ�������ڵ㡣
- **�û�����**������ʹ�������н��棨CLI����GUI���߰�����Swing��JavaFX����

### ʾ��
һ���򵥵�������JavaӦ�ó���
```java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

һ���򵥵�GUI JavaӦ�ó���ʹ��Swing����
```java
import javax.swing.*;

public class HelloWorldSwing {
    public static void main(String[] args) {
        JFrame frame = new JFrame("HelloWorldSwing");
        JLabel label = new JLabel("Hello, World!", SwingConstants.CENTER);
        frame.add(label);
        frame.setSize(300, 100);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }
}
```

## Web Ӧ�ó���

### ����
WebӦ�ó�����Web��������Ӧ�÷����������У�ͨ��HTTPЭ����ͻ��ˣ�ͨ����Web�������������WebӦ�ó���ͨ������ǰ�ˣ�HTML��CSS��JavaScript���ͺ�ˣ�Java Servlet��JSP��Spring MVC�ȣ���

### �ص�
- **���������**��ͨ��Web��������ʡ�
- **�������˴���**��ҵ���߼��ڷ������˴�����Ӧ�ͻ�������
- **�����ڷ�������**����Tomcat��Jetty��WildFly�ȡ�

### ʾ��
һ���򵥵�Servlet��
```java
import java.io.IOException;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

@WebServlet("/hello")
public class HelloServlet extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        response.getWriter().println("Hello, World!");
    }
}
```

## ��ҵ��Ӧ�ó���Enterprise Application��

### ����
��ҵ��Ӧ�ó���Enterprise Application����Ϊ��ҵ��������Ƶģ�ͨ��������Java EE����Jakarta EE��Ӧ�÷������ϣ����и߿���չ�Ժͷֲ�ʽ���ԣ�֧����������ȫ�ԡ��־��Եȡ�

### �ص�
- **�ֲ�ʽ�ܹ�**���������ģ�飬��EJB��Enterprise JavaBeans����JPA��Java Persistence API����JMS��Java Message Service���ȡ�
- **�������**��֧�ָ��ӵ�������
- **��ȫ��**������֧�ְ�ȫ����

### ʾ��
һ���򵥵�EJB��
```java
import javax.ejb.Stateless;

@Stateless
public class HelloBean {
    public String sayHello() {
        return "Hello, World!";
    }
}
```

## �ƶ�Ӧ�ó���

### ����
�ƶ�Ӧ�ó���ָ�������ƶ��豸����Android�ֻ����ϵ�Java����ͨ��ʹ��Android SDK���п�����

### �ص�
- **�ƶ�ƽ̨**��ר��Ϊ�ƶ��豸��ơ�
- **Android SDK**��ʹ��Android��ܺ͹��߽��п�����
- **APK���**�����ΪAPK�ļ�������Android�豸�ϡ�

### ʾ��
һ���򵥵�Android Activity��
```java
import android.app.Activity;
import android.os.Bundle;
import android.widget.TextView;

public class MainActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        TextView textView = new TextView(this);
        textView.setText("Hello, World!");
        setContentView(textView);
    }
}
```

## �ܽ�

Java������Ը������л�������;��Ϊ�������ͣ�

1. **Standalone Ӧ�ó���**���������У�����`main`������
2. **Web Ӧ�ó���**��������Web�������ϣ�ͨ����������ʡ�
3. **��ҵ��Ӧ�ó���**��������Java EE�������ϣ�֧�ָ��ӵ���ҵ�����ܡ�
4. **�ƶ�Ӧ�ó���**���������ƶ��豸�ϣ�ʹ��Android SDK������

ÿ�����͵�JavaӦ�ó��������ض��Ŀ������ߡ���ܺͲ���ʽ��ѡ����ʵ����Ϳ������㲻ͬ��Ӧ������