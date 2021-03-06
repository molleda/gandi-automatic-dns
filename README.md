gad
===

This script is intended to be used as a cron job to maintain the accuracy of multiple A or AAAA records in a Gandi.net zonefile. External IP address discovery is done via a network interface or [OpenDNS] [1]. The result is compared to the value in the active version of the zonefile of each record in RECORD-NAMES. Using the rpc() function to update different types of DNS records or call other methods of Gandi's XML-RPC API should be fairly trivial.

Prerequisites
=============

Your domain needs to be using a zone that you are allowed to edit. The default Gandi zone does not allow editing, so you must create a copy. There are instructions on Gandi's wiki to [create an editable zone] [2]. You only need to perform the first two steps. There is a FAQ regarding this [here] [3].

Requirements
============

  * Bourne shell
  * OpenSSL or [LibreSSL] [4]

If you're using a nutty OS that doesn't include the ifconfig or dig commands:
  * bind-tools (if you don't use the -i flag)
  * net-tools (if you do use the -i flag)

gad will never support iproute2.

Command line usage
==================

```
$ gad [-6] [-e] [-f] [-t] [-v] [-i EXT_IF] -a APIKEY -d EXAMPLE.COM -r "RECORD-NAMES"

-6: Update AAAA record(s) instead of A record(s)
-e: Print debugging information
-f: Force the creation of a new zonefile regardless of IP address discrepancy
-t: If a new version of the zonefile is created, do not activate it
-v: Print information to stdout even if a new zonefile isn't needed
-i: Use ifconfig instead of OpenDNS to determine external IP address

EXT_IF: The name of your external network interface
APIKEY: Your API key provided by Gandi
EXAMPLE.COM: The domain name whose active zonefile will be updated
RECORD-NAMES: A space-separated list of the name(s) of the A or AAAA record(s) to update or create
```

Request an API key from Gandi [here] [5].

rpc() syntax
============

```
rpc "methodName" "datatype" "value" "struct" "name" "datatype" "value"
```

This function can accept an arbitrary number of datatype/value pairs and structs and their member name/datatype/value tuples. structs _must_ be last! Valid method names can be found in the [Gandi API documentation] [6]. Note that the APIKEY value from the command line is automatically included as the first parameter.

  [1]: http://www.opendns.com
  [2]: http://wiki.gandi.net/en/dns/zone/edit
  [3]: http://wiki.gandi.net/en/dns/faq#cannot_change_zone_file
  [4]: http://www.libressl.org/
  [5]: https://www.gandi.net/admin/apixml/
  [6]: http://doc.rpc.gandi.net/index.html
