### uninstall nginx ubuntu 24.04
```
sudo apt-get remove nginx nginx-common -y
sudo apt-get purge nginx nginx-common -y
sudo apt-get autoremove
```
 ### Uninstall Apache2
 ```
sudo service apache2 stop
sudo apt-get purge apache2 apache2-utils apache2.2-bin apache2-common
sudo apt-get purge apache2
sudo apt remove apache2*
sudo rm -Rf /etc/apache2 /usr/lib/apache2 /usr/include/apache2
sudo apt-get autoremove --purge
 ```
