# GUI编程

GUI：图形用户界面编程

GUI编程学习路线

- GUI是什么
- GUI怎么写
- GUI使用场景

组件

- 监听
- 弹窗
- 面板
- 鼠标
- 键盘
- 按钮

## 1.简介

GUI核心技术：Swing   AWT

缺点：

1. 不美观
2. 需要jre环境

为什么要学习

1. 可以写出一些自己用的小工具
2. 可能会涉及到swing的维护工作 -> 破解
3. 了解MVC架构，了解监听

## 2.AWT

### 2.1.AWT介绍

1. AWT：抽象的窗口工具，包含了很多的类和接口
2. 元素：窗口、按钮、文本框
3. java.awt包下

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220205693.png)

### 2.2.组件和容器

#### 1.Frame

~~~java
import java.awt.*;

//GUI的第一个界面
public class TestFrame {
    public static void main(String[] args) {
        Frame frame = new Frame("frame标题");
//        设置窗口可见
        frame.setVisible(true);
//        设置大小
        frame.setSize(400,400);
//        设置背景颜色
        frame.setBackground(Color.BLACK);
//        设置初始位置
        frame.setLocation(400,400);
//        设置大小固定
        frame.setResizable(false);


    }
}
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220436260.png)

问题：关闭按钮无反应，关闭程序进程才会关闭窗口

封装打开多个实现：

~~~java
import java.awt.*;

public class TestFrame2 {
    public static void main(String[] args) {
        new MyFrame(100,100,200,200,Color.BLACK);
        new MyFrame(300,100,200,200,Color.BLUE);
        new MyFrame(100,300,200,200,Color.YELLOW);
        new MyFrame(300,300,200,200,Color.WHITE);
    }

}
class MyFrame extends Frame{
    //        计数器
    private static int id = 0;
    public MyFrame(int x,int y,int w,int h,Color color){
        super("MyFrame+"+id++);
        this.setBounds(x,y,w,h);
        this.setBackground(color);
        this.setVisible(true);
    }
}
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220509361.png)

#### 2.Panel

~~~java
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestPanel {
    public static void main(String[] args) {
        Frame frame = new Frame("带面板的Frame");
        Panel panel = new Panel();
        frame.setBounds(400,400,500,500);
        frame.setBackground(Color.BLACK);
        frame.setLayout(null);

        panel.setBounds(100,100,300,300);
        panel.setBackground(Color.YELLOW);
//        将面板放入frame中
        frame.add(panel);
        frame.setVisible(true);
//        添加监听
        frame.addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220525229.png)

### 2.3.布局管理器

**流式布局 FlowLayout**

~~~java
import java.awt.*;

//流式布局
public class TestFlowLayout {
    public static void main(String[] args) {
        Frame frame = new Frame();
        frame.setVisible(true);
        frame.setBounds(400,400,500,500);
        Button button1 = new Button("按钮1");
        Button button2 = new Button("按钮2");
        Button button3 = new Button("按钮3");

        frame.add(button1);
        frame.add(button2);
        frame.add(button3);
        frame.setLayout(new FlowLayout(FlowLayout.RIGHT));
    }
}
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/2020060822061739.png)

**东西南北中 BorderLayout**

~~~java
import java.awt.*;

public class TestBorderLayout {
    public static void main(String[] args) {
        Frame frame = new Frame();
        frame.setVisible(true);
        frame.setBounds(400,400,500,500);
        Button east = new Button("East");
        Button west = new Button("West");
        Button south = new Button("South");
        Button north = new Button("North");
        Button center = new Button("Center");

        frame.add(east,BorderLayout.EAST);
        frame.add(west,BorderLayout.WEST);
        frame.add(south,BorderLayout.SOUTH);
        frame.add(north,BorderLayout.NORTH);
//        frame.add(center,BorderLayout.CENTER);

    }

}
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220629729.png)

**表格式布局 GridLayout**

~~~java
import java.awt.*;

public class TestGridLayout {
    public static void main(String[] args) {
        Frame frame = new Frame();
        frame.setVisible(true);
//        frame.setBounds(400,400,500,500);

        Button button1 = new Button("but1");
        Button button2 = new Button("but2");
        Button button3 = new Button("but3");
        Button button4 = new Button("but4");
        Button button5 = new Button("but5");
        Button button6 = new Button("but6");
        frame.add(button1);
        frame.add(button2);
        frame.add(button3);
        frame.add(button4);
        frame.add(button5);
        frame.add(button6);
        frame.setLayout(new GridLayout(3,2));
        //        自动布局大小,测试过程中发现不能在第一行写这个，需要在添加完毕后增加会分配默认size
        frame.pack();
    }
}
~~~

