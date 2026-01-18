# Deployment Guide for Namecheap Server

## Pre-Deployment Checklist

### 1. Files to Upload
Upload all project files to your Namecheap server EXCEPT:
- `core/vendor/` (will be installed on server)
- `core/.env` (will be created manually on server)
- `core/node_modules/` (not needed on server)
- `core/storage/` contents (except `.gitignore` files)

### 2. Create `.env` File on Server
Once uploaded, create `core/.env` with these settings:

```bash
APP_NAME=ovox
APP_ENV=production
APP_KEY=base64:BJf8oHdfMnzKfuyixlz/OkI/gjb4Lzf+S1xboonYNsE=
APP_DEBUG=false
APP_URL=https://your-actual-domain.com

DB_CONNECTION=mysql
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=lxianbym_ovox
DB_USERNAME=lxianbym_info
DB_PASSWORD=wt07M0=RIV7K
```

**Important:** Replace `https://your-actual-domain.com` with your real domain!

### 3. Server Setup Commands

If you have SSH access, run these commands:

```bash
cd public_html/core  # or wherever you uploaded the files

# Install PHP dependencies
composer install --no-dev --optimize-autoloader

# Set proper permissions
chmod -R 755 storage bootstrap/cache
chmod -R 777 storage/framework
chmod -R 777 storage/logs

# Run migrations
php artisan migrate --force

# Clear and cache config
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

### 4. Web Server Configuration

#### For Apache (.htaccess already included)
Make sure your document root points to the `public` directory or use the provided `index.php` in root.

#### For cPanel
1. Go to **File Manager**
2. Upload files to `public_html`
3. The root `index.php` should handle routing to `core/`

### 5. Post-Deployment

1. Test the application at your domain
2. Check error logs if issues occur: `core/storage/logs/laravel.log`
3. If you see "500 Internal Server Error":
   - Check file permissions
   - Verify `.env` file exists and has correct credentials
   - Check PHP version (needs 8.2+)

## Troubleshooting

### Migration Errors
If migrations fail because tables exist:
```bash
php artisan migrate:status
# Then manually mark migrations as run or use migrate:fresh (DELETES DATA!)
```

### Permission Errors
```bash
chmod -R 775 storage
chmod -R 775 bootstrap/cache
chown -R www-data:www-data storage bootstrap/cache
```

### Composer Not Available
If your shared hosting doesn't have composer:
1. Install dependencies locally
2. Upload the entire `vendor/` folder
3. This will make your upload much larger but will work

## Security Checklist

- [ ] `.env` file is NOT in git repository
- [ ] `APP_DEBUG=false` in production
- [ ] `APP_ENV=production` is set
- [ ] Strong database password is used
- [ ] File permissions are correct (not 777 except where needed)
- [ ] SSL certificate is installed (HTTPS)

## Database Setup

Your Namecheap database is already created with:
- Database: `lxianbym_ovox`
- User: `lxianbym_info`
- Password: `wt07M0=RIV7K`

Just run migrations to create tables:
```bash
php artisan migrate --force
```

## Support

If you encounter issues:
1. Check `storage/logs/laravel.log`
2. Enable debug mode temporarily: `APP_DEBUG=true`
3. Check PHP error logs in cPanel
