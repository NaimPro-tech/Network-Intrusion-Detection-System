# Feature Details

## Basic/Intrinsic Features

    Feature   | Meaning  | Why important

 0   duration --> running time of a connection --> Scan attack normally takes less time
 1   protocol_type --> tcp, udp, icmp --> some attack are done through specific protocol
 2   service --> network service: http,ftp,telnet,smtp --> refers to an attack targeting a specific service
 3   flag --> connection status(SF=normal complete, SO=connection attempt rejected, REJ=rejected) --> flag abnormal in many attacks
 4   src_bytes --> bytes that send from source to destination --> abnormally high/low is suspicious
 5   dst_bytes --> bytes that returned from destination to source --> zero in scan  
 6   land --> source and destination ip/port same or not(0/1) --> this is an attack type itself
 7   wrong_fragment --> wrong/malformed fragment numbers --> Network level attack indications
 8   urgent --> Urgent Packet number --> rarely used, suspicious if unusual

## Content Features(Analysis the characteristics of payload)

- This features has derived by domain experts to understand what is happening inside a connection. This features is to catch R2L, U2R attack because this type of attack seems normal in traffic pattern but something exploit inside.

 9   hot --> hot indicator numbers(ex: system directory access, program execution)
 10  num_failed_logins --> how many time login have failed
 11  logged_in --> successfully loged in or not
 12  num_compromised --> how many time compromised
 13  root_shell --> Found root shell or not; direct indication of privilege escalation
 14  su_attempted --> trying su root command or not
 15  num_root --> time of access as root
 16  num_file_creations --> number of file creations
 18  num_access_files --> number of access into access control file
 17  num_shells --> how many times shell prompt has been open
 19  num_outbound_cmds --> number of command in outbound FTP session
 20  is_host_login --> logged in user is in hot list or not
 21  is_guest_login --> logged in user is guest or not

## Time based Traffic Features( pattern of 2 second window)

- Measures how many connection are being made to the same host/service at the same time- the most important group for detecting DoS and Probe/Scan attacks

 22  count --> total connection in same destination host in last 2 sec
 23  srv_count --> total connection in same service in last 2 sec
 24  serror_rate --> SYN error connection percentage in same host
 25  srv_serror_rate --> SYN error connection percentage in same connection
 26  rerror_rate --> REJ error connection percentage in same host
 28  same_srv_rate --> REJ error connection percentage in same connection
 27  srv_rerror_rate --> REJ error connection percentage in the same service
 29  diff_srv_rate --> connection rates for different services
 30  srv_diff_host_rate --> connection rate to the same service but different host

## Intuition

- Count(thousand connection in same host) increases a lot in DoS attack, & diff_srv_rate increases in diff_srv_rate(try different service to check which one is open).

## Host based Traffic Features (pattern in 100 connection window)

 31  dst_host_count --> How many of the last 100 connections were to the same destination host?
 32  dst_host_srv_count --> How many of the last 100 connections were to the same host and service
 33  dst_host_same_srv_rate --> Rate of going to the same service
 34  dst_host_diff_srv_rate --> Rate of going to the different service
 35  dst_host_same_src_port_rate --> Rate of using same source port
 36  dst_host_srv_diff_host_rate --> Rate of same service but going to different host
 37  dst_host_serror_rate --> SYN error rate(host based)
 38  dst_host_srv_serror_rate --> SYN error rate (host+service based)
 39  dst_host_rerror_rate --> REJ error rate (host based)
 40  dst_host_srv_rerror_rate --> REJ error (host+service based)
 41  label --> Attack category['normal','neptune','warezclient','ipsweep','portsweep','teardrop','nmap','satan',
'smurf','pod','back','guess_passwd','ftp_write','multihop','rootkit','buffer_overflow','imap','warezmaster','phf','land','loadmodule','spy','perl']

 42  difficulty_level --> It is derived from a research where 7 model train 3 times and predict all the records. value that close to 21 means the record is ovious pattern and easy to figure, low value means the record is tricky, complex, majority model failed to catch it. (Do not use as features)