实现效果：

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220642253.png)

嵌套布局：

~~~java
import java.awt.*;

public class TestDemo1 {
    public static void main(String[] args) {
        Frame frame = new Frame();
        frame.setVisible(true);
        frame.setBounds(100,100,800,800);
        Button button1 = new Button("button1");
        Button button2 = new Button("button2");
        Button button3 = new Button("button3");
        Button button4 = new Button("button4");
        Button button5 = new Button("button5");
        Button button6 = new Button("button6");
        Button button7 = new Button("button7");
        Button button8 = new Button("button8");
        Button button9 = new Button("button9");
        Button button10 = new Button("button10");
        frame.setLayout(null);
        Panel panel1 = new Panel(new GridLayout(1,1));
        panel1.setBounds(0,0,200,400);
        panel1.add(button1);
        Panel panel3 = new Panel(new GridLayout(2,1));
        panel3.setBounds(200,0,400,400);
        panel3.add(button2);
        panel3.add(button3);
        Panel panel4 = new Panel(new GridLayout(1,1));
        panel4.setBounds(600,0,200,400);
        panel4.add(button4);

        Panel panel2 = new Panel(new GridLayout());
        panel2.setBounds(0,400,200,400);
        panel2.add(button5);
        Panel panel5 = new Panel(new GridLayout(2,2));
        panel5.setBounds(200,400,400,400);
        panel5.add(button6);
        panel5.add(button7);
        panel5.add(button8);
        panel5.add(button9);
        Panel panel6 = new Panel(new GridLayout());
        panel6.setBounds(600,400,200,400);
        panel6.add(button10);

        frame.add(panel1);
        frame.add(panel3);
        frame.add(panel4);
        frame.add(panel2);
        frame.add(panel5);
        frame.add(panel6);
    }
}
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220712836.png)

### 2.4.监听器

1. 一个按钮可以添加多个监听

   ~~~java
   import java.awt.*;
   import java.awt.event.WindowAdapter;
   import java.awt.event.WindowEvent;
   
   public class TestActionEvent1 {
       public static void main(String[] args) {
           Frame frame = new Frame();
           frame.setVisible(true);
           frame.setBounds(100,100,500,500);
   //        一个按钮可以同时添加多个监听
           Button button = new Button();
           button.addActionListener((actionEvent) -> System.out.println("1"));
           button.addActionListener((actionEvent) -> System.out.println("2"));
           frame.add(button,BorderLayout.NORTH);
   
           frame.addWindowListener(new WindowAdapter() {
               @Override
               public void windowClosing(WindowEvent e) {
                   System.exit(0);
               }
           });
       }
   }
   ~~~

2. 一个监听可以给多个按钮共同使用

   ~~~java
   import java.awt.*;
   import java.awt.event.ActionEvent;
   import java.awt.event.ActionListener;
   import java.awt.event.WindowAdapter;
   import java.awt.event.WindowEvent;
   
   public class TestActionEvent2 {
       public static void main(String[] args) {
           Frame frame = new Frame();
           frame.setVisible(true);
           frame.setBounds(100,100,500,500);
   //        顶一个一个监听可以多个按钮共同使用
           ActionListener actionListener = new ActionListener() {
               @Override
               public void actionPerformed(ActionEvent e) {
                   System.out.println(e.getActionCommand());
               }
           };
           Button button1 = new Button("1");
   //        可以为按钮带参数来控制同一个监听执行不同按钮的方法
           button1.setActionCommand("11111111111111");
           button1.addActionListener(actionListener);
           Button button2 = new Button("2");
           button2.addActionListener(actionListener);
           frame.add(button1,BorderLayout.NORTH);
           frame.add(button2,BorderLayout.SOUTH);
   
           frame.addWindowListener(new WindowAdapter() {
               @Override
               public void windowClosing(WindowEvent e) {
                   System.exit(0);
               }
           });
       }
   }
   ~~~

总结

1. frame是一个顶级容器

2. panel面板不能单独存在，需要放到容器中使用

3. 布局管理器

   1. 流式布局
   2. 东西南北中
   3. 表格布局

4. 监听器

   ~~~java
   //        添加监听
   frame.addWindowListener(new WindowAdapter() {
       @Override
       public void windowClosing(WindowEvent e) {
           System.exit(0);
       }
   });
   ~~~

