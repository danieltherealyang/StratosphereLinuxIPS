# Features

Slips is a behavioral-based IPS that uses machine learning to detect malicious behaviors in the network traffic. It is a modular software that can be extended. When Slips is run, it spawns several child processes to manage the I/O, to profile attackers and to run the detection modules.

Here we describe what Slips can take as input, what it produces as an output, and what detection modules are run on the traffic to detect malicious behaviour.

## Input

The input process reads flows of different types:

- Pcap files (internally using Zeek) 
- Packets directly from an interface (internally using Zeek)
- Suricata flows (from JSON files created by Suricata, such as eve.json)
- Argus flows (CSV file separated by commas or TABs) 
- Zeek/Bro flows from a Zeek folder with log files
- Nfdump flows from a binary nfdump file

All the input flows are converted to an internal format. So once read, Slips works the same with all of them. 

**_Note: look at ./slips.py --help to find correct argument to run Slips on each type of the file._**

## Output
The output process collects output from the modules and handles the display of information on screen. Currently, Slips' analysis and detected malicious behaviour can be analyzed as following:
	
- Kalipso - Node.JS based graphical user interface in the terminal. Kalipso displays Slips detection and analysis in colorful table and graphs, highlighting important detections. See section Kalipso for more explanation. 
- alerts.json and alerts.txt - collects all evidences and detections generated by Slips in a .txt and .json formats.
- log files in a folder _current-date-time_ - separates the traffic into files according to a profile and timewindow and summarize the traffic according to each profile and timewindow.
	

## Detection Modules

Modules are Python-based files that allow any developer to extend the functionality of Slips. They process and analyze data, perform additional detections and store data in Redis for other modules to consume. Currently, Slips has the following modules:


<style>
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
}

td, th {
  border: 1px solid #dddddd;
  text-align: left;
  padding: 8px;
}

tr:nth-child(even) {
  background-color: #dddddd;
}
</style>


<table>
  <tr>
    <th>Module</th>
    <th>Description</th>
    <th>Status</th>
  </tr>
  <tr>
    <td>asn</td>
    <td>loads and finds the ASN of each IP</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>geoip</td>
    <td>finds the country and geolocation information of each IP</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>https</td>
    <td>training&test of RandomForest to detect malicious https flows</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>port scan detector</td>
    <td>detects Horizontal and Vertical port scans</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>threat intelligence</td>
    <td>checks if each IP is in a list of malicious IPs</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>timeline</td>
    <td>creates a timeline of what happened in the network based on all the flows and type of data available</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>rnn-cc-detection</td>
    <td>detects command and control channels using recurrent neural network and the stratosphere behavioral letters</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>VirusTotal</td>
    <td>module to lookup IP address on VirusTotal</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>flowalerts</td>
    <td>module to find malicious behaviour in each flow. Current measures are: long duration of the connection, successful ssh</td>
    <td>✅</td>
  </tr>
  <tr>
    <td>blocking</td>
    <td>module to block malicious IPs connecting to the device</td>
    <td>⚠️</td>
  </tr>
  
</table>

If you want to contribute: improve existing Slips detection modules or implement your own detection modules, see section :doc:`Contributing <contributing>`.

