#!/bin/bash
# subnet_ping_scan.sh
# Usage: ./subnet_ping_scan.sh <gateway_ip>
 
if [ -z "$1" ]; then
    echo "Usage: $0 <gateway_ip>"
    exit 1
fi
 
GATEWAY=$1
USERNAME=$(whoami)
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
OUTFILE="alive_hosts_${USERNAME}_${TIMESTAMP}.txt"
 
SUBNET=$(echo $GATEWAY | cut -d'.' -f1-3)
 
echo "Scanning subnet: ${SUBNET}.0/24"
echo "Output file    : $OUTFILE"
echo "------------------------------------------"
 
for i in $(seq 1 254); do
    IP="${SUBNET}.${i}"
    if ping -c 1 -W 1 "$IP" &> /dev/null; then
        echo "[+] $IP is alive"
        echo "$IP" >> "$OUTFILE"
    fi
done
 
echo "------------------------------------------"
echo "Scan complete. Active hosts saved to: $OUTFILE"
