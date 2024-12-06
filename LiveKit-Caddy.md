# LiveKit Caddy Configuration
```
{
    auto_https disable_redirects
}

# Define the machine IP and port number where the LiveKit server is running
10.131.29.108:7888 {
    # The following rule will proxy requests to LiveKit server on localhost:7880
    reverse_proxy localhost:7880

    # reverse_proxy localhost:7880 {
    #    # Set the HTTP transport to use HTTP/1.1 only (h1)
    #    transport http {
    #        versions h1
    #    }
    # }

    # Use Caddy's internal certificate authority to automatically generate an HTTPS certificate
    tls internal
}
```

# Start Caddy with the specified configuration file
```
# Be sure to replace '/path/to/Caddyfile' with the actual path to your Caddyfile
sudo caddy run --config /path/to/Caddyfile
```
