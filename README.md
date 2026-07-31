# Elma Shop

Elma Shop is a Django-powered e-commerce storefront for showcasing and selling curated fashion, beauty, jewelry, and home décor products. The application provides category-based browsing, product search, session-backed cart management, checkout with shipping locations, PDF order summaries, and WhatsApp handoff for customer follow-up.

## Project overview

This repository contains a production-oriented Django 5 application configured for deployment with Gunicorn, WhiteNoise static asset serving, PostgreSQL-compatible database URLs, and Railway hosting. The main project package is `ElmaShop`, while the `Shop` app contains the storefront domain models, views, forms, utilities, sitemaps, middleware, and template tags.

Core storefront pages include:

- Home page with featured categories and hero images.
- Dedicated category pages for dresses, beauty products, jewelry, and home décor.
- Search results for product discovery.
- Cart, checkout, order confirmation, and order-error pages.
- SEO support through `robots.txt` and Django sitemap endpoints.

## Features

- **Product catalog management** with categories, subcategories, featured products, ordered products, and category hero images.
- **Session-based cart** that supports adding, removing, and updating product quantities without requiring customer accounts.
- **Checkout workflow** that captures customer name, shipping location or custom address, subtotal, shipping cost, and total amount.
- **Purchase order records** with generated order numbers and persisted order line items.
- **PDF order generation** using ReportLab for downloadable/shareable order summaries.
- **WhatsApp integration** for forwarding order details to the shop owner or support team.
- **Product search** across product names and short descriptions with pagination.
- **Admin configuration** for products, categories, shipping locations, carts, and orders.
- **Static asset pipeline** using project-level static files, collected static output, and WhiteNoise compressed manifest storage.
- **SEO endpoints** including sitemap generation and a `robots.txt` template.
- **Production deployment support** via Railway configuration and a Procfile.

## Technologies used

- **Python 3**
- **Django 5.1**
- **PostgreSQL** via `dj-database-url` and `psycopg2`
- **Gunicorn** for WSGI serving
- **WhiteNoise** for static file serving
- **python-dotenv** for local environment variable loading
- **Pillow** for image processing support
- **ReportLab** for PDF generation
- **Requests** for HTTP requests
- **HTML, CSS, and JavaScript** for the storefront templates and interactive cart/checkout behavior
- **Railway** deployment configuration

## Installation instructions

### Prerequisites

- Python 3.12 or a compatible Python 3 version
- PostgreSQL database, or another database exposed through `DATABASE_URL`
- Git

### 1. Clone the repository

```bash
git clone <repository-url>
cd Elma-Shop
```

### 2. Create and activate a virtual environment

```bash
python -m venv .venv
source .venv/bin/activate
```

On Windows PowerShell:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

### 3. Install dependencies

```bash
pip install -r requirements.txt
```

> Note: If `requirements.txt` was generated on Windows and your installer reports encoding issues, regenerate it in UTF-8 with `pip freeze > requirements.txt` from a clean environment.

### 4. Configure environment variables

Create a `.env` file in the project root:

```env
DJANGO_SECRET_KEY=replace-with-a-secure-secret-key
DEBUG=True
DATABASE_URL=postgres://USER:PASSWORD@HOST:PORT/DB_NAME
WHATSAPP_NUMBER=254700000000
WHATSAPP_MESSAGE=Hello! I'm interested in ordering from your shop. Can you please provide me with information about available items?
```

Important settings:

- `DJANGO_SECRET_KEY`: Required by Django.
- `DEBUG`: Controls local development behavior.
- `DATABASE_URL`: Database connection string parsed by `dj-database-url`.
- `WHATSAPP_NUMBER`: Destination phone number used on order confirmation.
- `WHATSAPP_MESSAGE`: Default WhatsApp message used by site components.

### 5. Apply database migrations

```bash
python manage.py migrate
```

### 6. Load initial category data

```bash
python manage.py loaddata Shop/fixtures/initial_categories.json
```

### 7. Create an admin user

```bash
python manage.py createsuperuser
```

### 8. Collect static files for production-like serving

```bash
python manage.py collectstatic
```

### 9. Run the development server

```bash
python manage.py runserver
```

Then open `http://127.0.0.1:8000/` in your browser.

## Usage examples

### Browse category pages

- `GET /` — storefront home page
- `GET /dress/` — dresses category
- `GET /beauty/` — beauty products category
- `GET /jewelry/` — jewelry category
- `GET /homedecor/` — home décor category
- `GET /products/<category-slug>/` — category page by slug

### Search for products

```text
GET /search/?q=necklace
```

Search results are paginated and match product names or short descriptions.

### Cart operations

The cart endpoints are designed for POST requests, typically from the included JavaScript files:

```text
POST /add-to-cart/<product_id>/
POST /remove-from-cart/<product_id>/
POST /update-cart/<product_id>/
```

### Checkout and order confirmation

```text
GET  /checkout/
POST /checkout/
GET  /thank-you/
```

A successful checkout creates a pending purchase order, stores order data in the session, generates a PDF summary on the thank-you page, and prepares a WhatsApp message containing the order PDF URL.

### Admin workflow

1. Visit `/admin/`.
2. Add or update categories, subcategories, products, hero images, and shipping locations.
3. Mark products as featured or hero images as needed.
4. Review generated carts and purchase orders.

## Folder structure

```text
.
├── ElmaShop/              # Django project settings, root URLs, WSGI/ASGI entry points
├── Shop/                  # Storefront Django app: models, views, forms, utilities, sitemaps
│   ├── fixtures/          # Initial category fixture data
│   ├── migrations/        # Database schema migrations
│   └── templatetags/      # Custom template tags and filters
├── static/                # Source CSS, JavaScript, images, and admin static assets
├── staticfiles/           # Collected static output for deployment
├── templates/             # Django templates for pages, layout, cart, checkout, and errors
├── manage.py              # Django management command entry point
├── Procfile               # Gunicorn process definition
├── railway.json           # Railway deployment configuration
├── requirements.txt       # Python dependencies
└── test_postgres.py       # Database connectivity helper script
```

## Future improvements

- Add automated unit and integration tests for models, views, cart behavior, checkout, and PDF generation.
- Move hard-coded production hosts and base URLs into environment variables for easier multi-environment deployment.
- Add payment provider integration for online checkout.
- Add customer accounts, order history, and order-status notifications.
- Improve inventory tracking and stock availability validation.
- Add CI checks for formatting, linting, migrations, and Django system checks.
- Normalize dependency management and ensure `requirements.txt` is stored in UTF-8.
- Add containerized local development with Docker Compose for Django and PostgreSQL.

## License

No license file is currently included in this repository. Add a `LICENSE` file before distributing or open-sourcing the project so users know what permissions and restrictions apply.
