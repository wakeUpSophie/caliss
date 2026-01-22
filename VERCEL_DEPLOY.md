# Deploying to Vercel

This guide covers deploying your TanStack Start app with WorkOS AuthKit and Convex to Vercel.

## Prerequisites

1. Nitro plugin installed (already configured in `vite.config.ts`)
2. WorkOS account with AuthKit configured
3. Convex account and project
4. Vercel account

## Required Environment Variables

### In Vercel Dashboard

You need to configure these environment variables in your Vercel project settings:

#### WorkOS AuthKit Variables
- `WORKOS_CLIENT_ID` - Your WorkOS client ID
- `WORKOS_API_KEY` - Your WorkOS API key
- `WORKOS_COOKIE_PASSWORD` - Secure password (minimum 32 characters)

#### Convex Variables
- `VITE_CONVEX_URL` - Your Convex deployment URL (e.g., `https://your-deployment.convex.cloud`)

#### Vercel System Variables (auto-provided)
These are automatically available in Vercel builds:
- `VERCEL_BRANCH_URL` - Used for preview deployments
- `VERCEL_PROJECT_PRODUCTION_URL` - Used for production deployments

## Deployment Steps

### 1. Install Vercel CLI (if not already installed)

```bash
npm i -g vercel
```

### 2. Link Your Project

```bash
vercel link
```

### 3. Set Environment Variables

You can set environment variables via the Vercel dashboard or CLI:

```bash
# Set production environment variables
vercel env add WORKOS_CLIENT_ID production
vercel env add WORKOS_API_KEY production
vercel env add WORKOS_COOKIE_PASSWORD production
vercel env add VITE_CONVEX_URL production

# Set preview environment variables
vercel env add WORKOS_CLIENT_ID preview
vercel env add WORKOS_API_KEY preview
vercel env add WORKOS_COOKIE_PASSWORD preview
vercel env add VITE_CONVEX_URL preview
```

Or configure them in the Vercel dashboard:
1. Go to your project settings
2. Navigate to "Environment Variables"
3. Add each variable for Production and Preview environments

### 4. Deploy

```bash
# Deploy to production
vercel --prod

# Or deploy preview
vercel
```

## How Auto-Provisioning Works

The `.convex.json` file is configured to automatically provision WorkOS AuthKit environments:

- **Dev**: Uses `localhost:3000` for local development
- **Preview**: Uses Vercel preview URLs (e.g., `https://yourapp-branch.vercel.app`)
- **Prod**: Uses your production domain

Convex reads the environment variables from Vercel's build environment and automatically configures:
- Redirect URIs
- CORS origins
- App homepage URLs

## Troubleshooting

### Build Fails with Missing Environment Variables

Ensure all required environment variables are set in Vercel for the appropriate environment (production/preview).

### AuthKit Callback Errors

Check that:
1. The redirect URI in WorkOS matches your Vercel URL
2. CORS origins are properly configured
3. The `WORKOS_COOKIE_PASSWORD` is at least 32 characters

### Convex Connection Issues

Verify:
1. `VITE_CONVEX_URL` is correctly set
2. The Convex deployment is accessible
3. AuthKit configuration in `convex/auth.config.ts` uses correct `WORKOS_CLIENT_ID`

## Additional Resources

- [TanStack Start on Vercel](https://vercel.com/docs/frameworks/full-stack/tanstack-start)
- [Nitro Vercel Provider](https://v3.nitro.build/deploy/providers/vercel)
- [Convex AuthKit Auto-Provisioning](https://docs.convex.dev/auth/authkit/auto-provision)
