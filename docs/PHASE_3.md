# Phase 3: Email Notifications & Activity Feed

## Overview

Phase 3 adds comprehensive email notifications and activity tracking to the Bug Tracker application. Users receive email notifications for important events, and all project activities are logged for audit trails and activity feeds.

## Features Implemented

### 1. Email Notifications System

**Email Service** (`backend/services/emailService.js`)
- Integrated with SendGrid for reliable email delivery
- Pre-configured email templates for different notification types
- Support for bulk and individual email sending

**Email Types:**
- **Ticket Created** - Notifies when a new ticket is created in a project
- **Ticket Assigned** - Alerts user when assigned a ticket
- **Ticket Updated** - Informs about changes to tracked tickets
- **Comment Added** - Notifies about new comments on tickets
- **Team Invitation** - Sends project invitation to new team members

### 2. Activity Logging System

**Activity Service** (`backend/services/activityService.js`)
- Automatically logs all project activities (tickets, comments, assignments)
- Rich querying and filtering capabilities
- Analytics and summaries
- Automatic cleanup of old logs

**Supported Actions:**
- `created` - Entity creation
- `updated` - Entity modification
- `deleted` - Entity deletion
- `commented` - Comment addition
- `assigned` - Ticket assignment

### 3. Email Preferences

**Email Preference Service** (`backend/services/emailPreferenceService.js`)
- Per-user email notification settings
- Granular control over which notifications to receive
- Preset quick actions (Enable All / Disable All)
- Opt-in/opt-out for specific notification types

**Available Preferences:**
- Ticket Created notifications
- Ticket Assigned notifications
- Ticket Updated notifications
- Comment Added notifications
- Team Updates notifications
- Daily Digest emails
- Weekly Summary emails

### 4. Frontend Components

**Activity Feed Component** (`frontend/src/components/ActivityFeed.jsx`)
- Displays project activities in chronological order
- Color-coded activity types with icons
- Expandable "View changes" for modification details
- Pagination support for large activity logs
- Relative time formatting (e.g., "2 minutes ago")

**Email Preferences Component** (`frontend/src/components/EmailPreferences.jsx`)
- User-friendly preference management UI
- Toggle switches for each notification type
- Enable All / Disable All quick actions
- Real-time preference updates
- Clear descriptions for each notification type

## Database Schema

### email_preferences Table

```sql
CREATE TABLE email_preferences (
  id UUID PRIMARY KEY,
  user_id UUID UNIQUE NOT NULL,
  ticket_created BOOLEAN DEFAULT true,
  ticket_assigned BOOLEAN DEFAULT true,
  ticket_updated BOOLEAN DEFAULT true,
  comment_added BOOLEAN DEFAULT true,
  team_updates BOOLEAN DEFAULT true,
  daily_digest BOOLEAN DEFAULT false,
  weekly_summary BOOLEAN DEFAULT false,
  created_at TIMESTAMP,
  updated_at TIMESTAMP
);
```

### activity_logs Table

```sql
CREATE TABLE activity_logs (
  id UUID PRIMARY KEY,
  user_id UUID NOT NULL,
  project_id UUID NOT NULL,
  action TEXT NOT NULL,
  entity_type TEXT NOT NULL,
  entity_id UUID,
  details TEXT NOT NULL,
  changes JSONB,
  ip_address INET,
  user_agent TEXT,
  created_at TIMESTAMP
);
```

## API Endpoints

### Activity Endpoints

- `GET /projects/:projectId/activity` - Get project activity feed
  - Query params: `limit` (default: 50), `offset` (default: 0)
  
- `GET /tickets/:ticketId/activity` - Get activity for specific ticket
  - Query params: `limit` (default: 20)
  
- `GET /user/activity` - Get current user's activities
  - Query params: `limit` (default: 30), `offset` (default: 0)
  
- `GET /projects/:projectId/activity/summary` - Get activity summary
  - Query params: `days` (default: 7)
  
- `GET /projects/:projectId/activity/active-users` - Get most active users
  - Query params: `days` (default: 30), `limit` (default: 10)

### Email Preference Endpoints

- `GET /user/email-preferences` - Get user's email preferences
  
- `PUT /user/email-preferences` - Update all preferences
  - Body: `{ ticket_created: boolean, ... }`
  
- `PATCH /user/email-preferences/:key` - Update single preference
  - Body: `{ value: boolean }`
  
- `POST /user/email-preferences/enable-all` - Enable all emails
  
- `POST /user/email-preferences/disable-all` - Disable all emails

## Service Layer

### Email Service Methods

```javascript
// Send single email
await emailService.sendEmail(to, emailType, templateData);

// Send bulk emails
await emailService.sendBulkEmail(recipients, emailType, templateData);

// Send specific email types
await emailService.sendTicketCreatedEmail(user, ticket, project);
await emailService.sendTicketAssignedEmail(user, ticket, project);
await emailService.sendTicketUpdatedEmail(user, ticket, changes);
await emailService.sendCommentEmail(user, comment, ticket, project);
await emailService.sendTeamInvitationEmail(user, project);
```

### Activity Service Methods

```javascript
// Log activity
await activityService.logActivity({
  userId,
  projectId,
  action,
  entityType,
  entityId,
  details,
  changes
});

// Query activities
await activityService.getProjectActivity(projectId, limit, offset);
await activityService.getTicketActivity(ticketId, limit);
await activityService.getUserActivity(userId, limit, offset);
await activityService.getActivityByDateRange(projectId, startDate, endDate);
await activityService.getActivityByType(projectId, action);

// Analytics
await activityService.getActivitySummary(projectId, days);
await activityService.getMostActiveUsers(projectId, days, limit);

// Maintenance
await activityService.deleteOldActivities(projectId, daysOld);
```

