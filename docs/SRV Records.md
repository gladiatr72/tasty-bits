
# SRV Records

Service records became an Official Internet Cog in the year 2000 with the IETF’s
publication of the mercifully short [RFC2782](https://tools.ietf.org/html/rfc2782).

There have been dozens of articles, posts, etc
on the topic--this is not going to be one of those; only some brief notes for
the glancing (think of it as an _unstructured cheatsheet_) with some annotations
towards how they relate to current service discovery practices.

## typical public SRV record


```

gladiatr@lilbuddy-quatre.local
[~]
:# dig _sip._udp.sip.voice.google.com SRV

; [...]
;; ANSWER SECTION:
_sip._udp.[...].  218 IN  SRV   20 1 5060 sip-anycast-2.voice.google.com.
_sip._udp.[...].  218 IN  SRV   10 1 5060 sip-anycast-1.voice.google.com.
[  1][  2][  3]   [ 4][5] [6]   [7]       [10]
                                   [8]
                                     [9]
; [...]
```


  1. Service `++`
      * must start with an underscore
      * should be the name of an IANA registered service
  1. **Protocol** (either TCP or UDP) `++`
  1. The zone this record applies to `--`
  1. Record TTL `--`
  1. Record Class `--`
  1. Record Type `--`
  1. The target host record’s priority score `++`
  1. The target host record’s weight scaler `++`
  1. The service TCP/UDP **port** `++` 
  1. The target host record that will resolve to the numeric address of a **service host**

  `--` Standard RR bit

  `++` SRV RR bit 


## typically, somewhat useful

Historically SRV records were for scenarios where a client program
carried a baked-in or static configuration that would allow it to connect to 
the correct host and port to take care of its business--clients would happily
query the domain’s SRV records and the ops folks could move things around
without as much hassle.

The most prolific examples can be found with the introduction
of the Windows 2000 Server OS line mostly through its adoption of
Kerberos for the core of its Active Directory Service.


### into its own

The introduction of non-brittle service discovery cluster projects finally made
SRV records truly useful.

  * OSS Service Discovery projects
    * [Consul](https://www.hashicorp.com/blog/consul.html)
    * [Etcd](https://coreos.com/etcd) `*`
    * [Zookeeper](https://zookeeper.apache.org) 

`*` requires additional non-core pieces


They operate as internal services at the local and federated cluster levels.

DNS endpoints should be considered protocol gateways between the data store’s API
and the resolver client.

Because of the resolver service’s ability to respond efficiently to changes 
in services de/registered with the data store, the viability and accuracy of
SRV records have become an adequate service endpoint disseminator for all 
but the most low-latency requirements.


