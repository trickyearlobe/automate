+++
title = "Chef Automate Certificate Rotation Self Sign"

draft = false

gh_repo = "automate"

[menu]
  [menu.automate]
    title = "Chef Automate Certificate Rotation Self Sign"
    parent = "automate/install"
    identifier = "automate/install/ha_cert_selfsign.md Chef Automate Certificate Rotation Self Sign"
    weight = 250
+++

## Certificate Rotation

Certificate rotation means the replacement of existing certificates with new ones when any certificate expires or based on your organization policy. A new CA authority is substituted for the old requiring a replacement of root certificate for the cluster. 

The certificate rotation is also required when key for a node, client, or CA is compromised. Then, you need to modify the contents of a certificate, for example, to add another DNS name or the IP address of a load balancer through which a node can be reached.  In this case, you  would need to rotate only the node certificates.

## Rotating the Certificates

You can generate the required certificates or you can use the existing certificates of your organization. Follow these steps to rotate your certificates:

1. Navigate to your workspace folder. For example, `cd /hab/a2_deploy_workspace`.
2. Type the command, `./scripts/credentials set ssl --help `.

3. Type the command, `./scripts/credentials set ssl` with the appropriate options to generate list of skeleton files. ??

4. Copy your *x.509 SSL certs* into the appropriate files in `certs/` folder.

    - Place your root certificate into `ca_root.pem file`.

    - Place your intermediate CA into the `pem` file.

5. If your organization issues certificate from an intermediate CA, then place the respective certificate after the server certificate as per order listed. For example, in `certs/pg_ssl_public.pem`, paste it as them as listed:

   - Server Certificate
   - Intermediate CA Certificate 1
   - Intermediate CA Certificate n

6. Type the command, `./scripts/credentials set ssl` (with the appropriate options) and press **Enter**. This command deploys the nodes. You can check the options using --help command.

Have to cover the below ones?

- For rotating the postgresql certificates, ./scripts/credentials set postgresql --auto

- For rotating the elasticsearch certificates, ./scripts/credentials set elasticsearch --auto

- And to rotate all certificates in one command, ./scripts/credentials set ssl --rotate-all

DNS