### 2.5输入框监听

~~~java
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestActionEvent3 {
    public static void main(String[] args) {
        new MyFrame();
    }
}
class MyFrame extends Frame{
    public MyFrame(){
        TextField textArea = new TextField();
        textArea.addActionListener((event)-> {
            System.out.println(textArea.getText());
            textArea.setText("");
        });
        this.setVisible(true);
        this.setBounds(100,100,500,500);
        add(textArea);
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}
~~~

### 2.6.简易计算器

~~~java
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestCalc {
    public static void main(String[] args) {
        Calc calc = new Calc();
    }
}
class Calc extends Frame{
    public Calc(){
        TextField jia1 = new TextField("10");
        TextField jia2 = new TextField("10");
        TextField rest = new TextField("20");
        setLayout(new FlowLayout());
        add(jia1);
        add(new Label("+"));
        add(jia2);
        Button button = new Button("=");
        button.addActionListener(e -> {
            int jia = Integer.parseInt(jia1.getText());
            int beijia = Integer.parseInt(jia2.getText());
            rest.setText(jia+beijia+"");
            jia1.setText("");
            jia2.setText("");
        });
        add(button);
        add(rest);
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
        setVisible(true);
        pack();

    }
}
~~~

### 2.7.画笔

~~~java
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestPaint {
    public static void main(String[] args) {
        new MyPaint().loadMyPaint();
    }
}
class MyPaint extends Frame{
    public void loadMyPaint(){
        setVisible(true);
        setBounds(100,100,800,600);
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }

    @Override
    public void paint(Graphics g) {
//        super.paint(g);
        g.setColor(Color.blue);
        g.drawOval(100,100,100,100);
        g.fillOval(100,200,100,100);
        g.fillRect(100,300,200,100);
    }
}
~~~

![在这里插入图片描述](https://gitee.com/xudongyin/img/raw/master/img/20200608220730708.png)

### 2.8.鼠标监听

目的：想要实现鼠标画画

~~~java
import java.awt.*;
import java.awt.event.MouseAdapter;
import java.awt.event.MouseEvent;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

//测试鼠标监听
public class TestMouseListenter {
    public static void main(String[] args) {
        new Paint("画图").loadMyPaint();
    }
}
class Paint extends Frame {
    private List<Point> pointList;
    private Paint cuttorPaint;
    public Paint(String title){
        super(title);
        cuttorPaint = this;
        pointList = new ArrayList<>();
    }
    public void loadMyPaint(){
        setVisible(true);
        setBounds(100,100,800,600);
        addMouseListener(new MyMouseListenter());
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }

    @Override
    public void paint(Graphics g) {
        Iterator<Point> iterator = pointList.iterator();
        while (iterator.hasNext()){
            Point point = iterator.next();
            //循环画列表的点坐标
            g.fillOval(point.x,point.y,10,10);
        }
    }
    private class MyMouseListenter extends MouseAdapter{
//        按下鼠标时触发
        @Override
        public void mousePressed(MouseEvent e) {
            pointList.add(new Point(e.getX(),e.getY()));
            //画笔重新画点
            cuttorPaint.repaint();
        }
    }
}
~~~

### 2.9.窗口监听

~~~java
import java.awt.*;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestWindow {
    public static void main(String[] args) {
        new MyWindow().loadMyPaint();
    }
}
class MyWindow extends Frame{
    public MyWindow(){
        super("默认窗口名称");
    }
    public void loadMyPaint(){
        setVisible(true);
        setBounds(100,100,800,600);
        addWindowListener(new WindowAdapter() {
//            点击窗口关闭按钮
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }

            @Override
            public void windowDeactivated(WindowEvent e) {
                System.out.println("窗口失去焦点");
            }

            //            打开窗口时执行
            @Override
            public void windowOpened(WindowEvent e) {
                System.out.println("windowOpened");
            }

            @Override
            public void windowClosed(WindowEvent e) {
                System.out.println("windowClosed");
            }

            @Override
            public void windowIconified(WindowEvent e) {
                System.out.println("缩小");
            }

            @Override
            public void windowDeiconified(WindowEvent e) {
                System.out.println("缩小对应的弹出窗口");
            }

            @Override
            public void windowActivated(WindowEvent e) {
                setTitle("被激活了窗口");
            }


            @Override
            public void windowStateChanged(WindowEvent e) {
                System.out.println("windowStateChanged");
            }

            @Override
            public void windowGainedFocus(WindowEvent e) {
                System.out.println("windowGainedFocus");
            }

            @Override
            public void windowLostFocus(WindowEvent e) {
                System.out.println("windowLostFocus");
            }
        });

    }
}
~~~

### 2.10.键盘监听

~~~java
import java.awt.*;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;

public class TestKeyListener {
    public static void main(String[] args) {
        new MyKeyFrame().loadMyPaint();
    }
}

class MyKeyFrame extends Frame{
    public MyKeyFrame(){
        super("默认窗口名称");
    }
    public void loadMyPaint(){
        setVisible(true);
        setBounds(100,100,800,600);
        addKeyListener(new KeyAdapter() {
//            按下键时执行
            @Override
            public void keyPressed(KeyEvent e) {
                int keyCode = e.getKeyCode();
                System.out.println(keyCode);
                if(keyCode == KeyEvent.VK_ENTER){
                    System.out.println("按了回车");
                }
            }
        });
        addWindowListener(new WindowAdapter() {
            @Override
            public void windowClosing(WindowEvent e) {
                System.exit(0);
            }
        });
    }
}
~~~

## 3.Swing

Swing是AWT的封装演化，

### 3.1.窗口 JFrame

~~~java
import javax.swing.*;
import java.awt.Container;
import static java.awt.Color.YELLOW;

public class TestJFrame1 {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame();
        jFrame.setBounds(10,10,400,400);
        jFrame.setVisible(true);

