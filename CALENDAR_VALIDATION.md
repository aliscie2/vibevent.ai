# Calendar Event Validation System

This document explains how event creation validation works, including conflict prevention, availability checking, and shared availability intersection.

---

## Event Creation Validation

### Validation Scenarios

The system uses 5 different validation scenarios based on context:

1. **OWN_CALENDAR_GOOGLE** - Your calendar with Google Calendar connected
2. **OWN_CALENDAR_BACKEND** - Your calendar without Google Calendar
3. **SHARED_CALENDAR_BASIC** - Viewing someone else's calendar (basic)
4. **SHARED_CALENDAR_CONTRIBUTOR** - Booking on shared calendar with conflict checking
5. **SHARED_CALENDAR_ADVANCED** - Dual availability checking (both calendars)

### Validation Flow

```typescript
// From useEventActions.ts
const createEvent = async (eventData, slotInfo, onSuccess, onError) => {
  // 1. Build validation context
  const validationContext = {
    calendar,
    profile,
    eventData,
    slotInfo,
    isGoogleConnected,
  };

  // 2. Run validation
  const validationResult = ValidationEngine.validate(validationContext);

  // 3. Check if valid
  if (!validationResult.isValid) {
    onError(validationResult.errors.join("\n"));
    return;
  }

  // 4. Create event
  // ...
};
```

### Validation Rules

**All Scenarios Check:**
- ✅ Event data completeness (title, time slots)
- ✅ Time order (start before end)
- ✅ Past time prevention

**Shared Calendar Scenarios Also Check:**
- ✅ Owner availability
- ✅ Blocked slots
- ✅ Event conflicts
- ✅ Permissions

---

## Preventing Event Intersections

### Conflict Detection

Events cannot overlap with existing events. The system uses time-based overlap detection:

```typescript
// From validationHelpers.ts
export const hasTimeConflict = (
  newEvent: { start_time: number; end_time: number },
  existingEvents: Event[],
  excludeEventId?: string,
): boolean => {
  return existingEvents.some((event) => {
    if (excludeEventId && event.id === excludeEventId) {
      return false; // Skip event being edited
    }

    const eventStart = Number(event.start_time);
    const eventEnd = Number(event.end_time);
    const newStart = Number(newEvent.start_time);
    const newEnd = Number(newEvent.end_time);

    // Overlap formula: (StartA < EndB) AND (EndA > StartB)
    return newStart < eventEnd && newEnd > eventStart;
  });
};
```

**Example:**
- Existing event: 9:00 AM - 10:00 AM
- Try to book: 9:30 AM - 10:30 AM
- ❌ **Rejected**: 9:30 < 10:00 AND 10:30 > 9:00 (overlaps)

---

## Availability Boundaries

### Preventing Events Outside Availability

Events can only be created within defined availability windows.

**Availability Check:**
```typescript
// From validationHelpers.ts
export const checkOwnerAvailability = (context) => {
  const { calendar, slotInfo } = context;
  const availabilities = calendar.availabilities || [];

  const { isAvailable, isBlocked } = checkAvailability(
    slotInfo.start,
    availabilities,
  );

  if (isBlocked) {
    return { isValid: false, errors: ["Time slot is blocked"] };
  }

  if (!isAvailable) {
    return { isValid: false, errors: ["Time slot not available"] };
  }

  return { isValid: true, errors: [] };
};
```

**Checking Steps:**

1. **Day Check** - Is event on an available day?
   ```typescript
   const isWithinWeeklySchedule = (date, availability) => {
     const dayOfWeek = date.getDay();
     const adjustedDay = dayOfWeek === 0 ? 7 : dayOfWeek; // Sunday = 7
     return availability.schedule_type.WeeklyRecurring.days.includes(adjustedDay);
   };
   ```

2. **Time Check** - Is event within available hours?
   ```typescript
   const isWithinTimeSlot = (date, timeSlot) => {
     const slotStart = new Date(Number(timeSlot.start_time) / 1000000);
     const slotEnd = new Date(Number(timeSlot.end_time) / 1000000);
     const hours = date.getHours();
     const minutes = date.getMinutes();
     
     return currentTime >= slotStart && currentTime < slotEnd;
   };
   ```

