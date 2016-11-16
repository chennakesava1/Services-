# Services-

##------------------------------------##

##DNS Service requied packages##

 install "bind" package ( this is the DNS package )
 
 /etc/named.conf (configaration file ) : 
     listen-on port 53 { 127.0.0.1; };
     TO:
     listen-on port 53 { 127.0.0.1; 10.1.1.110; };
     
  check DNS port 53 
    netstat -ant | grep -w 53 or lsof -i :53
              -a : all
              -t TCP protocol
              -n numeric, don't use name
 open firewall to allow DNS queires from external
      firewall-cmd --zone=public --add-port=53/tcp --permanent
      firewall-cmd --zone=public --add-port=53/udp --permanent
      firewall-cmd --reload
      
 check from anether host is 53 is opend or not 
 
      nmap -p 53 10.1.1.110
      nmap -sU -p 53 10.1.1.110
      
      
      
  Zone file configuration
       need to create a directory "mkdir -p /etc/bind/zones/master/"
       creare a nrew file /etc/bind/zones/master/db.linuxconfig.org
          
          
          ;
; BIND data file for linuxconfig.org
;
$TTL    3h
@       IN      SOA       linuxconfig.org admin.linuxconfig.org. (
                          1        ; Serial
                          3h       ; Refresh after 3 hours
                          1h       ; Retry after 1 hour
                          1w       ; Expire after 1 week
                          1h )     ; Negative caching TTL of 1 day
;

@       IN      NS      ns1.rhel7.local.
@       IN      NS      ns2.rhel7.local.

linuxconfig.org.    		IN      A       1.1.1.1
www 				IN      A       1.1.1.1


=00----

Allow DNS quears from external sources
 /ets/named.conf
    FROM:
    allow-query     { localhost; };
    TO:
    allow-query     { any; };

restart the service
  service named restart
  
  
Testing DNS Server 

   dig @o.o.o.o www.example.com
   
   
   
   
