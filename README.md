# Professional Bug Tracker

A Jira-like professional bug tracking and project management system built with modern technologies.

## 🎯 Overview

Bug Tracker is a comprehensive, enterprise-grade issue and bug tracking system. It enables teams to:
- Create and manage projects
- Track bugs and issues with detailed descriptions
- Assign bugs to team members
- Collaborate through comments
- Attach files and screenshots
- Filter and search issues by status, priority, and project

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Frontend (React)                         │
│              Tailwind CSS + React Router                    │
└────────────────────────┬────────────────────────────────────┘
                         │ (Vite Dev Server)
                         │ Port 3000
                         │
┌────────────────────────▼────────────────────────────────────┐
│                  Backend (Express.js)                       │
│           Node.js REST API + JWT Authentication            │
└────────────────────────┬────────────────────────────────────┘
                         │
                         │ Port 5000
                         │
┌────────────────────────▼────────────────────────────────────┐
│              Supabase (PostgreSQL)                          │
│         Cloud Database + Authentication                    │
└─────────────────────────────────────────────────────────────┘
```

## 🚀 Tech Stack

### Frontend
- **React** 18.2.0 - UI Framework
- **React Router** 6.18.0 - Client routing
- **Tailwind CSS** 3.3.5 - Utility-first CSS
- **Zustand** 4.4.1 - State management
- **Axios** 1.6.2 - HTTP client
- **React Hot Toast** - Notifications
- **Vite** - Build tool

### Backend
- **Node.js** - Runtime
- **Express.js** 4.18.2 - REST API framework
- **Supabase JS** 2.38.0 - Database client
- **JWT** 9.1.2 - Authentication
- **bcryptjs** 2.4.3 - Password hashing
- **Helmet** 7.1.0 - Security headers
- **CORS** 2.8.5 - Cross-origin handling
- **Multer** 1.4.5 - File uploads

### Database
- **Supabase** (PostgreSQL) - Database & Auth
- **UUID** - Unique identifiers
- **Row Level Security** - Data protection

## 📁 Project Structure

```
BUG TRACKER/
├── backend/
│   ├── config/
│   │   └── supabase.js
│   ├── routes/
│   │   ├── authRoutes.js
│   │   ├── projectRoutes.js
│   │   ├── bugRoutes.js
│   │   ├── userRoutes.js
│   │   ├── commentRoutes.js
│   │   └── attachmentRoutes.js
│   ├── middleware/
│   │   └── errorHandler.js
│   ├── utils/
│   │   └── auth.js
│   ├── server.js
│   ├── package.json
│   └── .env
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Navbar.jsx
│   │   │   ├── Sidebar.jsx
│   │   │   └── Alert.jsx
│   │   ├── pages/
│   │   │   ├── LoginPage.jsx
│   │   │   ├── RegisterPage.jsx
│   │   │   ├── Dashboard.jsx
│   │   │   ├── ProjectsPage.jsx
│   │   │   ├── BugCreatePage.jsx
│   │   │   └── BugDetailPage.jsx
│   │   ├── services/
│   │   │   ├── api.js
│   │   │   ├── authService.js
│   │   │   ├── bugService.js
│   │   │   └── projectService.js
│   │   ├── store/
│   │   │   └── index.js
│   │   ├── App.jsx
│   │   ├── main.jsx
│   │   └── index.css
│   ├── index.html
│   ├── vite.config.js
│   ├── tailwind.config.js
│   └── package.json
│
├── database/
│   ├── schema.sql
│   └── README.md
│
└── docs/
    ├── SETUP_GUIDE.md
    └── API_DOCS.md
