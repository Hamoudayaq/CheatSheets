<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Study Sheet Template</title>

  <style>
    body {
      font-family: Inter, system-ui, -apple-system, 'Segoe UI', Roboto, 'Helvetica Neue', Arial;
      line-height: 1.5;
      color: #0f172a;
      background: #f8fafc;
      padding: 24px;
    }

    .container {
      max-width: 900px;
      margin: 0 auto;
      background: #ffffff;
      padding: 28px;
      border-radius: 12px;
      box-shadow: 0 6px 20px rgba(2, 6, 23, 0.08);
    }

    h1 {
      font-size: 1.6rem;
      margin-bottom: 6px;
    }

    h2 {
      font-size: 1.1rem;
      margin-top: 18px;
    }

    h3 {
      font-size: 1rem;
      margin-top: 14px;
    }

    p {
      margin: 8px 0;
    }

    ul {
      margin: 8px 0 8px 20px;
    }

    pre {
      background: #0b1220;
      color: #e6eef8;
      padding: 12px;
      border-radius: 8px;
      overflow: auto;
    }

    code {
      font-family: SFMono-Regular, Menlo, Monaco, Consolas, 'Liberation Mono', monospace;
    }

    hr {
      margin: 24px 0;
      border: none;
      border-top: 1px solid #e5e7eb;
    }

    .badge {
      display: inline-block;
      background: #eef2ff;
      color: #3730a3;
      padding: 6px 10px;
      border-radius: 999px;
      font-weight: 600;
      font-size: 0.85rem;
    }

    .pill {
      background: #eef2ff;
      color: #3730a3;
      padding: 6px 10px;
      border-radius: 999px;
      font-weight: 600;
    }

    .key {
      background: #fffbeb;
      border-left: 4px solid #f59e0b;
      padding: 10px;
      border-radius: 6px;
    }

    .tip {
      background: #ecfeff;
      border-left: 4px solid #06b6d4;
      padding: 10px;
      border-radius: 6px;
    }

    .actions {
      display: flex;
      gap: 8px;
      flex-wrap: wrap;
      margin-top: 12px;
    }

    .btn {
      display: inline-block;
      margin: 8px 0;
      padding: 10px 18px;
      background-color: #007bff;
      color: #ffffff;
      text-decoration: none;
      border-radius: 6px;
      font-size: 16px;
      transition: background-color 0.3s;
    }

    .btn:hover {
      background-color: #0056b3;
    }

    footer {
      margin-top: 24px;
      font-size: 0.9rem;
      color: #475569;
      text-align: center;
    }
  </style>
</head>

