# Alarm_Clock-JAVA
Create an Alarm clock program using JAVA and amaze your friends.

This is an Alarm Clock project created in JAVA using JFrame and other Swing Components.

Alarm Clock- A utility that everyone needs.
You all must have used Alarm for atleast once in your life. This code is written by Ayush and is free to use and is complete within 400 lines of code. The GUI is awesome and its working perfectly fine. You can set a default tone as well and you can select any other tone too.
The code is short and easy to understand.

Features of the code:
* Can add upto 3 alarms.
* If audio is not selected while setting alarm, default tone will get selected.
* Different audios can be selected for different alarms.
* Can edit and delete a perticular alarm.
* Ui looks great


Check Sample Video (v1):

[![Sample](https://user-images.githubusercontent.com/119154806/218260002-71dae93d-51ed-40db-9b00-dba0364451df.png)](https://youtu.be/PQ16ggjYI-8 "Alarm v1 Sample Video")


Check Screenshots:
<details>
  <summary>Click Here To Check</summary>
  <img src="https://user-images.githubusercontent.com/119154806/218260108-cd55820d-7c57-4efd-9a4b-044c93b8e4e4.png" name="Screenshot_vlc_20230211184225">
  <img src="https://user-images.githubusercontent.com/119154806/218260112-35ef5a23-2dda-4ea9-961d-994a18cdf03c.png" name="Screenshot_vlc_20230211184251">
  <img src="https://user-images.githubusercontent.com/119154806/218260118-2196d7ae-9542-42b9-8bfa-55156ddfe9c9.png" name="Screenshot_vlc_20230211184315">
  <img src="https://user-images.githubusercontent.com/119154806/218260120-af510729-5789-4bab-9671-9e791324650f.png" name="Screenshot_vlc_20230211184333">
  <img src="https://user-images.githubusercontent.com/119154806/218260123-3c891b1d-c982-4654-9cfa-3efc83d70d33.png" name="Screenshot_vlc_20230211184433">
  <img src="https://user-images.githubusercontent.com/119154806/218260126-37158e28-35e6-4be8-8af6-737d076b20dd.png" name="Screenshot_vlc_20230211184505">
  <img src="https://user-images.githubusercontent.com/119154806/218260127-84c4afb7-939f-4752-a112-17ce354f46da.png" name="Screenshot_vlc_20230211184526">
</details>


Check the code:

  	import java.awt.*;
	import java.awt.event.*;
	import java.io.*;
	import java.net.URL;
	import java.time.LocalDateTime;
	import java.util.Calendar;
	import javax.sound.sampled.*;
	import javax.swing.*;
	import javax.swing.filechooser.FileNameExtensionFilter;

	public class MyFrame extends JFrame implements ActionListener{

	private static final long serialVersionUID = 1L;
	URL def = getClass().getResource("music.wav");
	AudioInputStream audio;
	Clip clip;
	JComboBox<Object> ap;
	JLabel lab[]= new JLabel[6];
	JTextField field[] = new JTextField[4];
	JButton num[] = new JButton[4];
	JPanel panel = new JPanel();
	JPanel player = new JPanel();
	JPanel pan[] = new JPanel[3];
	String mer;
	String alarms[][] = {{"","",""},{"","",""}, {"","",""}};
	JLabel alab[]= new JLabel[3];
	JButton butt[] = new JButton[8];
	Boolean del=false, edit=false;
	JFileChooser filec;
	FileNameExtensionFilter filter;
	File f = new File("");
	int count=0, playing, x;
	
	void CurrentTime() {
		while(true) {
			LocalDateTime now = LocalDateTime.now();
			Calendar cal = Calendar.getInstance();
			int mm=cal.get(Calendar.AM_PM);
			if(mm==1) 
				mer="PM";
			else
				mer="AM";
			int z = now.getHour();
			if(z>12)
				z=z-12;
			lab[0].setText(z+":"+now.getMinute()+":"+now.getSecond()+" "+mer);
			if(!alarms[2][0].equalsIgnoreCase(""))
				num[0].setEnabled(false);
			else
				num[0].setEnabled(true);
			for(int i=0; i<3; i++) {
				if(String.valueOf(z).equals(alarms[i][0]) && String.valueOf(now.getMinute()).equals(alarms[i][1]) && mer.equals(alarms[i][2]) && String.valueOf(now.getSecond()).equals("0")) {
					for(int l = 0; l<3; l++)
						butt[l+2].setEnabled(false);
					clip.start();
					setExtendedState(JFrame.MAXIMIZED_BOTH);
					playing = i;
					pan[2].setVisible(false);
					player.setVisible(true);
					panel.setVisible(false);
				}
			}
		}
	}
	
	void settings_disp() {
		panel.setVisible(true);
		if(!(alab[1].getText().equalsIgnoreCase("")))
			pan[1].setVisible(false);
		if(!(alab[2].getText().equalsIgnoreCase("")))
			pan[2].setVisible(false);
	}
	
	void done_internal(int c) {
		if(ap.getSelectedItem()==null)
			alarms[c][2] = mer;
		else
			alarms[c][2] = (String) ap.getSelectedItem();
		alab[c].setText(field[0].getText()+":"+field[1].getText()+" "+ alarms[c][2]);
		for(int i=0;i<2;i++) {
			alarms[c][i] = field[i].getText();
			field[i].setText("");
		}
		panel.setVisible(false);
	}
	
	void done(int z) {
		try {
			if(!f.isFile()) {
				audio = AudioSystem.getAudioInputStream(def);
				clip = AudioSystem.getClip();
				clip.open(audio);
			}
			if(Integer.parseInt(field[0].getText())<=12 && Integer.parseInt(field[1].getText())<=59 && count<3 && !edit) {
				done_internal(count);
				count++;
			}
			else if(Integer.parseInt(field[0].getText())<=12 && Integer.parseInt(field[1].getText())<=59 && edit)
				done_internal(z);
			else
				panel.setVisible(false);
			edit=false;
			vis();
		} catch (NumberFormatException | UnsupportedAudioFileException | IOException | LineUnavailableException e1) {
			System.out.println("Please Enter A Valid Time!!!");
		}
	}
	
	void vis() {
		for(int i=0; i<3; i++) {
			if(!alarms[i][0].equals(""))
				pan[i].setVisible(true);
		}
	}
	
	void stop_or_del(int a) {
			for(int j=0; j<3; j++) {
				if(a==0) {
					if(alarms[1][0].equals("")) {
						for(int i=0; i<3; i++)
							alarms[0][i] = "";
						pan[0].setVisible(false);
					}
					else if(alarms[2][0].equals("")) {
						for(int i=0; i<3; i++) {
							alarms[0][i]=alarms[1][i];
							alarms[1][i] = "";
						}
						pan[1].setVisible(false);
					}
					else {
						for(int i=0; i<3; i++) {
							alarms[0][i]=alarms[1][i];
							alarms[1][i]=alarms[2][i];
							alarms[2][i] = "";
						}
						pan[2].setVisible(false);
					}
				}
					if(a==1) {
						if(alarms[2][0].equals("")) {
							for(int i=0; i<3; i++)
								alarms[1][i] = "";
							pan[1].setVisible(false);
						}
						else {
							for(int i=0; i<3; i++) {
								alarms[1][i]=alarms[2][i];
								alarms[2][i] = "";
							}
							pan[2].setVisible(false);
						}
					}
					if(a==2) {
							for(int i=0; i<3; i++)
								alarms[2][i] = "";
							pan[2].setVisible(false);
						}
					j=3;
					}
			for(int k=0;k<3;k++)
				alab[k].setText(alarms[k][0]+":"+alarms[k][1]+" "+ alarms[k][2]);
			if((del && clip.isActive() && playing == a) || !del) {
				for(int l = 0; l<3; l++)
					butt[l+2].setEnabled(true);
				clip.stop();
				clip.setMicrosecondPosition(0);
				player.setVisible(false);
			}
			count--;
			del=false;
				}
	
	void editing(int a) {
		field[0].setText(alarms[a][0]);
		field[1].setText(alarms[a][1]);
		ap.setSelectedItem(alarms[a][2]);
		settings_disp();
		edit=true;
	}
	
	MyFrame(){
		
		String nam[] = {"Add Alarm", "Cancel", "Done", "Select"};	
		String labels[] = {"", "Hour:", "Minutes:", "AM/PM", "SETTINGS", "OPTIONS: "};
		for(int i=0; i<=5; i++) {
			if(i<4) {
			field[i] = new JTextField();
			field[i].setFont(new Font("Comic Sans MS",Font.PLAIN,20));
			field[i].setBackground(Color.MAGENTA);
			
			num[i] = new JButton(nam[i]);
			num[i].setBackground(new Color(123,100,255));
			num[i].setFont(new Font("Comic Sans MS", Font.PLAIN, 15));
			num[i].setFocusable(false);
			num[i].setVisible(true);
			num[i].addActionListener(this);
			}
			lab[i] = new JLabel(labels[i]);
			lab[i].setOpaque(true);
			lab[i].setFont(new Font("Comic Sans MS", Font.PLAIN, 15));
			lab[i].setVisible(true);
			if(i==1 || i==2 || i==5 || i==3) {
				lab[i].setBackground(Color.BLACK);
				lab[i].setForeground(Color.WHITE);
			}
			else {
				lab[i].setHorizontalAlignment(JLabel.CENTER);
				lab[i].setBackground(Color.RED);
			}
		}
		
		String[] a = {"AM","PM"};
		ap = new JComboBox<Object>(a);
		ap.setSelectedItem(mer);
		ap.addActionListener(this);
		
		String help[] = {"Snooze", "Stop", "Edit", "Edit", "Edit", "Delete", "Delete", "Delete"};
		for(int i=0; i<8; i++) {
			butt[i] = new JButton(help[i]);
			butt[i].setBackground(new Color(123,100,255));
			butt[i].setFont(new Font("Comic Sans MS", Font.PLAIN, 15));
			butt[i].setFocusable(false);
			butt[i].setVisible(true);
			butt[i].addActionListener(this);
		}
		
		//POSITIONS:
		num[1].setBounds(20, 160, 100, 30);
		num[2].setBounds(145, 160, 100, 30);
		num[3].setBounds(145, 120, 100, 25);
		lab[0].setBounds(20, 35, 300, 50);
		num[0].setBounds(105, 95, 130, 30);
		lab[1].setBounds(20, 38, 100, 25);
		field[0].setBounds(20, 65, 100, 25);
		lab[2].setBounds(145, 38, 100, 25);
		field[1].setBounds(145, 65, 100, 25);
		lab[3].setBounds(20, 95, 100, 25);
		ap.setBounds(20, 120, 100, 25);
		lab[4].setBounds(10, 10, 250, 25);
		butt[0].setBounds(20, 50, 100, 30);
		butt[1].setBounds(145, 50, 100, 30);
		lab[5].setBounds(95,12,100,25);
	
		field[3] = new JTextField("AUDIO:");
		field[3].setEditable(false);
		field[3].setForeground(Color.WHITE);
		field[3].setFont(new Font("Comic Sans MS",Font.PLAIN,15));
		field[3].setBorder(null);
		field[3].setBackground(Color.BLACK);
		field[3].setBounds(145, 95, 100, 25);
		
		//SETTINGS PANEL
		panel.setBounds(30, 200, 270, 200);
		panel.setLayout(null);
		for(int i=1; i<=4; i++) {
			if(i<4) {
				panel.add(num[i]);
				panel.add(field[i]);
			}
			panel.add(lab[i]);
		}
		panel.add(field[0]);
		panel.add(ap);
		panel.setBackground(Color.BLACK);
		panel.setVisible(false);
		
		//Snooze/Stop Panel
		player.setBounds(30, 310, 270, 100);
		player.setLayout(null);
		player.setBackground(Color.BLACK);
		player.setVisible(false);
		player.add(butt[0]);
		player.add(butt[1]);
		player.add(lab[5]);
		
		//Alarm info panels
		for(int i=0; i<3; i++) {
			alab[i] = new JLabel();
			alab[i].setOpaque(true);
			alab[i].setBounds(10,20 , 180, 50);
			alab[i].setBackground(Color.LIGHT_GRAY);
			alab[i].setForeground(Color.BLACK);
			alab[i].setFont(new Font("Comic Sans MS", Font.PLAIN, 15));
			alab[i].setVisible(true);
			
			butt[i+2].setFont(new Font("Comic Sans MS", Font.PLAIN, 12));
			butt[i+2].setBounds(200,10 , 80, 25);
			butt[i+5].setFont(new Font("Comic Sans MS", Font.PLAIN, 12));
			butt[i+5].setBounds(200,40 , 80, 25);
			
			pan[i] = new JPanel();
			pan[i].setBounds(30, 200, 270, 200);
			pan[i].setBackground(Color.LIGHT_GRAY);
			pan[i].setLayout(null);
			pan[i].setVisible(false);
			pan[i].add(alab[i]);
			pan[i].add(butt[i+2]);
			pan[i].add(butt[i+5]);
		}

		pan[0].setBounds(20, 135, 300, 70);
		pan[1].setBounds(20, 220, 300, 70);
		pan[2].setBounds(20, 310, 300, 70);
		
		//FRAME
		this.setLocationRelativeTo(null);
		this.setDefaultCloseOperation(JFrame.DO_NOTHING_ON_CLOSE);
		this.addWindowListener(new WindowAdapter() {
		    public void windowClosing(WindowEvent e) {
		        setExtendedState(JFrame.ICONIFIED);
		    }
		});
		this.setResizable(false);
		this.setLayout(null);
		this.setSize(new Dimension(350,460));
		this.getContentPane().setBackground(Color.WHITE);
		for(int i=0;i<3;i++)
			this.add(pan[i]);
		this.add(lab[0]);
		this.add(num[0]);
		this.add(panel);
		this.add(player);
		this.setVisible(true);
		
		CurrentTime();
		}

	@Override
	public void actionPerformed(ActionEvent e) {
		if(e.getSource()==num[0])
			settings_disp();
		if(e.getSource()==num[1]) {
			panel.setVisible(false);
			field[0].setText("");
			field[1].setText("");
			vis();
		}
		if(e.getSource()==num[3]) {
			filec = new JFileChooser();
			filter = new FileNameExtensionFilter("WAV FILES","wav");
			filec.setFileFilter(filter);
			if(filec.showOpenDialog(null)==JFileChooser.APPROVE_OPTION)
				f = new File(filec.getSelectedFile().getAbsolutePath());
			try {
				audio = AudioSystem.getAudioInputStream(f);
				clip = AudioSystem.getClip();
				clip.open(audio);
			} catch (UnsupportedAudioFileException | IOException | LineUnavailableException e1) {}
		}
		if(e.getSource()==num[2]) {
			if(edit)
				done(x);
			else
				done(5);
		}
		if(e.getSource()==butt[0]) {
			for(int i=0; i<3; i++) {
			if(playing == i) {
				player.setVisible(false);
				clip.stop();
				clip.setMicrosecondPosition(0);
				if((Integer.parseInt(alarms[i][1]) + 5)>59) {
					alarms[i][1]=String.valueOf(Integer.parseInt(alarms[i][1]) + 5 - 59);
					if(Integer.parseInt(alarms[i][0])==11) {
						alarms[i][0]= "12";
						if(alarms[i][2].equals("AM"))
							alarms[i][2]="PM";
						else
							alarms[i][2]="AM";
					}
					else
						alarms[i][0]= String.valueOf(Integer.parseInt(alarms[i][0]) + 1);
				}
				else 
					alarms[i][1] = String.valueOf(Integer.parseInt(alarms[i][1]) + 5);
				alab[i].setText(alarms[i][0]+":"+alarms[i][1]+" "+ alarms[i][2]);
			}
			}
			if(!alarms[2][1].equals(""))
				pan[2].setVisible(true);
			for(int l = 0; l<3; l++)
				butt[l+2].setEnabled(true);
		}
		for(int i=5;i<8;i++){
			if(e.getSource()==butt[i]) {
				del=true;
				stop_or_del(i-5);
				}
		}
		for(int i=2;i<5;i++){
			if(e.getSource()==butt[i]) {
				x=i-2;
				editing(x);
			}
		}
		if(e.getSource()==butt[1])
			stop_or_del(playing);
	}
	}

Details about code:
* The code is self explanatory.
* Contact me on Telegram @SOUL_AYU if you need any help.
* You have to create a folder named "audio" outside src folder and paste any audio which have .wav extantion with filename as music.wav to set it as default tone. Check this video for help: https://www.youtube.com/watch?v=IgfcSr3NlG0&ab_channel=RyiSnow

Issues:
* Maximum alarms that can be set is 3.
* Can set alarm according to 12 Hour clock only.
* GUI sometime misbehaves.

Conclusion:
This program's UI is Amazing and the code is short as well. There are some limitations which are discussed above. You can get the code from here and Jar and Exe file from release section of this repo.

Thanks for reading till end, please consider checking my other repos as well.
