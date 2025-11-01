# Vibevent.ai White Paper

**Vision:** Making it easy to meet each other.

---

## The Problem We're Solving

Meeting people—whether for work, learning, or connection—is unnecessarily hard. We face four major friction points:

### 1. Time Zone & Availability Chaos
Two people in different time zones trying to schedule a meeting often spend days negotiating times. Add a third or fourth person, and it becomes nearly impossible. Even people in the same city struggle to find overlapping free time when their schedules differ.

### 2. Calendar Rigidity
Most calendar systems are static. You set your availability once, but life changes. You might say you're free at 9 AM every day, but after attending several 11 AM meetings, your actual preference has shifted—yet your calendar doesn't adapt.

### 3. Random Event Attendees
When you join events, you meet random people with completely different priorities and values. Someone focused on spirituality and wellness ends up in rooms with people focused on status and competition. The mismatch drains energy and wastes time.

### 4. Manual Coordination Overhead
Creating group events means manually checking everyone's availability, sending invites, tracking responses, and resolving conflicts. This gets exponentially harder with more attendees.

**Vibevent.ai solves all four problems with AI-powered scheduling and like-minded matching.**

---

## What is Vibevent.ai?

Vibevent.ai is a calendar tool with a community layer. It serves three types of users:

1. **Calendar users** — Anyone who needs smarter scheduling
2. **Digital nomads** — People moving between locations who want to meet locals and fellow travelers
3. **Knowledge exchangers** — People who attend group events to learn, teach, and collaborate

The platform has two modes:
- Use it as a **smart calendar** alone (no community features)
- Use it as a **calendar + matching tool** to find and meet like-minded people

---

## How It Works

### Core Pages

#### 1. Profile
View your basic information, including:
- Username and email
- Growth level (explained below)
- Skills (e.g., software, marketing, design)
- Matching score with other users

#### 2. Availabilities
Define when you're free. Examples:
- Available Monday-Friday, 9 AM - 6 PM
- Available for family meetings on weekends
- Available for business calls Tuesday/Thursday mornings

You can connect multiple Google Calendars. Vibevent reads them to avoid scheduling conflicts.

#### 3. Events
Two types of events:
- **Private events:** Invite-only or link-shared. Not visible publicly.
- **Public events:** Anyone can join (up to max seats). Ordered by start time and proximity to you. Filterable by tags.

All events are stored in Google Calendar, not our backend. We only store your availabilities and profile data.

---

## Key Features

### AI-Powered Event Creation
Click "Create Event" and talk to the AI. Examples:
- "Create a cooking session to learn vegan recipes"
- "Schedule a brainstorming call with Sarah and Tom next week"
- "Find a time for 5 people to meet online this month"

The AI suggests the top 3 possible times based on everyone's availability and time zones. It can also create the event directly in Google Calendar if you confirm.

### Quick Gather
Need to meet someone right now? Click "Quick Gather" and:
- Set how long you're available (e.g., next 1 hour)
- Set radius for in-person meetups (e.g., within 5 km)
- See who else has Quick Gather turned on

For **in-person** meetups: Shows people near you (by location) with the highest match scores.  
For **online** meetups: Shows your contacts who are available now.

Privacy: Your exact location is never shared. Other users only see "someone nearby" without coordinates.

### Like-Minded Matching
Vibevent matches you with people who share your:
1. **Growth level** (highest priority)
2. **Skills** (second priority)
3. **Location** (third priority)

When you create an event, the AI suggests 5 people who are:
- Available at the suggested time
- Close to you (if in-person)
- Aligned with your values and interests

---

## Understanding Growth Levels

People grow through different stages of focus. We call this your **growth level**:

- **Survival-focused** — Basic needs, immediate concerns (like a caveman mindset)
- **Tribe-focused** — Family, tradition, belonging to a group
- **Success-focused** — Achievement, status, competition
- **Community-focused** — Fairness, equality, social harmony
- **Spiritual-focused** — Humanity, well-being, inner peace, global consciousness

Your level isn't better or worse—it's where you are right now. Someone focused on spirituality and veganism won't match well with someone focused on competition and status. That's okay. We match people at similar stages so conversations flow naturally.