        JLabel jLabel = new JLabel("测试标签");
//        jFrame.setBackground(Color.BLACK);
        Container contentPane = jFrame.getContentPane();
        contentPane.setBackground(YELLOW);
        //标签居中
        jLabel.setHorizontalAlignment(SwingConstants.CENTER);
        jFrame.add(jLabel);
//        不设置，默认是隐藏窗口
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
    }
}
~~~

### 3.2弹窗 JDialog

~~~java

import javax.swing.*;
import java.awt.*;

import static java.awt.Color.YELLOW;

public class TestJFrame2 {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame();
        jFrame.setBounds(10,10,400,400);
        jFrame.setVisible(true);

        JButton jLabel = new JButton("测试按钮");
//        jFrame.setBackground(Color.BLACK);
        Container contentPane = jFrame.getContentPane();
        contentPane.setBackground(YELLOW);
//        设置居中
        jLabel.setHorizontalAlignment(SwingConstants.CENTER);
        jLabel.addActionListener(e ->
            new Mydiglog()
        );
        contentPane.setLayout(null);
        jLabel.setLocation(50,50);
        jLabel.setSize(200,50);
        contentPane.add(jLabel);
//        不设置，默认是隐藏窗口
        jFrame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }
}
class Mydiglog extends JDialog{
    public Mydiglog(){
        setVisible(true);
        setBounds(10,10,500,500);
//        弹窗默认带了关闭，不需要写
//        setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
        JLabel jLabel = new JLabel("测试弹窗标签2 ");
        Container container = getContentPane();
        container.setLayout(null);
        jLabel.setLocation(10,10);
        jLabel.setSize(100,20);
        container.add(jLabel);
    }
}
~~~

### 3.3 图标Icon

~~~java
package com.duan.lesson03;

import javax.swing.*;
import java.awt.*;

public class IconFrame1 {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame();
        jFrame.setBounds(10,10,400,400);
        jFrame.setVisible(true);
        JLabel jButton = new JLabel(" 标签",new MyIconDemo(10,10),SwingConstants.CENTER);
//        不设置，默认是隐藏窗口
        jFrame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        jButton.setSize(100,100);
        jFrame.add(jButton);
    }
}
class MyIconDemo implements Icon{
    private int iconWiddth;
    private int iconHeight;

    public MyIconDemo(int iconWiddth,int iconHeight){
        this.iconWiddth = iconWiddth;
        this.iconHeight = iconHeight;
    }
    @Override
    public void paintIcon(Component c, Graphics g, int x, int y) {
        g.fillOval(x,y,iconWiddth,iconHeight);
    }

    @Override
    public int getIconWidth() {
        return iconWiddth;
    }

    @Override
    public int getIconHeight() {
        return iconHeight;
    }
}
~~~

其他创建和使用与awt类似，类名加J前缀，增加了一些快捷实现

### 3.4滚动条面板

~~~java
import javax.swing.*;
import java.awt.*;
import java.io.File;
import java.net.MalformedURLException;

