# AI Chatbot Development Guide

## Project Structure

### Core Components

#### Authentication (`app/(auth)/`)
- `actions.ts`: Server actions for login/registration
- `auth.ts`: NextAuth configuration and credentials provider
- `auth.config.ts`: Authentication configuration settings

#### Chat Interface (`app/(chat)/`)
- `page.tsx`: Main chat interface with message history
- `chat/[id]/page.tsx`: Individual chat session pages
- `api/chat/route.ts`: Chat API endpoint handling

#### Database (`lib/db/`)
- `schema.ts`: Database schema definitions using Drizzle ORM
- `queries.ts`: Database operations (users, messages, chats)
- `migrate.ts`: Database migration utilities

### Key Components (`components/`)

#### Chat Components
- `chat.tsx`: Main chat component with message handling
- `chat-header.tsx`: Chat interface header with controls
- `messages.tsx`: Message display and rendering
- `multimodal-input.tsx`: Chat input with file upload support

#### UI Components
- `model-selector.tsx`: AI model selection interface
- `visibility-selector.tsx`: Chat visibility controls
- `sidebar.tsx`: Navigation sidebar
- `ui/`: Reusable UI components (buttons, inputs, etc.)

## Environment Configuration

### Required Environment Variables
```env
AUTH_SECRET=your_auth_secret
POSTGRES_URL=your_postgres_connection_string
BLOB_READ_WRITE_TOKEN=your_blob_token
XAI_API_KEY=your_xai_api_key
```

### Database Configuration
- Uses Neon PostgreSQL for data storage
- Drizzle ORM for database operations
- Schema includes tables for:
  - Users
  - Chats
  - Messages
  - Documents
  - Suggestions
  - Votes

## Authentication Flow

1. Registration (`app/(auth)/actions.ts`):
   - Validates email/password
   - Checks for existing users
   - Creates new user with hashed password
   - Automatically signs in after registration

2. Login:
   - Uses NextAuth with credentials provider
   - Verifies password against hashed DB value
   - Creates authenticated session

## Chat Implementation

### Message Flow
1. User sends message through `MultimodalInput`
2. Message processed by chat API route
3. AI response generated using xAI SDK
4. Response streamed back to client
5. Messages saved to database

### File Handling
- Uses Vercel Blob for file storage
- Supports file uploads in chat
- Processes attachments with AI model

## Development Commands

```bash
# Database Operations
pnpm run db:generate  # Generate migrations
pnpm run db:migrate   # Run migrations
pnpm run db:studio    # Open Drizzle Studio

# Development
pnpm run dev         # Start development server
pnpm run build       # Build for production
pnpm run lint        # Run linting
```

## API Routes

### Chat API (`app/(chat)/api/`)
- `/chat/route.ts`: Main chat endpoint
- `/files/upload/route.ts`: File upload handling
- `/vote/route.ts`: Message voting system

## Customization

### Adding New Models
1. Update `lib/ai/models.ts`
2. Implement model interface in `lib/ai/providers`
3. Add to model selector component

### Styling
- Uses Tailwind CSS for styling
- shadcn/ui components for UI elements
- Dark/light mode support

## Security Considerations

1. Environment Variables:
   - Keep sensitive keys in `.env`
   - Never commit secrets to repository

2. Authentication:
   - Password hashing with bcrypt
   - Secure session handling
   - CSRF protection

3. Database:
   - Use parameterized queries
   - Validate user permissions
   - Regular backups recommended

## Troubleshooting

### Common Issues
1. Database Connection:
   - Verify POSTGRES_URL in .env
   - Check database migrations
   - Run `db:migrate` if needed

2. Authentication:
   - Ensure AUTH_SECRET is set
   - Check user table exists
   - Verify password hashing

3. Chat Functionality:
   - Validate XAI_API_KEY
   - Check WebSocket connection
   - Monitor server logs

