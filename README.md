# Invoke-ReverseDnsLookup

A PowerShell function for performing reverse DNS lookups on IP address ranges.

## Features

- **Multiple IP range formats**: Single IPs, CIDR notation, or IP ranges
- **Custom DNS server**: Query specific DNS servers (useful for internal DNS enumeration)
- **Hosts file format**: Output results ready for /etc/hosts or Windows hosts file
- **Pipeline support**: Accept input from other PowerShell commands

## Installation

1. Download the script
2. Import it into your PowerShell session:
```powershell
. .\Invoke-ReverseDnsLookup.ps1
```

Or add it to your PowerShell profile for persistent availability.

## Usage

### Basic Examples

```powershell
# Single IP address
Invoke-ReverseDnsLookup 192.168.1.1

# CIDR range
Invoke-ReverseDnsLookup 192.168.1.0/24

# IP range (low-high)
Invoke-ReverseDnsLookup 10.10.10.1-10.10.10.50

# Multiple ranges (comma-separated)
Invoke-ReverseDnsLookup '192.168.1.1,10.0.0.0/28,172.16.0.1-172.16.0.10'
```

### Custom DNS Server

Query a specific DNS server instead of using system defaults:

```powershell
# Query internal DNS server
Invoke-ReverseDnsLookup 172.16.0.0/24 -DNSServer 172.16.0.1
```

### Hosts File Format

Output results in hosts file format for easy integration:

```powershell
# Display in hosts format
Invoke-ReverseDnsLookup 192.168.1.0/29 -HostsFormat

# Output:
# 192.168.1.1    router.local
# 192.168.1.2    nas.local
# 192.168.1.5    workstation.local

# Append to Windows hosts file (run as Administrator)
Invoke-ReverseDnsLookup 10.10.10.0/24 -HostsFormat | Out-File -Append C:\Windows\System32\drivers\etc\hosts

```

## Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `-IPRange` | String | Yes | IP address, CIDR range, or IP range (supports comma-separated values) |
| `-DNSServer` | String | No | IP address of DNS server to query (default: system DNS) |
| `-HostsFormat` | Switch | No | Output in hosts file format (IP<tab>hostname) |


## Output Format

### Default Output (PowerShell Objects)
```
IP              HostName
--              --------
192.168.1.1     router.local
192.168.1.2     nas.local
192.168.1.5     workstation.local
```

### Hosts Format Output
```
192.168.1.1     router.local
192.168.1.2     nas.local
192.168.1.5     workstation.local
```

## Notes

- Only IPs with valid PTR records will be returned
- Failed lookups are silently skipped (use `-Verbose` to see errors)
- CIDR ranges exclude network and broadcast addresses
- Requires appropriate network access and permissions
- Use responsibly and only on networks you're authorized to test

## Credits

- **Original Author**: Matthew Graeber (@mattifestation)
- **Project**: PowerSploit
- **License**: BSD 3-Clause
- **Modified**: Added DNSServer and HostsFormat parameters

## Links

- [PowerSploit Project](https://github.com/PowerShellMafia/PowerSploit)
- [Exploit Monday Blog](http://www.exploit-monday.com)
