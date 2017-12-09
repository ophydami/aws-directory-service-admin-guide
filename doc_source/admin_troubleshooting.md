# Troubleshooting AWS Directory Service Administration Issues<a name="admin_troubleshooting"></a>

The following can help you troubleshoot some common issues you might encounter when creating or using your directory\.

## Simple AD<a name="troubleshoot_simple"></a>

Here are some common problems with Simple AD\.


+ [I am not able to update the DNS name or IP address of an instance joined to my domain \(DNS dynamic update\)](#dns_dynamic_updates)
+ [I cannot log onto SQL Server using a SQL Server account](#sql_login_fail)
+ [My directory is stuck in the "Requested" state](#stuck_in_requested1)
+ [I receive an "AZ Constrained" error when I create a directory](#contrained_az1)
+ [Some of my users cannot authenticate with my directory](#kerberos_preauth1)

### I am not able to update the DNS name or IP address of an instance joined to my domain \(DNS dynamic update\)<a name="dns_dynamic_updates"></a>

DNS dynamic updates are not supported in Simple AD domains\. You can instead make the changes directly by connecting to your directory using DNS Manager on an instance that is joined to your domain\.

### I cannot log onto SQL Server using a SQL Server account<a name="sql_login_fail"></a>

You might receive an error if you attempt to use SQL Server Management Studio \(SSMS\) with a SQL Server account to log into SQL Server running on a Windows 2012 R2 EC2 instance or in Amazon RDS\. The issue occurs when SSMS is run as a domain user and can result in the error "Login failed for user," even when valid credentials are provided\. This is a known issue and AWS is actively working to resolve it\.

To work around the issue, you can log into SQL Server with Windows Authentication instead of SQL Authentication\. Or launch SSMS as a local user instead of a Simple AD domain user\. 

### My directory is stuck in the "Requested" state<a name="stuck_in_requested1"></a>

If you have a directory that has been in the "Requested" state for more than five minutes, try deleting the directory and recreating it\. If this problem persists, contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.

### I receive an "AZ Constrained" error when I create a directory<a name="contrained_az1"></a>

Some AWS accounts created before 2012 might have access to Availability Zones in the US East \(N\. Virginia\), US West \(N\. California\), or Asia Pacific \(Tokyo\) region that do not support AWS Directory Service directories\. If you receive an error such as this when creating a directory, choose a subnet in a different Availability Zone and try to create the directory again\.

### Some of my users cannot authenticate with my directory<a name="kerberos_preauth1"></a>

Your user accounts must have Kerberos preauthentication enabled\. This is the default setting for new user accounts, and it should not be modified\. For more information about this setting, go to [Preauthentication](http://technet.microsoft.com/en-us/library/cc961961.aspx) on Microsoft TechNet\.

## AD Connector<a name="troubleshoot_connector"></a>

Here are some common problems with AD Connector\.

### I receive a "DNS unavailable" error when I try to connect to my on\-premises directory<a name="dns_unavailable"></a>

You receive an error message similar to the following when connecting to your on\-premises directory:

```
DNS unavailable (TCP port 53) for IP: <DNS IP address>
```

AD Connector must be able to communicate with your on\-premises DNS servers via TCP and UDP over port 53\. Verify that your security groups and on\-premises firewalls allow TCP and UDP communication over this port\. For more information, see [AD Connector Prerequisites](prereq_connector.md)\.

### I receive a "Connectivity issues detected" error when I try to connect to my on\-premises directory<a name="connectivity_issues_detected"></a>

You receive an error message similar to the following when connecting to your on\-premises directory:

```
Connectivity issues detected: LDAP unavailable (TCP port 389) for IP: <IP address>
Kerberos/authentication unavailable (TCP port 88) for IP: <IP address>
Please ensure that the listed ports are available and retry the operation.
```

AD Connector must be able to communicate with your on\-premises domain controllers via TCP and UDP over the following ports\. Verify that your security groups and on\-premises firewalls allow TCP and UDP communication over these ports\. For more information, see [AD Connector Prerequisites](prereq_connector.md)\.

+ 88 \(Kerberos\)

+ 389 \(LDAP\)

### I receive an "SRV record" error when I try to connect to my on\-premises directory<a name="srv_record_not_found"></a>

You receive an error message similar to one or more of the following when connecting to your on\-premises directory:

```
SRV record for LDAP does not exist for IP: <DNS IP address>

SRV record for Kerberos does not exist for IP: <DNS IP address>
```

AD Connector needs to obtain the `_ldap._tcp.<DnsDomainName>` and `_kerberos._tcp.<DnsDomainName>` SRV records when connecting to your directory\. You will get this error if the service cannot obtain these records from the DNS servers that you specified when connecting to your directory\. For more information about these SRV records, see [SRV record requirements](prereq_connector.md#srv_records)\.

### My directory is stuck in the "Requested" state<a name="stuck_in_requested2"></a>

If you have a directory that has been in the "Requested" state for more than five minutes, try deleting the directory and recreating it\. If this problem persists, contact the [AWS Support Center](https://console.aws.amazon.com/support/home#/)\.

### I receive an "AZ Constrained" error when I create a directory<a name="contrained_az2"></a>

Some AWS accounts created before 2012 might have access to Availability Zones in the US East \(N\. Virginia\), US West \(N\. California\), or Asia Pacific \(Tokyo\) region that do not support AWS Directory Service directories\. If you receive an error such as this when creating a directory, choose a subnet in a different Availability Zone and try to create the directory again\.

### Some of my users cannot authenticate with my directory<a name="kerberos_preauth2"></a>

Your user accounts must have Kerberos preauthentication enabled\. This is the default setting for new user accounts, but it should not be modified\. For more information about this setting, go to [Preauthentication](http://technet.microsoft.com/en-us/library/cc961961.aspx) on Microsoft TechNet\.

### I receive an "Invalid Credentials" error when the service account used by AD Connector attempts to authenticate<a name="invalid_creds"></a>

This can occur if the hard drive on your domain controller runs out of space\. Ensure that your domain controller's hard drives are not full\.