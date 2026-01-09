# NS8 Vaultwarden Module

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Docker Image](https://img.shields.io/badge/docker-ready-blue.svg)](https://hub.docker.com)

A [NethServer 8](https://nethserver.github.io/ns8-core/) module that provides [Vaultwarden](https://github.com/dani-garcia/vaultwarden) - an alternative implementation of the Bitwarden server API written in Rust. This is perfect for self-hosted password management where running the official resource-heavy service might not be ideal.

## ✨ Features

- **Complete Bitwarden API compatibility** - Works with all Bitwarden clients
- **Lightweight and fast** - Written in Rust, much more efficient than the official server
- **Self-hosted security** - Keep your passwords under your control
- **Modern web interface** - Clean, responsive UI built with Vue.js and Carbon Design System
- **Multi-language support** - Available in multiple languages via Weblate
- **Easy integration** - Seamless integration with NethServer 8 ecosystem
- **Automated testing** - Comprehensive test suite with Robot Framework
- **Containerized deployment** - Easy to deploy and manage

## 📋 Prerequisites

Before installing this module, ensure you have the following dependencies:

- **Python and pip** - Required for package management:
  ```bash
  # Install pip if not already present
  python3 -m ensurepip --upgrade
  ```
- **argon2-cffi** - Required for password hashing:
  ```bash
  # Install argon2-cffi package
  pip install argon2-cffi
  ```

## 🚀 Quick Start

### Installation

1. **Add the module to your NethServer 8 cluster:**

   ```bash
   add-module ghcr.io/geniusdynamics/vaultwarden:latest 1
   ```

2. **Configure the module:**

   ```bash
   api-cli run module/vaultwarden1/configure-module --data '{
     "host": "vaultwarden.yourdomain.com",
     "lets_encrypt": true,
     "http2https": true
   }'
   ```

3. **Access your Vaultwarden instance** at `https://vaultwarden.yourdomain.com`

### Basic Configuration

The module supports extensive configuration options:

```bash
api-cli run module/vaultwarden1/configure-module --data '{
  "host": "vaultwarden.yourdomain.com",
  "lets_encrypt": true,
  "http2https": true,
  "WEB_VAULT_ENABLED": true,
  "SIGNUPS_ALLOWED": false,
  "SIGNUPS_VERIFY": true,
  "SIGNUPS_DOMAINS_WHITELIST": "yourdomain.com",
  "ADMIN_TOKEN": "your-secure-admin-token",
  "SMTP_HOST": "smtp.yourdomain.com",
  "SMTP_FROM": "vaultwarden@yourdomain.com"
}'
```

## 📋 Configuration Parameters

### Required Parameters

- `host` - Domain name for the Vaultwarden instance
- `lets_encrypt` - Enable Let's Encrypt SSL certificate
- `http2https` - Redirect HTTP to HTTPS

### Optional Parameters

- `WEB_VAULT_ENABLED` - Enable web vault interface (default: true)
- `SIGNUPS_ALLOWED` - Allow new user registrations (default: false)
- `SIGNUPS_VERIFY` - Require email verification for new accounts
- `SIGNUPS_DOMAINS_WHITELIST` - Comma-separated list of allowed domains
- `ADMIN_TOKEN` - Admin token for server management
- `SMTP_HOST` - SMTP server hostname
- `SMTP_FROM` - From email address for notifications
- `LOGIN_RATELIMIT_MAX_BURST` - Login rate limit burst (default: 10)
- `LOGIN_RATELIMIT_SECONDS` - Login rate limit window (default: 60)
- `EMERGENCY_ACCESS_ALLOWED` - Enable emergency access feature

For a complete list of configuration options, see the [Vaultwarden Wiki](https://github.com/dani-garcia/vaultwarden/wiki).

## 🔧 Management Commands

### Update Module

```bash
api-cli run update-module --data '{"module_url":"ghcr.io/geniusdynamics/vaultwarden:latest","instances":["vaultwarden1"],"force":true}'
```

### Test Installation

```bash
curl http://127.0.0.1/vaultwarden/
```

### Remove Module

```bash
remove-module --no-preserve vaultwarden1
```

## 🧪 Testing

Run the automated test suite:

```bash
./test-module.sh <NODE_ADDR> ghcr.io/geniusdynamics/vaultwarden:latest
```

Tests are implemented using [Robot Framework](https://robotframework.org/) and cover:

- Module installation and configuration
- Service availability and functionality
- Basic API endpoint validation

## 🌐 Internationalization

The UI supports multiple languages and is translated via [Weblate](https://hosted.weblate.org/projects/ns8/):

- English (en)
- Spanish (es)
- Italian (it)
- German (de)
- Basque (eu)

To contribute translations:

1. Visit the [NS8 Weblate project](https://hosted.weblate.org/projects/ns8/)
2. Find the Vaultwarden component
3. Submit your translations

## 🏗️ Architecture

### File Structure

```
├── imageroot/           # Container image root
│   ├── actions/         # Module actions (configure, create, destroy)
│   ├── bin/            # Utility scripts
│   ├── etc/            # Configuration files
│   ├── events/         # Event handlers
│   └── systemd/        # Systemd service definitions
├── ui/                 # Vue.js frontend application
│   ├── public/         # Static assets and metadata
│   ├── src/            # Vue.js source code
│   └── dist/           # Built frontend assets
├── tests/              # Robot Framework tests
└── build-images.sh     # Build script
```

## 🔒 Security Features

- **Rate limiting** - Built-in protection against brute force attacks
- **Admin token authentication** - Secure admin access
- **Domain whitelisting** - Control who can register
- **Email verification** - Prevent spam registrations
- **Emergency access** - Account recovery mechanisms
- **Encrypted data storage** - All sensitive data is encrypted at rest

## 🚀 Building from Source

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

### Areas for Contribution

- [ ] LDAP/AD integration
- [ ] SSO implementation
- [ ] Additional language translations
- [ ] Performance optimizations
- [ ] Security enhancements
- [ ] Documentation improvements

## 📝 Roadmap

### Planned Features

- **LDAP Integration** - Active Directory and LDAP authentication
- **SSO Support** - Single Sign-On capabilities
- **Advanced Backup** - Automated backup and restore functionality
- **Metrics & Monitoring** - Integration with monitoring systems
- **Mobile App Support** - Enhanced mobile client compatibility

### Future Enhancements

- Kubernetes deployment support
- High availability configuration
- Advanced audit logging
- API rate limiting customization
- Custom branding options

## 🐛 Bug Reports & Support

- **Bug Reports**: [GitHub Issues](https://github.com/geniusdynamics/ns8-vaultwarden/issues)
- **Documentation**: [Vaultwarden Wiki](https://github.com/dani-garcia/vaultwarden/wiki)
- **Community**: [NethServer Forum](https://community.nethserver.org/)

## 📄 License

This project is licensed under the GNU General Public License v3.0 - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Vaultwarden](https://github.com/dani-garcia/vaultwarden) - The core password management server
- [NethServer](https://nethserver.org/) - The hosting platform
- [Bitwarden](https://bitwarden.com/) - The original API specification
- [Carbon Design System](https://carbondesignsystem.com/) - UI design system
- [Weblate](https://weblate.org/) - Translation management

## 📞 Contact

- **Project Maintainers**: Martin Bhuong, Kemboi Elvis
- **Email**: martin@genius.ke, kemboielvis@genius.ke
- **GitHub**: [@geniusdynamics](https://github.com/geniusdynamics)

---

_Built with ❤️ for the NethServer community_
