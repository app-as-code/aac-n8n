# Changelog

All notable changes to this deployment definition are documented in this file.
The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Changed

- Upstream image tracks n8n v2.28.2 as defined in `service.yml`.

## [0.0.1]

### Added

- Initial App-as-Code service definition for n8n with Redis and PostgreSQL 18 dependencies.
- Web interface and webhooks on port 5678 with Traefik routing and automatic DNS.
- README with configuration reference and project icon.
