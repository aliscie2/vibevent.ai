# Vibevent.ai

> Making it easy to meet each other.

AI-powered calendar and community platform that eliminates scheduling friction and enables like-minded connections through intelligent matching.

## Features

- 🤖 **AI-Powered Event Creation** — Natural language event scheduling
- ⚡ **Quick Gather** — Spontaneous meetups with nearby aligned people
- 🎯 **Like-Minded Matching** — Growth-based and skill-based connections
- 📅 **Smart Scheduling** — Automatic time zone and availability coordination
- 🔄 **Self-Healing Intelligence** — Learns from behavior patterns
- 🔒 **Privacy-First** — No event storage, Google Calendar integration only

## Tech Stack

### Frontend
- **React 19.2.0** — Latest React with concurrent features
- **Tailwind CSS** — Utility-first styling
- **Mobile-First Design** — Optimized for mobile devices

### Backend
- **ICP (Internet Computer Protocol)** — Decentralized blockchain infrastructure
- **Juno** — Serverless platform on ICP

### Testing
- **PocketIC** — Backend canister testing
- **Playwright** — End-to-end frontend testing

## Prerequisites

- Node.js 18+ and npm/yarn
- dfx (DFINITY Canister SDK)
- Google Calendar API credentials

## Installation

```bash
# Clone repository
git clone https://github.com/yourusername/vibevent.git
cd vibevent

# Install dependencies
npm install

# Install dfx (if not already installed)
sh -ci "$(curl -fsSL https://internetcomputer.org/install.sh)"
```

## Development

### Start Local ICP Replica

```bash
# Start dfx in background
dfx start --background --clean

# Deploy canisters locally
dfx deploy
```

### Run Frontend

```bash
# Development server
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

### Environment Variables

Create `.env` file:

```env
VITE_GOOGLE_CLIENT_ID=your_google_client_id
VITE_GOOGLE_API_KEY=your_google_api_key
VITE_CANISTER_ID_BACKEND=your_backend_canister_id
```

## Testing

### Backend Tests (PocketIC)

```bash
# Run all backend tests
npm run test:backend

# Run specific test suite
npm run test:backend -- --grep "matching"

# Watch mode
npm run test:backend:watch
```

### Frontend Tests (Playwright)

```bash
# Install Playwright browsers
npx playwright install

# Run all e2e tests
npm run test:e2e

# Run tests in UI mode
npm run test:e2e:ui

# Run specific test file
npx playwright test tests/auth.spec.ts

# Generate test report
npx playwright show-report
```

## Project Structure

```
vibevent/
├── src/
│   ├── components/          # React components
│   │   ├── Profile/
│   │   ├── Events/
│   │   ├── Availability/
│   │   └── QuickGather/
│   ├── hooks/               # Custom React hooks
│   ├── utils/               # Helper functions
│   ├── services/            # API services
│   │   ├── googleCalendar.ts
│   │   ├── matching.ts
│   │   └── ai.ts
│   ├── types/               # TypeScript types
│   └── App.tsx              # Root component
├── backend/
│   ├── canisters/           # ICP canisters
│   │   ├── matching/
│   │   ├── profile/
│   │   └── availability/
│   └── tests/               # PocketIC tests
├── tests/                   # Playwright e2e tests
│   ├── auth.spec.ts
│   ├── events.spec.ts
│   ├── matching.spec.ts
│   └── quick-gather.spec.ts
├── dfx.json                 # DFX configuration
├── playwright.config.ts     # Playwright configuration
└── tailwind.config.js       # Tailwind configuration
```

## Architecture

### Frontend Architecture
- **Mobile-First**: All components optimized for mobile viewports first
- **Component-Based**: Reusable React components with Tailwind styling
- **State Management**: React 19 Context API + hooks
- **Routing**: React Router v6

### Backend Architecture
- **ICP Canisters**: Decentralized smart contracts on Internet Computer
- **Juno Integration**: Serverless functions and storage
- **Inverted Index**: Dual indexes for growth levels and skills matching
- **Google Calendar API**: OAuth 2.0 integration for calendar operations

### Matching Algorithm
1. Location filtering (for in-person events)
2. Growth level index query
3. Skills index query
4. Composite scoring: Location 40% + Growth 40% + Skills 20%
5. Return top 5 matches

## Deployment

### Deploy to ICP Mainnet

```bash
# Add mainnet identity
dfx identity new mainnet
dfx identity use mainnet

# Deploy to mainnet
dfx deploy --network ic

# Get canister IDs
dfx canister --network ic id backend
dfx canister --network ic id frontend
```

### Deploy Frontend (Vercel/Netlify)

```bash
# Build
npm run build

# Deploy (example with Vercel)
vercel --prod
```

## API Documentation

### Key Endpoints

**Matching API**
```typescript
POST /api/matching/find
Body: { userId, eventType, location?, limit: 5 }
Returns: Array<MatchedUser>
```

**Schedule API**
```typescript
POST /api/schedule/suggest
Body: { attendees[], duration, preferredRange? }
Returns: Array<TimeSlot>
```

**Profile API**
```typescript
GET /api/profile/:userId
PUT /api/profile/:userId
Body: { growthLevel, skills, availability }
```

**Quick Gather API**
```typescript
POST /api/quick-gather/activate
Body: { duration, radius?, type: 'online' | 'in-person' }
Returns: Array<ActiveUser>
```

## Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

### Code Style
- Use Prettier for formatting
- Follow React hooks best practices
- Write tests for new features
- Mobile-first responsive design

## Mobile Optimization

All components follow mobile-first principles:

```tsx
// Example: Mobile-first Tailwind classes
<div className="w-full p-4 md:w-1/2 md:p-6 lg:w-1/3 lg:p-8">
  {/* Mobile: full width, 1rem padding */}
  {/* Tablet: half width, 1.5rem padding */}
  {/* Desktop: third width, 2rem padding */}
</div>
```

### Touch Optimizations
- Minimum touch target: 44x44px
- Bottom navigation for thumb reach
- Swipe gestures for common actions
- Optimized for one-handed use

## Performance

- **Lighthouse Score Target**: 90+ on mobile
- **First Contentful Paint**: < 1.5s
- **Time to Interactive**: < 3.5s
- **Bundle Size**: < 200KB (gzipped)

## Security

- No event data stored in backend
- OAuth 2.0 for Google Calendar access
- Location data anonymized (proximity only)
- User controls all data through Google settings
- ICP blockchain security guarantees

## License

MIT License - see [LICENSE](LICENSE) file for details

## Support

- **Documentation**: [docs.vibevent.ai](https://docs.vibevent.ai)
- **Issues**: [GitHub Issues](https://github.com/yourusername/vibevent/issues)
- **Email**: contact@vibevent.ai

## Roadmap


- [ ] Google Calendar integration
- [ ] Quick Gather feature
- [ ] Core matching algorithm
- [ ] Self-healing intelligence
- [ ] Weighted matching preferences (Q1 2026)
- [ ] Mobile apps (iOS/Android) (Q3 2026)
- [ ] Team features (Q3 2026)
- [ ] Outlook/Apple Calendar integration (Q3 2026)

---

Made with ❤️ by the Vibevent team