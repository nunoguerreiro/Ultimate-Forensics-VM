***Had issues with APT updates breaking and I don't feel like messing with APT problems.  Therefore I am attempting to move to a Docker based forensics VM.  See the docker directory for more information.***  
The below may still work but I don't feel like troubleshooting the APT conflicts.

# Ultimate-Forensics-VM
Evolving directions on building the best Open Source Forensics VM


**VM minimum config recommendations:**  
- 2 procs  
- 4GB RAM  
- 30GB disk space  
- 2 NICs  
  - One shared or direct connect  
  - One host only  
      - Use this NIC for the SO monitor interface  
      - Replay your PCAPs on this interface  


**Install Ubuntu 14.04 LTS and run:**  
sudo apt-get update  
sudo apt-get upgrade  
sudo apt-get update  
sudo apt-get dist-upgrade  
sudo reboot  

**Install SIFT Workstation:**  
wget --quiet -O - https://raw.github.com/sans-dfir/sift-bootstrap/master/bootstrap.sh | sudo bash -s -- -i  
sudo apt-get update  
sudo apt-get upgrade  
sudo reboot  

**Install Remnux Tools:**  
wget --quiet -O - https://remnux.org/get-remnux.sh | sudo bash  
sudo apt-get update  
sudo apt-get upgrade  
sudo reboot  

(above three steps sourced from: https://digital-forensics.sans.org/blog/2015/06/13/how-to-install-sift-workstation-and-remnux-on-the-same-forensics-system)  


**Install JSDetox Docker:**  
	- Awesome JavaScript forensic tool: http://www.relentless-coding.com/projects/jsdetox/  
sudo apt-get install docker  
sudo apt-get update  
sudo apt-get upgrade  
***Run JSDetox:***  
docker run  
sudo docker run --rm -p 3000:3000 remnux/jsdetox  
To stop JSDetox --> use "sudo docker ps -l" to obtain the container ID, then use the "sudo docker stop *container-id*" and wait about a minute.  
Source: https://hub.docker.com/r/remnux/jsdetox/  


**Install SecurityOnion:**  
echo "debconf debconf/frontend select noninteractive" | sudo debconf-set-selections  
sudo apt-get -y install software-properties-common  
sudo add-apt-repository -y ppa:securityonion/stable  
sudo apt-get update  
sudo apt-get -y install securityonion-all syslog-ng-core  
Now setup SecurityOnion as a standalone server.  
	- You can now run PCAPs against the monitor interface and have Bro and Suricata run against them.  
	- Install all of your custom Bro scripts  
	- View the results of the PCAP replay in Sguil and ELSA.  
Source: https://github.com/Security-Onion-Solutions/security-onion/wiki/InstallingOnUbuntu  


**Install Libemu:**  
sudo apt-get install python-libemu  


**Install YARA rules:** 
https://github.com/Yara-Rules/rules  
mkdir yara  
cd yara  
git clone https://github.com/Yara-Rules/rules.git  
**Update to latest YARA version and enable various support:**  
Download latest version of Yara  
tar -xzf yara-3.X.0.tar.gz   
cd yara-3.4.0/  
./bootstrap.sh  
sudo apt-get install libjansson-dev  
./configure --with-crypto --enable-cuckoo --enable-magic  
make  
sudo make install  
cd yara-python/  
python setup.py build  
sudo python setup.py install  
yara -v  
**more work to do here, not sure if this is the best way forward


**Update SIFT and Remnux:**  
update-sift  
update-remnux  


**If you have any APT keys that need added run the below command.**  
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys KEY  


**Reset ELSA DB**  
(good to do before starting a new investigation)  
securityonion-elsa-reset  