3. **Block Check** - Is time slot explicitly blocked?

**Example:**
- Available: Monday-Friday, 9:00 AM - 5:00 PM
- Try Saturday 10:00 AM → ❌ Not an available day
- Try Monday 8:00 AM → ❌ Not within time slot
- Try Monday 10:00 AM → ✅ Within availability

---

## Shared Availability Intersection

### Finding Common Available Time

When two people need to meet, the system finds the **intersection** of their availabilities.

**Concept:**
- Person A available: 9:00 AM - 1:00 PM
- Person B available: 10:00 AM - 3:00 PM
- **Shared availability**: 10:00 AM - 1:00 PM

### How It Works

**Scenario Detection:**
```typescript
// From validationEngine.ts
static determineScenario(context) {
  const { calendar, profile, userCalendar } = context;
  const isSharedCalendar = calendar.owner !== profile.id;
  
  // Advanced scenario: User has their own calendar
  if (isSharedCalendar && userCalendar) {
    return ValidationScenario.SHARED_CALENDAR_ADVANCED;
  }
  // ...
}
```

**Dual Validation:**
```typescript
// From validationRules.ts - Scenario 5
[ValidationScenario.SHARED_CALENDAR_ADVANCED]: [
  {
    name: "check_owner_availability",
    validate: checkOwnerAvailability,
    errorMessage: "Not available in calendar owner's schedule",
  },
  {
    name: "check_user_availability",
    validate: checkUserAvailability,
    errorMessage: "Not available in your own schedule",
  },
  {
    name: "check_owner_event_conflicts",
    validate: checkOwnerEventConflicts,
    errorMessage: "Conflicts with existing event on this calendar",
  },
  {
    name: "check_user_event_conflicts",
    validate: checkUserEventConflicts,
    errorMessage: "Conflicts with event on your own calendar",
  },
  // ...
]
```

**User Availability Check:**
```typescript
// From validationHelpers.ts
export const checkUserAvailability = (context) => {
  const { userCalendar, slotInfo } = context;

  if (!userCalendar) {
    return { isValid: true, errors: [] };
  }

  const availabilities = userCalendar.availabilities || [];
  const { isAvailable, isBlocked } = checkAvailability(
    slotInfo.start,
    availabilities,
  );

  if (isBlocked || !isAvailable) {
    return { isValid: false, errors: ["Not available in your schedule"] };
  }

  return { isValid: true, errors: [] };
};
```

### Example Calculation

**Person A (Owner):**
- Monday: 9:00 AM - 1:00 PM ✅
- Tuesday: 10:00 AM - 2:00 PM ✅

**Person B (User):**
- Monday: 10:00 AM - 3:00 PM ✅
- Tuesday: 9:00 AM - 12:00 PM ✅

**Shared Availability (Intersection):**
- Monday: 10:00 AM - 1:00 PM ✅ (overlap of 9-1 and 10-3)
- Tuesday: 10:00 AM - 12:00 PM ✅ (overlap of 10-2 and 9-12)

**Booking Attempts:**
- Monday 9:30 AM → ❌ Owner available, User not available yet
- Monday 10:30 AM → ✅ Both available
- Monday 2:00 PM → ❌ Owner not available anymore
- Tuesday 11:00 AM → ✅ Both available

---

## Validation Priority Order

When creating an event, checks run in this order:

1. ✅ Event data completeness
2. ✅ Past time check
3. ✅ Owner availability check
4. ✅ User availability check (if applicable)
5. ✅ Blocked slot check
6. ✅ Owner event conflict check
7. ✅ User event conflict check (if applicable)
8. ✅ Permission check

All checks must pass for event creation to succeed.

---

## Key Files

**Validation Engine:**
- `src/frontend/pages/calendar/validation/validationEngine.ts` - Main validation orchestration
- `src/frontend/pages/calendar/validation/validationRules.ts` - Scenario-based rules
- `src/frontend/pages/calendar/validation/validationHelpers.ts` - Reusable validation functions

**Event Actions:**
- `src/frontend/pages/calendar/hooks/useEventActions.ts` - Event CRUD with validation

**Calendar Logic:**
- `src/frontend/pages/calendar/hooks/useCalendar.ts` - Calendar state and slot status
