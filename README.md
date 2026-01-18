# OVOX

A Laravel-based application with React flow builder integration.

## Requirements

- PHP 8.3+
- MySQL 5.7+
- Composer
- Node.js & NPM

## Installation

1. Clone the repository:
```bash
git clone https://github.com/Ceaser16/ovoxo.git
cd ovoxo
```

2. Install PHP dependencies:
```bash
cd core
composer install
```

3. Install Node dependencies:
```bash
npm install
```

4. Configure environment:
```bash
cp .env.example .env
php artisan key:generate
```

5. Update `.env` with your database credentials:
```
DB_DATABASE=ovox
DB_USERNAME=your_username
DB_PASSWORD=your_password
```

6. Run migrations:
```bash
php artisan migrate
```

7. Build frontend assets:
```bash
npm run build
```

## Development

To run the development server:
```bash
npm run flow
```

## Production

Build production assets:
```bash
npm run build
```

## License

Proprietary - All rights reserved
