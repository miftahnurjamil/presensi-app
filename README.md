# Frontend README

## Setup Instructions

### 1. Install Dependencies

```bash
npm install
```

### 2. Start Development Server

```bash
npm run dev
```

Frontend will run on `http://localhost:3000` with API proxy to `http://localhost:5000`

### 3. Build for Production

```bash
npm run build
```

Output goes to `dist/` directory

## Project Structure

```
frontend/
├── src/
│   ├── components/     - Reusable UI components
│   ├── pages/          - Page components
│   ├── services/       - API service layer
│   ├── context/        - React Context for state
│   ├── hooks/          - Custom React hooks
│   ├── App.jsx         - Main app component
│   ├── main.jsx        - Entry point
│   └── index.css       - Global styles
├── index.html
├── vite.config.js      - Vite configuration
├── package.json
└── .gitignore
```

## Components

### Navbar.jsx

Navigation bar with user info and logout button.

- Displays username and role
- Logout functionality
- Shows only when user is logged in

### Alert.jsx

Reusable alert component for notifications.

- Types: info, success, error, warning
- Dismissible with close button
- Auto-styled based on type

### QRScanner.jsx

QR code scanner component using html5-qrcode.

- Real-time camera feed
- Automatically extracts NPM from QR code
- Returns NPM to parent component
- Can be toggled on/off

## Pages

### LoginPage.jsx

Authentication page for both lecturers and students.

- Role selection (Lecturer/Student)
- Username and password input
- Error handling
- Demo credentials display

### LecturerDashboard.jsx

Main interface for lecturers.

- Course selection
- Start/End session
- Real-time QR scanning
- Live attendance display

### StudentDashboard.jsx

Main interface for students.

- Profile with QR code display
- Enrolled courses
- Attendance records
- Attendance statistics

## Services

### api.js

Centralized API service with axios.

- Base URL configuration
- JWT token injection
- Request/response interceptors
- All API methods:
  - authService
  - lecturerService
  - studentService
  - attendanceService
  - courseService

## Context & Hooks

### AuthContext.jsx

Manages authentication state.

- User state
- Login/Register/Logout methods
- Token management (localStorage)
- Loading state

### useAuth.js

Custom hook to access auth context.

```javascript
const { user, login, logout, loading } = useAuth();
```

## Styling

Global styles in `index.css`:

- CSS Variables
- Common utilities (.container, .card, .btn-\*)
- Alert styles
- Loading spinner animation
- Responsive grid system

Component-specific CSS:

- Each component has its own .css file
- BEM naming convention
- Mobile responsive design

## Features Implementation

### Authentication

1. User enters credentials on login page
2. Frontend calls `/api/auth/login`
3. Backend returns JWT token and user info
4. Token stored in localStorage
5. Token added to all subsequent API requests

### QR Scanning

1. Lecturer clicks "Start Session"
2. Scanner component mounts and accesses camera
3. Scanner detects QR code
4. Extracts NPM from QR code string
5. Sends NPM to backend via `/api/attendance/scan-qr`
6. Attendance recorded and displayed

### Protected Routes

- Routes use `ProtectedRoute` component
- Checks user authentication
- Validates user role
- Redirects unauthorized users

## API Configuration

Proxy configured in `vite.config.js`:

```javascript
proxy: {
  '/api': {
    target: 'http://localhost:5000',
    changeOrigin: true,
  }
}
```

This allows frontend to call `/api/*` endpoints.

## Development Tips

### Add New Page

1. Create component in `src/pages/`
2. Add route in `App.jsx`
3. Implement component UI
4. Use API services for data

### Add New Component

1. Create in `src/components/`
2. Create corresponding .css file
3. Export from component
4. Import in page/parent component

### Add New API Service

1. Add method to appropriate service in `api.js`
2. Call service from component via useEffect/event handler
3. Handle loading and error states

### Authentication Flow

```
Login Page
    ↓
AuthContext.login()
    ↓
API /api/auth/login
    ↓
Save token to localStorage
    ↓
Redirect to dashboard
    ↓
useAuth() provides user state
```

## Browser Compatibility

- Chrome/Edge (latest)
- Firefox (latest)
- Safari 12+
- Mobile browsers (with camera access)

## Performance Tips

1. **Code Splitting**: Vite automatically chunks routes
2. **Lazy Loading**: Use React.lazy() for page components
3. **Image Optimization**: Compress images before adding
4. **API Caching**: Cache course list in state
5. **Debounce Search**: Add debounce for search inputs

## Building & Deployment

### Development Build

```bash
npm run dev
```

### Production Build

```bash
npm run build
npm run preview
```

### Environment Variables

Create `.env.local` file:

```
VITE_API_URL=https://api.example.com
VITE_APP_NAME=Attendance System
```

Access in code:

```javascript
const apiUrl = import.meta.env.VITE_API_URL;
```

## Troubleshooting

### QR Scanner Not Working

- Check camera permissions
- Try different browser
- Ensure HTTPS in production

### API Calls Failing

- Verify backend is running
- Check token in localStorage
- Check network tab in DevTools

### Styles Not Loading

- Clear browser cache
- Restart dev server
- Check CSS file imports

### State Not Updating

- Check useEffect dependencies
- Verify useState syntax
- Use React DevTools for debugging

## Resources

- [React Documentation](https://react.dev)
- [Vite Guide](https://vitejs.dev)
- [React Router](https://reactrouter.com)
- [Axios Docs](https://axios-http.com)
- [html5-qrcode](https://scanapp.org)