**Example:**  
- User A: Spiritual-focused, vegan, interested in yoga → Matches with others at spiritual level who share similar values  
- User B: Success-focused, entrepreneur, interested in startups → Matches with others focused on achievement and business growth

Your growth level is assessed through conversations with the AI and updated automatically as you attend events and interact with the platform.

---

## The Matching Algorithm

We use an **inverted index** system to match people efficiently:

### Two Separate Indexes:
1. **Growth Level Index** — Keywords like "survival," "tribe," "success," "community," "spiritual" (each with 3 sub-levels for precision)
2. **Skills Index** — Keywords like "software," "marketing," "design," "consulting," "writing"

When you create an event or open the matching page, the AI searches both indexes and ranks results by:
1. Location (closest first for in-person events)
2. Growth level alignment
3. Skill overlap

This approach keeps searches fast and accurate without storing unnecessary data.

---

## Self-Healing Intelligence

Vibevent learns from your behavior and suggests adjustments. Three types of self-healing:

### 1. Profile Updates
You initially say you're "success-focused," but after attending several yoga and meditation events, the AI notices you're more "spiritual-focused" and suggests updating your profile.

### 2. Event Preferences
You attend events about business, but later shift to attending more wellness events. The AI suggests adjusting your interests.

### 3. Availability Adjustments
You say you're free at 9 AM, but you keep attending events at 11 AM. The AI suggests shifting your default availability.

**Important:** The AI only suggests changes. You approve them. Your calendar stays under your control.

---

## Privacy & Data Security

We take privacy seriously:

- **No event storage:** Events are stored only in your Google Calendar, not our servers
- **Location privacy:** Quick Gather shows proximity, not exact coordinates
- **Minimal data:** We store only username, email, growth level, skills, availabilities, and match scores
- **Google Calendar integration:** We read your calendar to check conflicts, but never modify it without your permission

---

## Who Should Use Vibevent?

### Calendar Users
If you're tired of:
- Back-and-forth emails to schedule meetings
- Manually checking time zones
- Rigid calendar systems that don't adapt

Use Vibevent as a smarter scheduling tool. No community features needed.

### Digital Nomads
If you:
- Move between cities or countries frequently
- Want to meet locals and other travelers
- Need flexible scheduling across time zones

Use Vibevent to find nearby people with shared interests and schedule meetups instantly.

### Knowledge Exchangers
If you:
- Attend or host group events regularly
- Want to learn from or teach others
- Prefer small, curated groups over random attendees

Use Vibevent to create events with like-minded people who match your growth level and skills.

---

## Technical Architecture (Overview)

### Backend
- **Inverted index matching:** Two indexes (growth levels + skills) for fast searches
- **Availability storage:** User-defined time slots stored by user ID
- **Profile storage:** Basic user data linked to matching keywords

### Frontend
- **AI chat interface:** For event creation and profile assessment
- **Google Calendar integration:** Read-only for conflict checking, write access for event creation
- **Quick Gather mode:** Real-time availability broadcasting

### AI Roles
1. **Schedule AI:** Suggests optimal meeting times across time zones and availabilities
2. **Matching AI:** Runs when creating events or opening the matching page
3. **Profile AI:** Assesses growth level through conversation and updates based on event attendance

---

## Roadmap & Future Features

- **Tag-based filtering:** Filter public events by custom tags (e.g., "vegan," "startup," "yoga")
- **Weighted matching:** Let users prioritize growth level vs. skills vs. location
- **Advanced self-healing:** Automatically adjust availabilities based on patterns (with user approval)
- **Integration with more calendar systems:** Outlook, Apple Calendar, etc.

---

## Conclusion

Vibevent.ai reimagines how we schedule and meet. By combining AI-powered scheduling with like-minded matching, we eliminate the friction of coordinating across time zones, availabilities, and mismatched values.

Whether you're a busy professional, a traveling nomad, or someone seeking meaningful group experiences, Vibevent makes it easy to meet each other.

**Get started at vibevent.ai**

---

*Questions or feedback? Contact us at [your-email@vibevent.ai]*