<body>
  <div class="container">

    <!-- Navigation -->
    <div class="actions">
      <a href="#" class="btn">Home</a>
      <a href="#" class="btn">Back</a>
      <a href="./quay-dependencies.md" class="btn">Dependencies</a>
    </div>

    <!-- Page Title -->
    <h1>Installing and Configuring Red Hat Quay Enterprise</h1>
    <span class="badge">Prerequisites:
      <ul>
        <li>Security - Certificate CA in anchors</li>
        <li>Storage - Ensure storage dir have minimum 30Gib</li>
        <li>Network - Create a dedicated podman network using <strong> quay </strong> user.</li>
        <li>User settings - Create quay user and activate linger </li>
      </ul>
    
    </span>

    <!-- Intro Section -->
    <section>
      <h2>Preparing the Environment to Deploy Red Hat Quay</h2>
      <p>
        <ul>
          <li>Ensuring That a Certificate Authority Is Available</li>
          <li>Installing Container Tools</li>
          <li>Creating a Dedicated User</li>
          <li>Authenticating to the Red Hat Registry</li>
          <li>Creating a Private Podman Network for the Services</li>
          <li>Creating Storage Locations for Data and Configuration Files</li>

        </ul>
      </p>
    </section>

    <hr>

    <!-- Content Section -->
    <section>
      <h2>Ensuring That a Certificate Authority Is Available</h2>
      <p>Description or explanation.</p>

      <h3>Create a CA directory and generate a private key and a self-signed certificate.</h3>
     
        <pre><code>
        [user@host ~]$ mkdir ca
        [user@host ~]$ cd ca

        [user@host ca]$ openssl genrsa -out ca-key.pem -aes256 \
        -passout pass:yourpassword 8192

        [user@host ca]$ openssl req -new -x509 -key ca-key.pem \
        -passin pass:yourpassword -days 3650 -out ca-cert.pem \
        -subj "/C=XX/ST=State/L=City/O=Org/OU=Dept/CN=CA Name"

        [user@host ca]$ cd
        </code></pre>

    </section>
    <section>
          <h3>Ensure that all systems trust the certificate, by installing it in their /etc/pki/ca-trust directory.</h3>
          <pre><code>
          [user@host ~]$ sudo cp ./ca/ca-cert.pem \
          /etc/pki/ca-trust/source/anchors/lab-ca.pem

          [user@host ca]$ sudo update-ca-trust

          [user@host ca]$ trust list --filter=ca-anchors | grep -3 "CA Name"

          pkcs11:id=%37%6F%D0%6F%7A%09%B0%25%A3%FC%FA%8D%11%C1%9B%A2%11%13%D9%6D;type=cert
              type: certificate
              label: CA Name
              trust: anchor
              category: authority

          </code></pre>

    </section>

        <section>
          <h3>After the self-signed certificate is created and trusted, you must configure a CA. An example configuration file to put into the ca/openssl.cnf file looks as follows:</h3>

          <pre><code>
            [ ca ]
            default_ca      = CA_default

            [ CA_default ]
            dir             = /home/user/ca/ca-data
            serial          = $dir/serial
            database        = $dir/index.txt
            new_certs_dir   = $dir/newcerts

            certificate     = /home/user/ca/ca-cert.pem
            private_key     = /home/user/ca/ca-key.pem

            default_days    = 365
            default_crl_days= 30
            default_md      = sha256

            policy          = policy_any
            email_in_dn     = no

            name_opt        = ca_default
            cert_opt        = ca_default
            copy_extensions = copy

            [ policy_any ]
            countryName            = supplied
            stateOrProvinceName    = optional
            organizationName       = optional
            organizationalUnitName = optional
            commonName             = supplied
            emailAddress           = optional
          </code></pre>

    </section>

    <section>
          <h3>Create three important files for the CA: a new certificate storage directory, an index of issued certificates, and a serial number tracker.</h3>

          <pre><code>
            [user@host ~]$ cd ca
            [user@host ca]$ mkdir -p ca-data/newcerts
            [user@host ca]$ touch ca-data/index.txt
            [user@host ca]$ echo 0000 > ca-data/serial
          </code></pre>
    </section>

    <section>
        <h3> The directory structure should look as follows: </h3>
          <pre><code>
            [user@host ca]$ tree ./
            ./
            ├── ca-cert.pem
            ├── ca-key.pem
            ├── ca-data
            │   ├── index.txt
            │   ├── newcerts
            │   └── serial
            └── openssl.cnf
          </code></pre>

        <h3>Install the container-tools package. </h3> 
          <pre><code>
          [user@host ~]$ sudo dnf install container-tools
          ...output omitted...
          </code></pre>
    </section>

    <section>
       <h3> Creating a Dedicated User </h3>
        <pre><code>
        [user@host ~]$ sudo useradd quay
        [user@host ~]$ sudo passwd quay

        Enable lingering for the quay user.
        [user@host ~]$ sudo loginctl enable-linger quay
        </code></pre>
    </section>

    <section>
      <h3>Authenticating to the Red Hat Registry</h3>
        <pre><code>      
          [quay@host ~]$ podman login registry.redhat.io
          Username: my-redhat-username
          Password: my-redhat-password
          Login Succeeded!

          [quay@host ~]$ mkdir .docker
          [quay@host ~]$ cp /run/user/$(id -u)/containers/auth.json .docker/config.json
        </code></pre>        
    </section>

    <section>
      <h3>Creating a Private Podman Network for the Services</h3>    
        <pre><code>      
          [quay@host ~]$ podman network create quay
          quay

          [quay@host ~]$ podman network inspect quay
          [
              {
                    "name": "quay",
                    ...output omitted...
            <strong>"dns_enabled": true, </strong>
                    "ipam_options": {
                        "driver": "host-local"
                    },
                    "containers": {}
              }
          ]
        </code></pre>        
    </section>
    <section>
      <h3>Creating Storage Locations for Data and Configuration Files</h3>    
        <pre><code>  
          [user@host ~]$ df -h /storage/
          Filesystem      Size  Used Avail Use% Mounted on
          /dev/sdb1       120G   35G   86G  29% /storage

          [user@host ~]$ sudo mkdir /storage/quay
          [user@host ~]$ sudo mkdir /storage/postgresql
        </code></pre>        
    </section>





    <hr>

    <!-- Highlight Boxes -->
    <div class="key">
      <strong>Key Point:</strong> Important concept or warning.
    </div>

    <br>

    <div class="tip">
      <strong>Tip:</strong> Best practice or recommendation.
    </div>

    <!-- Footer -->
    <footer>
      © Study Sheet Template - Yassine
    </footer>

  </div>
</body>