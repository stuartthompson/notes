# DNSSEC

DNSSEC is a security technology used to secure a domain against DNS poisoning
attacks. A chain of trust is established between the domain registrar and the
host.

## Configuring DNSSec in AWS Route 53

This was a fairly easy process to follow. I started with the AWS documentation
here:
[Enabling DNSSEC signing and establishing a chain of trust](
https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-configuring-dnssec-enable-signing.html?icmpid=docs_console_unmapped)

### Enabling DNSSEC Signing

Per the instructions, I enabled DNSSEC signing from the Route53 dashboard.

### Key-signing Keys (KSKs)

These keys are used to sign the keys that will secure the chain of trust
between the registrar and the host.

I created a customer-managed key via the Route 53 dashboard.

#### KSK Pricing

It was noted that there can be a cost associated with using KSKs in Route53.
Information on pricing here:
[AWS Key Management Service Pricing](https://aws.amazon.com/kms/pricing/)

It looks as though the service is free for less than 20,000 requests per month.

### Creating a DS Record on the Registrar (Google Domains)

The following instructions describe how to create a DS record for DNSSEC on
Google Domains. The steps may be different on another registrar's dashboard.

Google Domains appears to register DS records using digest values.

From the Google Domains dashboard, select the domain to work with:

[image needed]

Under "Next steps", click on the link named "Add a CNAME record to your DNS
configuration".

In the section named "DNSSEC", add a new key using the values that are shown
on the Route 53 dashboard.

#### Key Tag

The key tag is a number between 0 and 65535 that is shown in the left column
of the Route53 dashboard.

#### Algorithm

The algorithm is listed at the top of the second column on the Route53
dashboard.

#### Digest Type

The digest type is listed in the second column.

#### Digest

The digest is listed in the second column.
