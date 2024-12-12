# LiveKit Caddy Configuration
```
{
    auto_https disable_redirects
}

# 10.131.29.108 is the address of the computer running Caddy
# Clients connect to this address and port (7888) to use Caddy
# Use 0.0.0.0 to allow connections on all addresses of this computer
10.131.29.108:7888 {
    # Forward requests to the LiveKit server running on localhost:7880
    reverse_proxy localhost:7880

    # reverse_proxy localhost:7880 {
    #    # Use HTTP/1.1 for connections to the LiveKit server
    #    transport http {
    #        versions h1
    #    }
    # }

    # Automatically create an HTTPS certificate using Caddy's internal tools
    tls internal
}
```

# Start Caddy with the specified configuration file
```
# Be sure to replace '/path/to/Caddyfile' with the actual path to your Caddyfile
sudo caddy run --config /path/to/Caddyfile
```