```

## 🔑 Key Features

### Authentication & Authorization
- ✅ User registration and login
- ✅ JWT token-based authentication
- ✅ Password hashing with bcrypt
- ✅ Role-based access control

### Project Management
- ✅ Create and manage projects
- ✅ Organize bugs by project
- ✅ Project-specific filtering

### Bug Tracking
- ✅ Create detailed bug reports
- ✅ Set priority levels (Low, Medium, High, Critical)
- ✅ Track status (Open, In Progress, Resolved, Closed)
- ✅ Assign bugs to team members
- ✅ View detailed bug information

### Collaboration
- ✅ Add comments to bugs
- ✅ Discuss issues with team
- ✅ Real-time comment updates
- ✅ File attachments (screenshots, logs)

### Kanban Board ⭐ NEW
- ✅ Drag-and-drop ticket management
- ✅ Three workflow columns: To Do, In Progress, Done
- ✅ Real-time status updates
- ✅ Optimistic UI updates with error recovery
- ✅ Project-based board switching
- ✅ Assignee information with avatars
- ✅ Priority color indicators
- ✅ Quick view/delete actions

### Search & Filter
- ✅ Filter by project
- ✅ Filter by status
- ✅ Filter by priority
- ✅ Search by keywords

### UI/UX
- ✅ Clean, professional design
- ✅ Responsive layout (mobile-friendly)
- ✅ Dark mode ready
- ✅ Smooth animations
- ✅ Toast notifications
- ✅ Professional dashboard with charts
- ✅ Kanban board view

## 📊 Database Schema

### Tables
1. **Users** - User accounts and profiles
2. **Projects** - Project management
3. **Bugs** - Bug/issue records
4. **Comments** - Bug discussions
5. **Attachments** - File storage
6. **Activity Logs** - Audit trail (optional)

See `database/README.md` for detailed schema documentation.

## 🔐 Security Features

- **JWT Authentication** - Secure token-based auth
- **Password Hashing** - bcrypt with salt rounds
- **CORS Protection** - Restricted cross-origin requests
- **Rate Limiting** - 100 requests per 15 minutes
- **Input Validation** - Sanitization on all inputs
- **SQL Injection Prevention** - Parameterized queries
- **XSS Protection** - Helmet.js headers
- **HTTPS Ready** - Production-ready SSL support

## 🚀 Getting Started

### Quick Setup
1. Clone repository
2. Follow [SETUP_GUIDE.md](SETUP_GUIDE.md)
3. Install dependencies for both backend and frontend
4. Configure environment variables
5. Start both servers
6. Access at http://localhost:3000

### Detailed Instructions
See [SETUP_GUIDE.md](SETUP_GUIDE.md) for step-by-step instructions.

### API Documentation
See [API_DOCS.md](docs/API_DOCS.md) for complete API reference.

## 📝 Environment Variables

### Backend (.env)
```
SUPABASE_URL=
SUPABASE_KEY=
SUPABASE_SERVICE_KEY=
JWT_SECRET=
PORT=5000
NODE_ENV=development
CORS_ORIGIN=http://localhost:3000
```

### Frontend (.env)
```
VITE_API_URL=http://localhost:5000/api
VITE_SUPABASE_URL=
VITE_SUPABASE_KEY=
```

## 📈 Performance

- **Frontend**: Optimized with Vite and React
- **Backend**: Express.js with efficient middleware
- **Database**: Indexed queries and RLS policies
- **API**: RESTful design with proper caching headers

## 🧪 Testing

### Manual Testing
- Register new account
- Create project
- Create bug
- Add comments
- Filter and search
- Update bug status

### Automated Testing (Coming Soon)
- Jest unit tests
- Integration tests
- E2E tests with Cypress

## 📱 Responsive Design

- Desktop (1920px+)
- Laptop (1024px - 1920px)
- Tablet (768px - 1024px)
- Mobile (320px - 768px)

## 🔄 Future Enhancements

- [ ] Real-time notifications with Socket.io
- [ ] Kanban board view (Drag & Drop)
- [ ] Advanced filtering and saved filters
- [ ] Bulk operations
- [ ] Email notifications
- [ ] Activity timeline
- [ ] Integration with GitHub/GitLab
- [ ] Two-factor authentication
- [ ] Dark mode toggle
- [ ] Export reports (PDF, CSV)

## 🤝 Contributing

1. Create a feature branch
2. Make changes
3. Test thoroughly
4. Submit pull request

## 📄 License

MIT License - feel free to use for personal and commercial projects.

## 💬 Support

- Check the README files in backend and frontend
- Review API documentation
- Check Supabase documentation
- Open an issue for bugs

## 👨‍💼 Project by

Created as a professional, production-ready Bug Tracker system similar to Jira.

---

**Built with ❤️ for professional teams**
#   B u g - T r a c k e r 1  
 