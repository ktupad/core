# Ktupad Core

**A lightweight PHP PDO engine for JSON-driven CRUD operations**

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.XXXXXXX.svg)](https://doi.org/10.5281/zenodo.XXXXXXX)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![PHP](https://img.shields.io/badge/PHP-%3E%3D7.4-blue.svg)](https://php.net)

---

## Overview

Ktupad Core is a single-entry-point MVC micro-framework built on PHP PDO, designed for rapid backend API development with minimal dependencies. All CRUD operations are dispatched via JSON payloads, making it suitable for decoupled frontend architectures, low-resource server environments, and educational institutional systems.

Originally developed in 2019 and registered as intellectual property (EC00201952487, Direktorat Jenderal Kekayaan Intelektual, Republic of Indonesia), Ktupad Core represents the foundational layer of the Ktupad ecosystem — preceding and informing the architecture of BayamJS (2023) and DonatJS (2024).

---

## Features

- **Single Entry Point** — all requests routed through one dispatcher
- **JSON-driven CRUD** — `create`, `read`, `update`, `delete`, `table`, `cari` via JSON body or query string
- **PDO Abstraction** — supports MySQL; extensible to SQLite, PostgreSQL, MSSQL
- **Role-Based Access Control** — token-based authentication with per-menu CRUD permission matrix
- **File Upload Handler** — built-in image upload with validation
- **CSV Import** — bulk data import via semicolon-delimited CSV
- **Hierarchical Access** — `tableP()` for tree-structured user data scoping
- **Zero Dependency** — pure PHP, no Composer required

---

## Architecture

```
Request (HTTP/JSON)
        │
        ▼
  ktupad.php          ← Front Controller + Base Class
  (init → dispatch)
        │
        ├── database.php    ← PDO Connection Layer (koneksi)
        │
        └── model.php       ← Domain Model (mod extends ktupad)
```

**Inheritance chain:** `koneksi → ktupad → mod`

**Design patterns applied:**
- Front Controller (single entry dispatch)
- Active Record (model carries query logic)
- Command Pattern (dispatch via `mod` parameter)
- Configuration-as-State (`$conf` array as runtime context)

---

## Requirements

- PHP >= 7.4
- MySQL >= 5.7 / MariaDB >= 10.3
- PDO extension enabled

---

## Installation

```bash
git clone https://github.com/ktupad/core.git
cd core
```

Configure your database in `database.php`:

```php
public $database = array(
    'h' => 'localhost',
    'u' => 'your_user',
    'p' => 'your_password',
    'n' => 'your_database'
);
```

---

## Usage

### Read (GET)

```
GET /database.php?mod=table&tb=master_users&limit=10&offset=0
```

### Create (POST JSON)

```json
{
  "mod": "create",
  "tb": "master_users",
  "token": "your_token",
  "data": {
    "nama": "Ahmad",
    "email": "ahmad@example.com"
  }
}
```

### Update (POST JSON)

```json
{
  "mod": "update",
  "tb": "master_users",
  "id": "5",
  "token": "your_token",
  "data": {
    "nama": "Ahmad Dahlan"
  }
}
```

### Delete (POST JSON)

```json
{
  "mod": "delete",
  "tb": "master_users",
  "id": "5,6,7",
  "token": "your_token"
}
```

---

## Response Format

All responses return JSON:

```json
{
  "sql": "SELECT * FROM master_users ...",
  "info": "Berhasil Read",
  "fld": ["id", "nama", "email"],
  "data": [...],
  "akses": ["c", "r", "u", "d"]
}
```

---

## Extending the Model

Create `model.php` to define your domain context:

```php
<?php
class mod extends ktupad {
    public $conf2 = array(
        'tb' => 'your_table',
        'mn' => 'your_menu_name',
    );
}
```

---

## Security Notes

- Token-based authentication is required for all write operations when `isAkses = 1`
- Basic SQL injection prevention via string replacement on `or` patterns
- **Recommended:** Add prepared statement support before production deployment
- Credentials in `database.php` should be moved to environment variables in production

---

## Ecosystem

| Project | Year | Description | HKI |
|---------|------|-------------|-----|
| **Ktupad** | 2019 | MVC PHP backend engine | EC00201952487 |
| **BayamJS** | 2023 | MVC JavaScript framework | EC00202367008 |
| **DonatJS** | 2024 | JSON-driven MVC frontend | EC00202414144 |

---

## Citation

If you use Ktupad Core in your research, please cite:

```bibtex
@software{sismadi_ktupad_core_2019,
  author       = {Sismadi, Wawan},
  title        = {{Ktupad Core: A Lightweight PHP PDO Engine for JSON-Driven CRUD Operations}},
  year         = {2019},
  publisher    = {Zenodo},
  version      = {1.0.0},
  doi          = {10.5281/zenodo.XXXXXXX},
  url          = {https://github.com/ktupad/core},
  note         = {Registered intellectual property EC00201952487, DJKI Republic of Indonesia}
}
```

---

## Intellectual Property

- **Copyright:** Wawan Sismadi
- **HKI Number:** EC00201952487
- **Registered:** Direktorat Jenderal Kekayaan Intelektual (DJKI), Republic of Indonesia
- **Institution:** PT Sismadi Langit Solusi / Universitas IPWIJA

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Author

**Wawan Sismadi**  
NIDN: 0816087703 | SINTA ID: 6848496 | ORCID: [0009-0007-2685-5663](https://orcid.org/0009-0007-2685-5663)  
Lecturer, Universitas IPWIJA · Doctoral Candidate, Universitas Ahmad Dahlan  
Founder, PT Sismadi Langit Solusi