public class TestPanle4 {
    public static void main(String[] args)  {
        JFrame jFrame = new JFrame();
        jFrame.setBounds(10,10,400,400);
        jFrame.setVisible(true);
        JTextArea jTextArea = new JTextArea(50,50);
//        滚动条面板
        JScrollPane jButton = new JScrollPane(jTextArea);
        jFrame.add(jButton);
    }
}
~~~

## 4.贪吃蛇

帧，如果时间片足够小，就是动画 一秒三时帧，连起来是动画 拆开就是图片

键盘监听 addKeyListener

需要注意 如果是面板添加监听需要设置当前面板获取焦点 setFocusable (true);

定时器Timer

~~~java
import javax.swing.*;
import java.awt.*;
import java.awt.event.KeyAdapter;
import java.awt.event.KeyEvent;
import java.io.File;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Random;

public class StartGame  {
    public static void main(String[] args) {
        JFrame jFrame = new JFrame("贪吃蛇");

        jFrame.add(new GamePanel());
//        设置窗口不可拉伸
        jFrame.setResizable(false);
//        固定大小
        jFrame.setBounds(100,100,900,750);
        jFrame.setVisible(true);
        jFrame.setDefaultCloseOperation(WindowConstants.EXIT_ON_CLOSE);
    }

}
//游戏面板
class GamePanel extends JPanel{
    private int length = 3;
    private int fx;
    private int[][] zuobiao = new int[2][850];
    private boolean isStart = false;
    private boolean isError = false;
    private int[] foot = new int[2];
    Random random = new Random();
    private Timer timer = new Timer(100,e -> {
        if(isStart && !isError){
            if(zuobiao[0][0] == foot[0] && zuobiao[1][0] == foot[1]){
                length++;
                setRandomFoot();
            }
            for (int i = length - 1; i > 0; i--) {
                zuobiao[0][i] =  zuobiao[0][i-1];
                zuobiao[1][i] =  zuobiao[1][i-1];
            }
            switch (fx){
                case KeyEvent.VK_RIGHT:
                    zuobiao[0][0] = zuobiao[0][0]+25;
                    if(zuobiao[0][0] >= 875)zuobiao[0][0]=25;
                    break;
                case KeyEvent.VK_LEFT:
                    zuobiao[0][0] = zuobiao[0][0]-25;
                    if(zuobiao[0][0] < 25)zuobiao[0][0]=850;
                    break;
                case KeyEvent.VK_UP:
                    zuobiao[1][0] = zuobiao[1][0]-25;
                    if(zuobiao[1][0] <= 70)zuobiao[1][0]=675;
                    break;
                case KeyEvent.VK_DOWN:
                    zuobiao[1][0] = zuobiao[1][0]+25;
                    if(zuobiao[1][0] >= 675)zuobiao[1][0]=70;
                    break;
            }
//            失败条件
            for (int i = 1; i < length; i++) {
                if(zuobiao[0][0] == zuobiao[0][i] && zuobiao[1][0] == zuobiao[1][i]){
                    isError = true;
                }
            }
            repaint();
        }
    });
    public GamePanel(){
        addKeyListener(new KeyAdapter() {
            @Override
            public void keyPressed(KeyEvent e) {
                switch (e.getKeyCode()){
                    case KeyEvent.VK_ENTER:
                        isError = false;
                        timer.stop();
                        init();
                        break;
                    case KeyEvent.VK_SPACE:
                        if(!isError){
                            isStart = !isStart;
                            repaint();
                        }
                        break;
                    case KeyEvent.VK_UP:
                        if(fx != KeyEvent.VK_DOWN)fx = KeyEvent.VK_UP;
                        break;
                    case KeyEvent.VK_DOWN:
                        if(fx != KeyEvent.VK_UP)fx = KeyEvent.VK_DOWN;
                        break;
                    case KeyEvent.VK_LEFT:
                        if(fx != KeyEvent.VK_RIGHT)fx = KeyEvent.VK_LEFT;
                        break;
                    case KeyEvent.VK_RIGHT:
                        if(fx != KeyEvent.VK_LEFT)fx = KeyEvent.VK_RIGHT;
                        break;
                }
            }
        });
        init();
    }
    public void init(){
        length = 3;
        zuobiao[0][0] = 100;
        zuobiao[1][0] = 95;
        for (int i = 1; i < length; i++) {
            zuobiao[0][i] = zuobiao[0][0]-i*25;
            zuobiao[1][i] = zuobiao[1][0];
        }
        setRandomFoot();
//        默认向右
        fx = KeyEvent.VK_RIGHT;
        setFocusable(true);
//        timer.setDelay(1);
        timer.start();
    }
//设置事物随机坐标
    private void setRandomFoot() {
        foot[0] = 25 + 25*random.nextInt(34);
        foot[1] = 70 + 25*random.nextInt(24);
        for (int i = 0; i < length; i++) {
            if(zuobiao[0][i] == foot[0] && zuobiao[1][i] == foot[1]){
                setRandomFoot();
            }
        }
    }