### Email Preference Service Methods

```javascript
// Get preferences
const prefs = await emailPreferenceService.getPreferences(userId);

// Update preferences
await emailPreferenceService.updatePreferences(userId, updates);
await emailPreferenceService.updatePreference(userId, key, value);

// Bulk actions
await emailPreferenceService.enableAllEmails(userId);
await emailPreferenceService.disableAllEmails(userId);

// Check if should send
const should = await emailPreferenceService.shouldSendEmail(userId, 'ticket_created');
```

## Integration Points

### With Ticket CRUD Operations

When creating/updating tickets, integrate email and activity logging:

```javascript
// In ticket creation route
await ticketService.create(ticketData);
await activityService.logActivity({
  userId: req.user.id,
  projectId,
  action: 'created',
  entityType: 'ticket',
  entityId: ticket.id,
  details: `Created ticket: ${ticket.title}`,
});

// Send emails to project members
const members = await projectService.getMembers(projectId);
for (const member of members) {
  const shouldSend = await emailPreferenceService.shouldSendEmail(member.user_id, 'ticket_created');
  if (shouldSend) {
    await emailService.sendTicketCreatedEmail(member, ticket, project);
  }
}
```

### With Comments

```javascript
// When adding comment
await commentService.create(commentData);
await activityService.logActivity({
  userId: req.user.id,
  projectId,
  action: 'commented',
  entityType: 'comment',
  entityId: comment.id,
  details: `Commented on ticket: ${ticket.title}`,
});

// Notify ticket owner and assignee
const notifyUsers = [ticket.created_by, ticket.assigned_to].filter(Boolean);
for (const userId of notifyUsers) {
  const shouldSend = await emailPreferenceService.shouldSendEmail(userId, 'comment_added');
  if (shouldSend) {
    await emailService.sendCommentEmail(user, comment, ticket, project);
  }
}
```

## Configuration

### Environment Variables

Add to `.env`:

```env
# SendGrid Configuration
SENDGRID_API_KEY=your_sendgrid_api_key
SENDGRID_FROM_EMAIL=noreply@bugtracker.com

# Email Template IDs (from SendGrid)
SENDGRID_TEMPLATE_TICKET_CREATED=d-xxxxx
SENDGRID_TEMPLATE_TICKET_ASSIGNED=d-xxxxx
SENDGRID_TEMPLATE_TICKET_UPDATED=d-xxxxx
SENDGRID_TEMPLATE_COMMENT=d-xxxxx
SENDGRID_TEMPLATE_TEAM_INVITE=d-xxxxx

# Activity Log Configuration
ACTIVITY_LOG_RETENTION_DAYS=90
```

### SendGrid Setup

1. Create SendGrid account at https://sendgrid.com
2. Create email templates for each notification type
3. Note the template IDs (format: `d-xxxxx`)
4. Add to environment variables
5. Verify sender email address

## Testing

### Test Email Service

```javascript
// Test sending email
const result = await emailService.sendEmail(
  'test@example.com',
  'ticket_created',
  { ticket: { id: '123', title: 'Test' }, project: { name: 'Project' } }
);
```

### Test Activity Logging

```javascript
// Test logging activity
await activityService.logActivity({
  userId: 'user-123',
  projectId: 'project-456',
  action: 'created',
  entityType: 'ticket',
  entityId: 'ticket-789',
  details: 'Created test ticket',
});

// Retrieve activities
const activities = await activityService.getProjectActivity('project-456', 10, 0);
```

### Test Email Preferences

```javascript
// Get preferences
const prefs = await emailPreferenceService.getPreferences('user-123');

// Update single preference
await emailPreferenceService.updatePreference('user-123', 'ticket_created', false);

// Check before sending
const should = await emailPreferenceService.shouldSendEmail('user-123', 'ticket_created');
if (should) {
  await emailService.sendEmail(...);
}
```

## Frontend Integration

### Using Activity Feed Component

```jsx
import ActivityFeed from './components/ActivityFeed';

function ProjectPage() {
  const { projectId } = useParams();
  
  return (
    <div>
      <ActivityFeed projectId={projectId} limit={30} />
    </div>
  );
}
```

### Using Email Preferences Component

```jsx
import EmailPreferences from './components/EmailPreferences';

function SettingsPage() {
  return (
    <div>
      <EmailPreferences />
    </div>
  );
}
```

## Security Considerations

1. **RLS Policies**: Activity logs only visible to project members
2. **Preference Privacy**: Users can only access their own preferences
3. **Server-Side Logging**: Activity logs only created server-side, not from client
4. **Rate Limiting**: Implement rate limiting on email endpoints to prevent abuse
5. **Email Validation**: Verify email addresses before sending
6. **Token-Based Auth**: All endpoints require valid JWT token

## Performance Optimization

1. **Indexes**: activity_logs table indexed on project_id, user_id, action, created_at
2. **Pagination**: Activity feeds paginated with limit/offset
3. **Caching**: Consider Redis for frequently accessed summaries
4. **Cleanup**: Old activities automatically deleted based on retention policy
5. **Bulk Operations**: EmailService supports bulk sending for newsletters

## Future Enhancements

- Scheduled daily/weekly email digests
- Email digest templates with activity summaries
- Webhook integrations for external notifications
- SMS notifications support
- Push notifications for mobile app
- Custom notification rules and filters
- Email template customization per project
- Activity export (CSV/PDF)
- Email delivery tracking and analytics
