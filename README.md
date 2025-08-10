# LinkShrink - URL Shortener

A **highly scalable URL shortener** built with Next.js with advanced features like QR codes, rate limiting, and user authentication - perfect for solo developers or startup MVPs.

## 🚀 Features

- **Lightning Fast**: Generate shortened URLs in milliseconds
- **User Authentication**: Secure sign-up/sign-in with personal dashboards
- **QR Code Generation**: Instant QR codes for easy sharing
- **Rate Limiting**: API protection with Redis-based rate limiting
- **Click Analytics**: Track clicks and monitor performance
- **Personal Dashboard**: Manage all your URLs in one place
- **Scalable Architecture**: Built on Next.js with serverless functions
- **Beautiful UI**: Production-ready interface with Tailwind CSS
- **Database Agnostic**: Works with PlanetScale, Supabase, or any MySQL database

## 🛠️ Tech Stack

| Component | Technology | Purpose |
|-----------|------------|---------|
| Framework | Next.js 14 | Full-stack React framework with App Router |
| Authentication | NextAuth.js | Secure user authentication and session management |
| Database | MySQL (PlanetScale/Supabase) | Serverless, scalable database |
| ORM | Drizzle ORM | Lightweight, type-safe database queries |
| Rate Limiting | Redis (Upstash) | API protection and abuse prevention |
| QR Codes | qrcode.react | QR code generation for shortened URLs |
| Short IDs | nanoid | Unique, URL-safe short codes |
| Styling | Tailwind CSS | Utility-first CSS framework |
| Icons | Lucide React | Beautiful, consistent icons |
| Hosting | Vercel | Serverless deployment platform |

## 📁 Project Structure

```
/
├── src/
│   ├── app/
│   │   ├── auth/signin/page.tsx     # Authentication page
│   │   ├── dashboard/page.tsx       # User dashboard
│   │   ├── api/shorten/route.ts    # API endpoint for URL shortening
│   │   ├── api/user-urls/route.ts   # API for user's URLs
│   │   ├── api/auth/[...nextauth]/route.ts # NextAuth.js API routes
│   │   ├── [slug]/page.tsx         # Dynamic redirect handler
│   │   ├── page.tsx                # Homepage with URL shortener form
│   │   └── layout.tsx              # Root layout
│   ├── lib/
│   │   └── db.ts                   # Database connection
│   │   ├── auth.ts                  # NextAuth.js configuration
│   │   └── redis.ts                 # Redis connection and rate limiting
│   ├── components/
│   │   └── SessionProvider.tsx      # Session provider wrapper
│   └── utils/
│       └── generateCode.ts         # Short code generation
├── supabase/migrations/             # Database migrations
├── .env.local                      # Environment variables
└── README.md
```

## 🚀 Quick Start

### 1. Clone and Install

```bash
git clone <your-repo>
cd url-shortener
npm install
```

### 2. Set up Database

Choose one of these options:

#### Option A: PlanetScale
1. Create a [PlanetScale](https://planetscale.com) account
2. Create a new database
3. Get your connection string

#### Option B: Supabase
1. Create a [Supabase](https://supabase.com) project
2. Go to Settings > Database
3. Get your connection string

#### Option C: Local MySQL
```bash
# Install MySQL locally or use Docker
docker run --name mysql-shortener -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=urlshortener -p 3306:3306 -d mysql:8.0
```

### 3. Configure Environment

Create `.env.local`:

```env
DATABASE_URL=mysql://username:password@host:port/database
BASE_URL=http://localhost:3000
NEXTAUTH_SECRET=your-secret-key-here
REDIS_URL=redis://localhost:6379
```

### 4. Set up Redis (Optional but Recommended)

For rate limiting, you can use:

#### Option A: Upstash Redis (Recommended for production)
1. Create an [Upstash](https://upstash.com) account
2. Create a Redis database
3. Get your Redis URL

#### Option B: Local Redis
```bash
# Install Redis locally or use Docker
docker run --name redis-shortener -p 6379:6379 -d redis:alpine
```

### 5. Set up Database Schema

Run the SQL commands from the migration files in your database:

```sql
-- Run both migration files:
-- 1. supabase/migrations/20250728115331_turquoise_cliff.sql (URLs table)
-- 2. supabase/migrations/create_auth_tables.sql (Authentication tables)
```

### 6. Run Development Server

```bash
npm run dev
```

Visit [http://localhost:3000](http://localhost:3000) to see your URL shortener!

## 🌐 Deployment

### Deploy to Vercel

1. Push your code to GitHub
2. Import your repository in [Vercel](https://vercel.com)
3. Add environment variables in Vercel dashboard:
   - `DATABASE_URL`
   - `BASE_URL` (your production domain)
   - `NEXTAUTH_SECRET`
4. Deploy! 🎉

### Environment Variables for Production

```env
DATABASE_URL=your_production_database_url
BASE_URL=https://yourdomain.com
NEXTAUTH_SECRET=your-production-secret-key
REDIS_URL=your_redis_url
```

## 📊 API Endpoints

### POST `/api/shorten`

Shorten a URL (with rate limiting).

**Request:**
```json
{
  "url": "https://example.com/very/long/url"
}
```

**Response:**
```json
{
  "shortUrl": "https://yourdomain.com/abc123",
  "rateLimitRemaining": 9
}
```

**Rate Limit Response (429):**
```json
{
  "error": "Too many requests. Please try again later.",
  "resetTime": 1640995200000
}
```

### GET `/api/user-urls`

Get authenticated user's URLs.

**Response:**
```json
{
  "urls": [
    {
      "id": 1,
      "short_code": "abc123",
      "original_url": "https://example.com",
      "shortUrl": "https://yourdomain.com/abc123",
      "created_at": "2024-01-01T00:00:00.000Z",
      "click_count": 42,
      "expires_at": null
    }
  ]
}
```

### DELETE `/api/user-urls?code=abc123`

Delete a user's URL.

### GET `/[slug]`

Redirect to original URL and increment click count.

## 🔧 Customization

### Change Short Code Length

Edit `src/utils/generateCode.ts`:

```typescript
export const generateShortCode = (): string => nanoid(8); // Change from 6 to 8
```

### Add Custom Domains

Update your environment variables and modify the `BASE_URL` logic in the API route.

### Enable Analytics

The database schema includes an optional `url_analytics` table for detailed tracking. Implement analytics by:

1. Creating the analytics table
2. Logging clicks in the redirect handler
3. Building an analytics dashboard

## 🚀 Future Enhancements

- [x] User authentication and dashboard
- [x] QR code generation
- [x] API rate limiting
- [ ] Custom aliases for short URLs
- [ ] Bulk URL shortening
- [ ] Link expiration
- [ ] Advanced analytics and reporting
- [ ] Custom domains
- [ ] Link preview and safety checking
- [ ] Team collaboration features
- [ ] API key management
- [ ] Webhook notifications

## 📝 License

MIT License - feel free to use this project for personal or commercial purposes.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

---

Built with ❤️ using Next.js, Tailwind CSS, and modern web technologies.