    //    画板
    @Override
    protected void paintComponent(Graphics g) {
        super.paintComponent(g);
        setBackground(Color.white);
        GameData.header.paintIcon(this,g ,25,0);
        g.fillRect(25,70,850,625);
        GameData.food.paintIcon(this,g ,foot[0],foot[1]);
        switch (fx){
            case KeyEvent.VK_RIGHT:
                GameData.right.paintIcon(this,g,zuobiao[0][0],zuobiao[1][0]);
                break;
            case KeyEvent.VK_LEFT:
                GameData.left.paintIcon(this,g,zuobiao[0][0],zuobiao[1][0]);
                break;
            case KeyEvent.VK_UP:
                GameData.up.paintIcon(this,g,zuobiao[0][0],zuobiao[1][0]);
                break;
            case KeyEvent.VK_DOWN:
                GameData.dowm.paintIcon(this,g,zuobiao[0][0],zuobiao[1][0]);
                break;
        }
        for (int i = 1; i < length; i++) {
            GameData.body.paintIcon(this,g,zuobiao[0][i],zuobiao[1][i]);
        }

        if(!isStart){
            g.setColor(Color.YELLOW);
            g.setFont(new Font("微软雅黑",Font.BOLD,48));
            g.drawString("按空格开启游戏",250,500);
        }
        if(isError){
            g.setColor(Color.RED);
            g.setFont(new Font("微软雅黑",Font.BOLD,48));
            g.drawString("游戏失败,按回车开启游戏",250,500);
        }
    }
}
//游戏图标类
class GameData{
    private static URL bodyurl;
    private static URL downurl;
    private static URL foodurl;
    private static URL headerurl;
    private static URL lefturl;
    private static URL righturl;
    private static URL upurl;
    static {
        try {
            bodyurl = new File("D:\\xuexi\\img\\statics\\body.png").toURL();
            foodurl = new File("D:\\xuexi\\img\\statics\\food.png").toURL();
            headerurl = new File("D:\\xuexi\\img\\statics\\header.png").toURL();
            upurl = new File("D:\\xuexi\\img\\statics\\up.png").toURL();
            downurl = new File("D:\\xuexi\\img\\statics\\down.png").toURL();
            lefturl = new File("D:\\xuexi\\img\\statics\\left.png").toURL();
            righturl = new File("D:\\xuexi\\img\\statics\\right.png").toURL();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
    }
    public static ImageIcon header = new ImageIcon(headerurl);
    public static ImageIcon body = new ImageIcon(bodyurl);
    public static ImageIcon food = new ImageIcon(foodurl);
    public static ImageIcon up = new ImageIcon(upurl);
    public static ImageIcon dowm = new ImageIcon(downurl);
    public static ImageIcon left = new ImageIcon(lefturl);
    public static ImageIcon right = new ImageIcon(righturl);
}
\\img\\statics\\body.png").toURL();
            foodurl = new File("D:\\xuexi\\img\\statics\\food.png").toURL();
            headerurl = new File("D:\\xuexi\\img\\statics\\header.png").toURL();
            upurl = new File("D:\\xuexi\\img\\statics\\up.png").toURL();
            downurl = new File("D:\\xuexi\\img\\statics\\down.png").toURL();
            lefturl = new File("D:\\xuexi\\img\\statics\\left.png").toURL();
            righturl = new File("D:\\xuexi\\img\\statics\\right.png").toURL();
        } catch (MalformedURLException e) {
            e.printStackTrace();
        }
    }
    public static ImageIcon header = new ImageIcon(headerurl);
    public static ImageIcon body = new ImageIcon(bodyurl);
    public static ImageIcon food = new ImageIcon(foodurl);
    public static ImageIcon up = new ImageIcon(upurl);
    public static ImageIcon dowm = new ImageIcon(downurl);
    public static ImageIcon left = new ImageIcon(lefturl);
    public static ImageIcon right = new ImageIcon(righturl);
}
~~~

