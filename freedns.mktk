##############   Script FreeDNS.afraid.org   ##################
##############   PARSER EDITION   ##################
##############   CREATED LESHIY_ODESSA   ##################

# Este scrip lo obtuve aqui:
# https://forum.mikrotik.com/viewtopic.php?t=83744

   
# Specify the "Direct URL", which is https://freedns.afraid.org/dynamic/
# If RouterOS version 5.xx, then remove from the URL encryption - "https" change this to "http". Also see below.
# In front of the sign "?" put a backslash "\".
:global "direct-url" "https://freedns.afraid.org/dynamic/update.php\?########## CAMBIAR AQUI ##############"

# Specify the URL API "ASCII"
# Log in under your account and open the page https://freedns.afraid.org/api/
# Then copy the URL of your site - Available API Interfaces : ASCII (!!! NOT XML !!!)
# ATTENTION!!!! Before the question mark, put a backslash "\".
# If RouterOS version 5.xx, then remove from the URL encryption - "https" change this to "http".
:global "api-url" "https://freedns.afraid.org/api/\?action=getdyndns&v=2&sha=########### CAMBIAR AQUI ##############"
    
# Specify your domain or subdomain.
:global "dns-domain" "######### CAMBIAR AQUI name.com #############"

# Define variables for the external (WAN) interface
# Case sensitive.
:global "out-interface" "ether2-wan2"
       
# !!!!!!!!!!!!!!!!! Nothing more do not need to edit!!!!!!!!!!!!!!!!!
       
# Check whether the file with the IP domain - freedns.txt
:if ([:len [/file find name=freedns.txt]] > 0) do={
} else={
/tool fetch url=$"api-url" dst-path="/freedns.txt"
}
# Find out the IP address of the domain using the API and parsing.
# Split the file
:local "result" [/file get freedns.txt contents]
:local "startloc" ([:find $"result" $"dns-domain"] + ([:len $"dns-domain"] + 1))
:local "endloc" ([:find $"result" $"direct-url" -1] -1)
:global "dns-domain-ip" [:pick $"result" $"startloc" $"endloc"]
       
# Find the current IP address on the external interface
:global "current-ip" [/ip address get [find interface=$"out-interface"] address]
    
# Obtained from IP addresses to be excluded subnet mask
:set "current-ip" [:pick $"current-ip" 0 ([:len $"current-ip"]-3) ]

