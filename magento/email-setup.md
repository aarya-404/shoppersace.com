# Email Setup In Magento


![logo](https://docsify.js.org/_media/icon.svg ':size=50x100')


**Steps To Install**

 - *Step 1*
 
  Install msmtp And ca-certificates
  
   ```bash
    sudo apt-get install msmtp ca-certificates
   ```
 
 - *Step 2*
  Edit the file msmtprc

  ```bash
    sudo nano /etc/msmtprc
   ```

 - *Step 3*

  Paste the below code in the file and save the file :
 
  ```bash 
    defaults
    tls on
    tls_starttls on
    tls_trust_file /etc/ssl/certs/ca-certificates.crt
     
    account default
    host smtp.gmail.com
    port 587
    auth on
    user my.gmail.id@gmail.com
    password gmail-password
    from my.gmail.id@gmail.com
    logfile /var/log/msmtp.log
  ```

 - *Step 4*

  Crate the a log file and give the permission: 
  ```bash
  sudo touch /var/log/msmtp.log

  sudo chmod 0644 /etc/msmtprc
  ```

   - *Step 5*

   ****For testing mail is it working or not**** ,run below code in the terminal
   if your mail not working then check your credential and also make sure that you have enabled less secure app in your mail provider

   ```bash
  echo -e "Subject: Test Mail\r\n\r\nThis is my first test email." |msmtp --debug --from=default -t my.gmail.id@gmail.com
  ```

   - *Step 6*

  then Setup it in php.ini
  ```bash
  sudo nano /etc/php5/apache2/php.ini
  ```
  You have to set sendmail path in your php.ini file.Search "sendmail_path" and edit as below
  Check your SMTP path with

  ```bash
  root@welcome-Vostro1510:~$ which msmtp
  ```
  and you will get /usr/bin/msmtp like that. sendmail_path = '/usr/bin/msmtp -t' now save and exit from

  - *Step 7*


  ```bash
  sudo /etc/init.d/apache2 restart
  ```
