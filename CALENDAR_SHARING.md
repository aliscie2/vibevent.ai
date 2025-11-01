# Calendar Sharing System

Simple explanation of how calendar sharing works in our platform.

## How It Works

```
SHARING FLOW
════════════

1. GET SHARE ID
   ┌─────────────┐
   │   Owner     │ Has calendar with ID (calendar.id)
   └──────┬──────┘
          │
          ▼
   📋 Share Link: /share_calendar?id={calendar.id}


2. SHARE WITH OTHERS
   ┌─────────────┐
   │   Owner     │────► Sends link to others
   └─────────────┘


3. ACCESS SHARED CALENDAR
   ┌─────────────┐
   │   Viewer    │ Clicks link
   └──────┬──────┘
          │
          ▼
   Extract ID from URL → Call backend.get_calendar(id) → Show calendar
```

## Code Implementation

**Getting the Share ID:**
```typescript
// From ShareCalendarButton.tsx
const shareLink = `${window.location.origin}/share_calendar?id=${calendar.id}`;
```

**Accessing Shared Calendar:**
```typescript
// From sharedCalendar.tsx
const id = urlParams.get("id");
const res = await backendActor.get_calendar(id);
```

**Backend Retrieval:**
```rust
// From src/backend/src/calendar/queries.rs
fn get_calendar(calendar_id: String) -> Option<Calendar> {
    Calendar::get_calendar(&calendar_id).ok()
}
```

## Privacy Filters

When someone views a shared calendar, the backend automatically:
- Hides other people's event details (title, description)
- Filters out past events
- Shows only availability slots and their own bookings

## Key Files

- `src/frontend/pages/calendar/components/ShareCalendarButton.tsx` - Share button
- `src/frontend/pages/calendar/sharedCalendar.tsx` - Shared calendar view
- `src/backend/src/calendar/queries.rs` - Backend get_calendar function
- `src/backend/src/calendar/types.rs` - Privacy filters
