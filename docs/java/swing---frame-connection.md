---
layout: default
title: Swing - 화면(프레임) 연결
last_modified_date: 2020-12-18 15:12:16
parent: Java
---

<div style="font-size:32px; font-weight: 800; border-left: 7px solid #0687f0; padding-left:15px !important; color:#000000; margin-bottom:15px;">Swing - 화면(프레임) 연결</div>

{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

자바 GUI 프로젝트를 진행하면서 JFrame을 만들기 위해 swing을 이용하고 있다. 팀 프로젝트이다보니 여러 명이서 협업하기 위해 필요한 프레임별로 파일을 구분해서 만들었다. 물론 굳이 팀 프로젝트가 아니였더라도 기능별로 파일을 구분해서 만드는 것이 보기도 좋고 나중에 유지보수하기에도 좋다. 결국 기능별로 파일을 다 만든 뒤에 파일들을 서로 연결해야 하는데, 구글링을 해도 제대로 연결이 되지 않아 생각보다 오래 걸렸다.

내가 원한건 **버튼을 누르면 현재 클래스에서 다른 클래스로 이동** 하는 기능이었다. 그래서 구글링을 한 결과, 버튼을 누르면 이동하고자 하는 클래스인 newFrame을 만들고, setVisible() 메서드를 통해 newFrame을 보여주고 현재 클래스는 dispose() 메서드를 이용해 닫는 코드를 발견했다.

```java
button.addActionListener(new ActionListener() {
	public void actionPerformed(ActionEvent e) {
		newFrame n = new newFrame();
		n.setVisible(true);
		frame.dispose();
	}
});
```

# 에러 코드

하지만 위 코드를 사용하면 현재 클래스는 닫히지만, 이동하고자 하는 클래스가 보여지지 않는 문제가 발생했다. 왜 이러한 문제가 발생했는지 설명하기 위해 그 때 내가 적었던 코드를 참고하고자 한다.

loginFrame에서 Sign Up 버튼을 눌렀을 때 registerFrame으로 이동하는 코드이다.

> loginFrame
> ![swing1](/assets/images/java/swing1.png)

```java
import java.awt.Dimension;
import java.awt.EventQueue;
import java.awt.Graphics;
import java.awt.Image;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.List;
import java.util.Map;

import javax.swing.Icon;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JPanel;
import javax.swing.JTextField;
import javax.swing.JPasswordField;
import javax.swing.JButton;
import java.awt.Color;
import javax.swing.JLabel;
import javax.swing.JOptionPane;

import java.awt.Font;

public class loginFrame {
	private JFrame frame;
	private JTextField id;
	private JPasswordField password;

	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					loginFrame window = new loginFrame();
					window.frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the application.
	 */
	public loginFrame() {
		initialize();
		db = new DB();
	}

	/**
	 * Initialize the contents of the frame.
	 */
	private void initialize() {
		frame = new JFrame();

		ImagePanel bgPanel = new ImagePanel(new ImageIcon("./image/bg_loginFrame.jpg").getImage());
		frame.setSize(bgPanel.getWidth(), bgPanel.getHeight());
		frame.getContentPane().add(bgPanel);
		bgPanel.setLayout(null);

		this.drawText(bgPanel, new int[]{519, 312, 313, 45}); // ID
		this.drawPassword(bgPanel, new int[]{519, 421, 313, 45}); // password
		this.drawSignupButton(bgPanel);

		JButton btn_login = this.makeImageButton("login1.jpg", "login2.jpg");
		btn_login.setBounds(519, 518, 313, 57);
		bgPanel.add(btn_login);

		frame.setLocationRelativeTo(null);
		frame.setResizable(false);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

	}

	public void drawText(ImagePanel bgPanel, int[] bounds) {
		JTextField btn = new JTextField();
		btn.setBounds(bounds[0], bounds[1], bounds[2], bounds[3]);
		bgPanel.add(btn);
	}

	public void drawPassword(ImagePanel bgPanel, int[] bounds) {
		JPasswordField btn = new JPasswordField();
		btn.setBounds(bounds[0], bounds[1], bounds[2], bounds[3]);
		bgPanel.add(btn);
	}

	public void drawSignupButton(ImagePanel bgPanel) {
		JButton button = new JButton("Sign Up");
		button.setBounds(734, 585, 123, 29);
		button.setFont(new Font("Arial", Font.BOLD, 17));
		button.setBorderPainted(false);
		button.setFocusPainted(false);
		button.setForeground(Color.WHITE);
		button.setBackground(Color.BLACK);

		button.addActionListener(new ActionListener() {
			public void actionPerformed(ActionEvent e) {
				registerFrame r = new registerFrame();
				r.setVisible(true);
				frame.dispose();
			}
		});

		bgPanel.add(button);
	}

	public JButton makeImageButton(String img, String hoverImg) {
		Icon IMG = new ImageIcon("./image/btn/" + img);
		Icon IMG_HOVER = new ImageIcon("./image/btn/" + hoverImg);
		JButton btn = new JButton();

		btn.setIcon(IMG);
		btn.setRolloverIcon(IMG_HOVER);

		return btn;
	}
}
```

> registerFrame
> ![swing2](/assets/images/java/swing2.png)

```java
import java.awt.EventQueue;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JTextField;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.List;
import java.util.Map;

import javax.swing.JPasswordField;
import javax.swing.JButton;
import java.awt.Color;

public class registerFrame {

	private JFrame frame;
	private JTextField id;
	private JPasswordField password;
	private JTextField name;

	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					registerFrame window = new registerFrame();
					window.frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the application.
	 */
	public registerFrame() {
		initialize();
	}

	/**
	 * Initialize the contents of the frame.
	 */
	private void initialize() {
		frame = new JFrame();

		ImagePanel bgPanel = new ImagePanel(new ImageIcon("./image/bg_registerFrame.jpg").getImage());
		frame.setSize(bgPanel.getWidth(), bgPanel.getHeight());
		frame.getContentPane().add(bgPanel);
		bgPanel.setLayout(null);

		this.drawIdFeild(bgPanel);
		this.drawPwFeild(bgPanel);
		this.drawNameFeild(bgPanel);
		this.drawRegisterBtn(bgPanel);

		frame.setLocationRelativeTo(null);
		frame.setResizable(false);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	}

	public void drawIdFeild(ImagePanel bgPanel) {
		id = new JTextField();
		id.setBounds(519, 387, 313, 45);
		id.setColumns(10);
		bgPanel.add(id);
	}

	public void drawPwFeild(ImagePanel bgPanel) {
		password = new JPasswordField();
		password.setBounds(519, 495, 313, 45);
		bgPanel.add(password);
	}

	public void drawNameFeild(ImagePanel bgPanel) {
		name = new JTextField();
		name.setBounds(519, 279, 313, 45);
		name.setColumns(10);
		bgPanel.add(name);
	}

	public void drawRegisterBtn(ImagePanel bgPanel) {
		JButton btn_register = new JButton("New button");
		btn_register.setBackground(Color.LIGHT_GRAY);
		btn_register.setForeground(Color.WHITE);
		btn_register.setIcon(new ImageIcon("./image/btn/register1.jpg"));
		btn_register.setRolloverIcon(new ImageIcon("./image/btn/register2.jpg"));
		btn_register.setBounds(519, 586, 313, 57);
		btn_register.addActionListener(new RegisterAction());
		bgPanel.add(btn_register);
	}
}
```

# 에러 발생 이유

왜 문제가 발생했는지 확인하기 위해서 전체 코드를 가져왔더니 소스 코드가 길지만, 위 코드에서 문제점을 확인할 수 있다. loginFrame에서 drawSignupButton() 메서드를 보면 registerFrame을 생성해서 setVisible() 메서드를 이용해 registerFrame을 보여주려고 한다. 바로 처음에 구글링을 통해 찾았던 방법으로 말이다. **하지만 문제는 registerFrame 클래스를 확인해보면 registerFrame은 JFrame이 아닌 단순히 클래스이기 때문에 JFrame의 메소드인 setVisible()을 사용할 수가 없다.**

### `setVisble()` 메서드를 사용하기 위한 2가지 방법

1. registerFrame이 JFrame을 상속받기

   ```java
   public class registerFrame extends JFrame {

   }
   ```

2. registerFrame에 setVisible() 메서드 추가하기

   ```java
   public void setVisible(boolean b) {
   		frame.setVisible(b);
   }
   ```

# 에러 코드 해결

위에서 말한 2가지 방법 중 두 번째 방법을 사용해서 loginFrame을 registerFrame과 연결했다. 아래 코드는 두 번째 방법을 적용한 registerFrame 코드 이다.

```java
import java.awt.EventQueue;
import javax.swing.ImageIcon;
import javax.swing.JFrame;
import javax.swing.JOptionPane;
import javax.swing.JTextField;

import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.List;
import java.util.Map;

import javax.swing.JPasswordField;
import javax.swing.JButton;
import java.awt.Color;

public class registerFrame {

	private JFrame frame;
	private JTextField id;
	private JPasswordField password;
	private JTextField name;

	/**
	 * Launch the application.
	 */
	public static void main(String[] args) {
		EventQueue.invokeLater(new Runnable() {
			public void run() {
				try {
					registerFrame window = new registerFrame();
					window.frame.setVisible(true);
				} catch (Exception e) {
					e.printStackTrace();
				}
			}
		});
	}

	/**
	 * Create the application.
	 */
	public registerFrame() {
		initialize();
	}

	/**
	 * Initialize the contents of the frame.
	 */
	private void initialize() {
		frame = new JFrame();

		ImagePanel bgPanel = new ImagePanel(new ImageIcon("./image/bg_registerFrame.jpg").getImage());
		frame.setSize(bgPanel.getWidth(), bgPanel.getHeight());
		frame.getContentPane().add(bgPanel);
		bgPanel.setLayout(null);

		this.drawIdFeild(bgPanel);
		this.drawPwFeild(bgPanel);
		this.drawNameFeild(bgPanel);
		this.drawRegisterBtn(bgPanel);

		frame.setLocationRelativeTo(null);
		frame.setResizable(false);
		frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
	}

	public void drawIdFeild(ImagePanel bgPanel) {
		id = new JTextField();
		id.setBounds(519, 387, 313, 45);
		id.setColumns(10);
		bgPanel.add(id);
	}

	public void drawPwFeild(ImagePanel bgPanel) {
		password = new JPasswordField();
		password.setBounds(519, 495, 313, 45);
		bgPanel.add(password);
	}

	public void drawNameFeild(ImagePanel bgPanel) {
		name = new JTextField();
		name.setBounds(519, 279, 313, 45);
		name.setColumns(10);
		bgPanel.add(name);
	}

	public void drawRegisterBtn(ImagePanel bgPanel) {
		JButton btn_register = new JButton("New button");
		btn_register.setBackground(Color.LIGHT_GRAY);
		btn_register.setForeground(Color.WHITE);
		btn_register.setIcon(new ImageIcon("./image/btn/register1.jpg"));
		btn_register.setRolloverIcon(new ImageIcon("./image/btn/register2.jpg"));
		btn_register.setBounds(519, 586, 313, 57);
		btn_register.addActionListener(new RegisterAction());
		bgPanel.add(btn_register);
	}

	public void setVisible(boolean b) {
		frame.setVisible(b);
	}

	public void dispose() {
		frame.dispose();
	}
}
```
