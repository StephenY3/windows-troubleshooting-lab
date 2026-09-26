# Troubleshooting: DNS Resolution Failure

## Scenario

A Windows user reports that internet access appears to be working, but websites and domain names are not resolving.

The objective was to determine whether the issue was related to general network connectivity or DNS resolution, identify the cause, restore the correct configuration and verify the fix.

## Initial Baseline

The workstation initially had a functioning network connection.

The baseline confirmed:

- IPv4 connectivity was configured
- A default gateway was available
- External connectivity to `8.8.8.8` was working
- DNS resolution was initially functioning

This established that the workstation had network connectivity before the fault was introduced.

## Fault Introduced

To simulate a DNS-related support incident, the workstation's DNS configuration was changed from automatic DNS assignment to the test address:

`192.0.2.1`

This address was used to simulate an unavailable DNS server.

## Investigation

### 1. Test internet connectivity

The command `ping 8.8.8.8` was used to test connectivity to an external IP address.

The test returned successful replies with:

- 4 packets sent
- 4 packets received
- 0% packet loss

This confirmed that general internet connectivity was still working.

### 2. Test DNS resolution

The command `nslookup google.com` was then used.

The DNS request timed out and the configured DNS server was shown as:

`192.0.2.1`

This demonstrated that the workstation could reach the internet by IP address but could not successfully resolve domain names using the configured DNS server.

![DNS resolution failure](../dns-failure-project2.png)

## Diagnosis

The issue was isolated to DNS configuration.

The workstation had working external network connectivity, but DNS requests were being sent to an unavailable DNS server.

The troubleshooting process therefore identified:

**Workstation → Internet connectivity = Working**

**Workstation → DNS server → Domain resolution = Failing**

## Resolution

The DNS server assignment was changed back to:

**Automatic (DHCP)**

The DNS resolver cache was then cleared using:

`ipconfig /flushdns`

Windows confirmed that the DNS Resolver Cache was successfully flushed.

## Verification

### Internet connectivity

`ping 8.8.8.8`

Successful replies confirmed that external connectivity remained available.

### DNS resolution

`nslookup google.com`

The DNS server responded successfully and returned multiple IP addresses for `google.com`.

![DNS resolution restored](../dns-restored-project2.png)

This confirmed that DNS resolution had been restored.

## Key Learning

This investigation demonstrated the importance of separating network connectivity problems from DNS problems.

A successful ping to an external IP address does not necessarily mean that domain-name resolution is working.

Using `ping` and `nslookup` together helped isolate the fault:

**Test connectivity → Test DNS → Identify configuration issue → Restore DNS → Verify resolution**

This structured approach can be applied to first-line IT support incidents involving users who report that websites or applications cannot be reached.
