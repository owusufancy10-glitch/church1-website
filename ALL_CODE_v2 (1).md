# Church Website Project — Full Source Code (v2 / latest)

## `./.bolt/config.json`

```json
{
  "template": "bolt-vite-react-ts"
}

```

## `./eslint.config.js`

```js
import js from '@eslint/js';
import globals from 'globals';
import reactHooks from 'eslint-plugin-react-hooks';
import reactRefresh from 'eslint-plugin-react-refresh';
import tseslint from 'typescript-eslint';

export default tseslint.config(
  { ignores: ['dist'] },
  {
    extends: [js.configs.recommended, ...tseslint.configs.recommended],
    files: ['**/*.{ts,tsx}'],
    languageOptions: {
      ecmaVersion: 2020,
      globals: globals.browser,
    },
    plugins: {
      'react-hooks': reactHooks,
      'react-refresh': reactRefresh,
    },
    rules: {
      ...reactHooks.configs.recommended.rules,
      'react-refresh/only-export-components': [
        'warn',
        { allowConstantExport: true },
      ],
    },
  }
);

```

## `./index.html`

```html
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/vite.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Defence Word Ministry - God of Possibilities</title>
    <meta name="description" content="Defence Word Ministry International Ghana - A place of worship, fellowship, and spiritual growth. God of Possibilities." />
    <meta name="theme-color" content="#1e40af" />
    <meta property="og:image" content="https://bolt.new/static/og_default.png">
    <meta name="twitter:card" content="summary_large_image">
    <meta name="twitter:image" content="https://bolt.new/static/og_default.png">
</head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.tsx"></script>
  </body>
</html>

```

## `./package.json`

```json
{
  "name": "vite-react-typescript-starter",
  "private": true,
  "version": "0.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "lint": "eslint .",
    "preview": "vite preview",
    "typecheck": "tsc --noEmit -p tsconfig.app.json"
  },
  "dependencies": {
    "@supabase/supabase-js": "^2.57.4",
    "lucide-react": "^0.344.0",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^7.17.0"
  },
  "devDependencies": {
    "@eslint/js": "^9.9.1",
    "@types/react": "^18.3.5",
    "@types/react-dom": "^18.3.0",
    "@vitejs/plugin-react": "^4.3.1",
    "autoprefixer": "^10.4.18",
    "eslint": "^9.9.1",
    "eslint-plugin-react-hooks": "^5.1.0-rc.0",
    "eslint-plugin-react-refresh": "^0.4.11",
    "globals": "^15.9.0",
    "postcss": "^8.4.35",
    "tailwindcss": "^3.4.1",
    "typescript": "^5.5.3",
    "typescript-eslint": "^8.3.0",
    "vite": "^5.4.2"
  }
}

```

## `./postcss.config.js`

```js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};

```

## `./src/App.tsx`

```tsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Navigate } from 'react-router-dom';
import { AuthProvider, useAuth } from './contexts/AuthContext';
import { ThemeProvider } from './contexts/ThemeContext';

// Layouts
import { PublicLayout } from './components/Layout';
import { AdminLayout } from './components/AdminLayout';

// Public Pages
import { HomePage } from './pages/HomePage';
import { AboutPage } from './pages/AboutPage';
import { ServicesPage } from './pages/ServicesPage';
import { SermonsPage } from './pages/SermonsPage';
import { EventsPage } from './pages/EventsPage';
import { GalleryPage } from './pages/GalleryPage';
import { DonationPage } from './pages/DonationPage';
import { ContactPage } from './pages/ContactPage';
import { RegistrationPage } from './pages/RegistrationPage';

// Admin Pages
import { AdminLoginPage } from './pages/admin/LoginPage';
import { AdminDashboard } from './pages/admin/DashboardPage';
import { MembersPage } from './pages/admin/MembersPage';
import { AttendancePage } from './pages/admin/AttendancePage';
import { EventsAdminPage } from './pages/admin/EventsPage';
import { SettingsPage } from './pages/admin/SettingsPage';
import { SermonsAdminPage } from './pages/admin/SermonsPage';
import { GalleryAdminPage } from './pages/admin/GalleryPage';
import { AnnouncementsAdminPage } from './pages/admin/AnnouncementsPage';
import { AdvertsPage } from './pages/admin/AdvertsPage';
import { DonationsPage } from './pages/admin/DonationsPage';
import { PastorsPage } from './pages/admin/PastorsPage';
import { MessagesPage } from './pages/admin/MessagesPage';
import { ContentPage } from './pages/admin/ContentPage';
import { BackupPage } from './pages/admin/BackupPage';
import { TrashPage } from './pages/admin/TrashPage';

function ProtectedRoute({ children }: { children: React.ReactNode }) {
  const { user, loading } = useAuth();
  if (loading) return <div className="min-h-screen flex items-center justify-center bg-gray-100"><div className="animate-spin rounded-full h-12 w-12 border-4 border-blue-500 border-t-transparent" /></div>;
  if (!user) return <Navigate to="/admin/login" replace />;
  return <>{children}</>;
}

function AppRoutes() {
  return (
    <Routes>
      {/* Public Routes */}
      <Route path="/" element={<PublicLayout><HomePage /></PublicLayout>} />
      <Route path="/about" element={<PublicLayout><AboutPage /></PublicLayout>} />
      <Route path="/services" element={<PublicLayout><ServicesPage /></PublicLayout>} />
      <Route path="/sermons" element={<PublicLayout><SermonsPage /></PublicLayout>} />
      <Route path="/events" element={<PublicLayout><EventsPage /></PublicLayout>} />
      <Route path="/gallery" element={<PublicLayout><GalleryPage /></PublicLayout>} />
      <Route path="/donation" element={<PublicLayout><DonationPage /></PublicLayout>} />
      <Route path="/contact" element={<PublicLayout><ContactPage /></PublicLayout>} />
      <Route path="/register" element={<PublicLayout><RegistrationPage /></PublicLayout>} />

      {/* Admin Routes */}
      <Route path="/admin/login" element={<AdminLoginPage />} />
      <Route path="/admin" element={<ProtectedRoute><AdminLayout><AdminDashboard /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/members" element={<ProtectedRoute><AdminLayout><MembersPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/attendance" element={<ProtectedRoute><AdminLayout><AttendancePage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/events" element={<ProtectedRoute><AdminLayout><EventsAdminPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/sermons" element={<ProtectedRoute><AdminLayout><SermonsAdminPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/gallery" element={<ProtectedRoute><AdminLayout><GalleryAdminPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/announcements" element={<ProtectedRoute><AdminLayout><AnnouncementsAdminPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/adverts" element={<ProtectedRoute><AdminLayout><AdvertsPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/donations" element={<ProtectedRoute><AdminLayout><DonationsPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/pastors" element={<ProtectedRoute><AdminLayout><PastorsPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/messages" element={<ProtectedRoute><AdminLayout><MessagesPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/content" element={<ProtectedRoute><AdminLayout><ContentPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/backup" element={<ProtectedRoute><AdminLayout><BackupPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/trash" element={<ProtectedRoute><AdminLayout><TrashPage /></AdminLayout></ProtectedRoute>} />
      <Route path="/admin/settings" element={<ProtectedRoute><AdminLayout><SettingsPage /></AdminLayout></ProtectedRoute>} />

      <Route path="*" element={<Navigate to="/" replace />} />
    </Routes>
  );
}

function App() {
  return (
    <Router>
      <ThemeProvider>
        <AuthProvider>
          <AppRoutes />
        </AuthProvider>
      </ThemeProvider>
    </Router>
  );
}

export default App;

```

## `./src/components/AdminLayout.tsx`

```tsx
import React, { useState } from 'react';
import { Link, useLocation, useNavigate } from 'react-router-dom';
import { useAuth } from '../contexts/AuthContext';
import { useTheme } from '../contexts/ThemeContext';
import {
  LayoutDashboard,
  Users,
  Calendar,
  Mic,
  Image,
  Settings,
  FileText,
  Heart,
  Bell,
  MessageSquare,
  Database,
  LogOut,
  Menu,
  X,
  Sun,
  Moon,
  ChevronLeft,
  Clock,
  Trash2,
  DollarSign,
  UserCog,
  Megaphone
} from 'lucide-react';

interface AdminLayoutProps {
  children: React.ReactNode;
}

const sidebarLinks = [
  { path: '/admin', label: 'Dashboard', icon: LayoutDashboard, exact: true },
  { path: '/admin/members', label: 'Members', icon: Users },
  { path: '/admin/attendance', label: 'Attendance', icon: Clock },
  { path: '/admin/events', label: 'Events', icon: Calendar },
  { path: '/admin/sermons', label: 'Sermons', icon: Mic },
  { path: '/admin/gallery', label: 'Gallery', icon: Image },
  { path: '/admin/announcements', label: 'Announcements', icon: Megaphone },
  { path: '/admin/adverts', label: 'Adverts', icon: Megaphone },
  { path: '/admin/donations', label: 'Donations', icon: DollarSign },
  { path: '/admin/pastors', label: 'Pastors', icon: UserCog },
  { path: '/admin/messages', label: 'Messages', icon: MessageSquare },
  { path: '/admin/content', label: 'Content', icon: FileText },
  { path: '/admin/backup', label: 'Backup & Recovery', icon: Database },
  { path: '/admin/trash', label: 'Trash', icon: Trash2 },
  { path: '/admin/settings', label: 'Settings', icon: Settings }
];

export function AdminLayout({ children }: AdminLayoutProps) {
  const { user, logout, hasPermission } = useAuth();
  const { darkMode, toggleDarkMode, settings } = useTheme();
  const { pathname } = useLocation();
  const navigate = useNavigate();
  const [sidebarOpen, setSidebarOpen] = useState(true);
  const [mobileSidebarOpen, setMobileSidebarOpen] = useState(false);

  const handleLogout = () => {
    logout();
    navigate('/');
  };

  const isActive = (path: string, exact = false) => {
    if (exact) return pathname === path;
    return pathname.startsWith(path);
  };

  // Filter links based on permissions
  const filteredLinks = sidebarLinks.filter((link) => {
    const permissionMap: Record<string, string> = {
      '/admin/members': 'members',
      '/admin/attendance': 'attendance',
      '/admin/events': 'events',
      '/admin/sermons': 'sermons',
      '/admin/gallery': 'gallery',
      '/admin/announcements': 'announcements',
      '/admin/adverts': 'adverts',
      '/admin/donations': 'donations',
      '/admin/pastors': 'pastors',
      '/admin/messages': 'contact',
      '/admin/content': 'settings',
      '/admin/settings': 'settings',
      '/admin/backup': 'all',
      '/admin/trash': 'all'
    };
    const permission = permissionMap[link.path];
    if (!permission || permission === 'all') return true;
    return hasPermission(permission);
  });

  return (
    <div className={`min-h-screen bg-gray-100 dark:bg-gray-900 transition-colors duration-200`}>
      {/* Mobile Header */}
      <header className="lg:hidden fixed top-0 left-0 right-0 z-50 bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700 h-16 flex items-center justify-between px-4">
        <div className="flex items-center space-x-3">
          <div className="h-8 w-8 rounded-full bg-gradient-to-br from-blue-600 to-green-500 flex items-center justify-center text-white font-bold text-sm">
            D
          </div>
          <span className="font-semibold text-gray-900 dark:text-white">Admin Panel</span>
        </div>
        <button
          onClick={() => setMobileSidebarOpen(!mobileSidebarOpen)}
          className="p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700"
        >
          {mobileSidebarOpen ? <X className="h-6 w-6" /> : <Menu className="h-6 w-6" />}
        </button>
      </header>

      {/* Sidebar */}
      <aside className={`fixed top-0 left-0 z-40 h-full bg-white dark:bg-gray-800 border-r border-gray-200 dark:border-gray-700 transition-all duration-300 ${
        sidebarOpen ? 'w-64' : 'w-20'
      } hidden lg:block`}>
        <div className="h-full flex flex-col">
          {/* Logo */}
          <div className="h-16 flex items-center justify-between px-4 border-b border-gray-200 dark:border-gray-700">
            {sidebarOpen && (
              <div className="flex items-center space-x-3">
                {settings.logo_url ? (
                  <img src={settings.logo_url} alt="" className="h-8 w-8 rounded-full" />
                ) : (
                  <div className="h-8 w-8 rounded-full bg-gradient-to-br from-blue-600 to-green-500 flex items-center justify-center text-white font-bold text-sm">
                    D
                  </div>
                )}
                <span className="font-semibold text-gray-900 dark:text-white">Admin</span>
              </div>
            )}
            <button
              onClick={() => setSidebarOpen(!sidebarOpen)}
              className={`p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors ${!sidebarOpen && 'mx-auto'}`}
            >
              <ChevronLeft className={`h-5 w-5 transition-transform ${!sidebarOpen ? 'rotate-180' : ''}`} />
            </button>
          </div>

          {/* Navigation */}
          <nav className="flex-1 overflow-y-auto py-4 px-3">
            <ul className="space-y-1">
              {filteredLinks.map((link) => (
                <li key={link.path}>
                  <Link
                    to={link.path}
                    className={`flex items-center rounded-lg px-3 py-2.5 text-sm font-medium transition-colors ${
                      isActive(link.path, link.exact)
                        ? 'bg-blue-50 dark:bg-blue-900/20 text-blue-600 dark:text-blue-400'
                        : 'text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700'
                    }`}
                    title={!sidebarOpen ? link.label : undefined}
                  >
                    <link.icon className={`h-5 w-5 flex-shrink-0 ${sidebarOpen ? 'mr-3' : 'mx-auto'}`} />
                    {sidebarOpen && <span>{link.label}</span>}
                  </Link>
                </li>
              ))}
            </ul>
          </nav>

          {/* User section */}
          <div className="border-t border-gray-200 dark:border-gray-700 p-4">
            <div className={`flex items-center ${sidebarOpen ? 'justify-between' : 'justify-center'}`}>
              {sidebarOpen && (
                <div className="flex items-center space-x-3">
                  <div className="h-8 w-8 rounded-full bg-gray-300 dark:bg-gray-600 flex items-center justify-center text-gray-600 dark:text-gray-300 font-medium text-sm">
                    {user?.full_name?.charAt(0) || 'A'}
                  </div>
                  <div className="text-sm">
                    <p className="font-medium text-gray-900 dark:text-white truncate max-w-[120px]">
                      {user?.full_name}
                    </p>
                    <p className="text-xs text-gray-500 capitalize">{user?.role}</p>
                  </div>
                </div>
              )}
              <div className="flex space-x-1">
                <button
                  onClick={toggleDarkMode}
                  className="p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700"
                  title="Toggle theme"
                >
                  {darkMode ? <Sun className="h-4 w-4" /> : <Moon className="h-4 w-4" />}
                </button>
                <button
                  onClick={handleLogout}
                  className="p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700 hover:text-red-500"
                  title="Logout"
                >
                  <LogOut className="h-4 w-4" />
                </button>
              </div>
            </div>
          </div>
        </div>
      </aside>

      {/* Mobile Sidebar Overlay */}
      {mobileSidebarOpen && (
        <div className="lg:hidden fixed inset-0 z-50 bg-black/50" onClick={() => setMobileSidebarOpen(false)}>
          <aside className="w-64 h-full bg-white dark:bg-gray-800 overflow-y-auto">
            <div className="h-16 flex items-center px-4 border-b border-gray-200 dark:border-gray-700">
              <div className="flex items-center space-x-3">
                <div className="h-8 w-8 rounded-full bg-gradient-to-br from-blue-600 to-green-500 flex items-center justify-center text-white font-bold text-sm">
                  D
                </div>
                <span className="font-semibold text-gray-900 dark:text-white">Admin Panel</span>
              </div>
            </div>
            <nav className="py-4 px-3">
              <ul className="space-y-1">
                {filteredLinks.map((link) => (
                  <li key={link.path}>
                    <Link
                      to={link.path}
                      onClick={() => setMobileSidebarOpen(false)}
                      className={`flex items-center rounded-lg px-3 py-2.5 text-sm font-medium transition-colors ${
                        isActive(link.path, link.exact)
                          ? 'bg-blue-50 dark:bg-blue-900/20 text-blue-600 dark:text-blue-400'
                          : 'text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700'
                      }`}
                    >
                      <link.icon className="h-5 w-5 mr-3" />
                      <span>{link.label}</span>
                    </Link>
                  </li>
                ))}
              </ul>
            </nav>
            <div className="border-t border-gray-200 dark:border-gray-700 p-4">
              <div className="flex items-center justify-between mb-4">
                <span className="text-sm text-gray-500">{user?.email}</span>
              </div>
              <div className="flex space-x-2">
                <button
                  onClick={toggleDarkMode}
                  className="flex-1 flex items-center justify-center space-x-2 px-3 py-2 rounded-lg text-gray-500 bg-gray-100 dark:bg-gray-700"
                >
                  {darkMode ? <Sun className="h-4 w-4" /> : <Moon className="h-4 w-4" />}
                </button>
                <button
                  onClick={handleLogout}
                  className="flex-1 flex items-center justify-center space-x-2 px-3 py-2 rounded-lg text-red-500 bg-gray-100 dark:bg-gray-700"
                >
                  <LogOut className="h-4 w-4" />
                  <span className="text-sm">Logout</span>
                </button>
              </div>
            </div>
          </aside>
        </div>
      )}

      {/* Main content */}
      <main className={`transition-all duration-300 ${sidebarOpen ? 'lg:ml-64' : 'lg:ml-20'} pt-16 lg:pt-0`}>
        <div className="p-4 lg:p-8">
          {children}
        </div>
      </main>
    </div>
  );
}

```

## `./src/components/Layout.tsx`

```tsx
import React, { useState } from 'react';
import { Link, useLocation } from 'react-router-dom';
import { useTheme } from '../contexts/ThemeContext';
import {
  Menu,
  X,
  Home,
  Users,
  Info,
  Calendar,
  Mic,
  Image,
  Heart,
  Phone,
  Sun,
  Moon,
  ChevronDown,
  UserPlus
} from 'lucide-react';

interface LayoutProps {
  children: React.ReactNode;
}

export function PublicLayout({ children }: LayoutProps) {
  const { settings, darkMode, toggleDarkMode } = useTheme();
  const [mobileMenuOpen, setMobileMenuOpen] = useState(false);
  const location = useLocation();

  const navLinks = [
    { path: '/', label: 'Home', icon: Home },
    { path: '/about', label: 'About', icon: Info },
    { path: '/services', label: 'Services', icon: Calendar },
    { path: '/sermons', label: 'Sermons', icon: Mic },
    { path: '/events', label: 'Events', icon: Calendar },
    { path: '/gallery', label: 'Gallery', icon: Image },
    { path: '/donation', label: 'Support', icon: Heart },
    { path: '/contact', label: 'Contact', icon: Phone },
    { path: '/register', label: 'Join Us', icon: UserPlus, highlight: true }
  ];

  const isActive = (path: string) => location.pathname === path;

  return (
    <div className={`min-h-screen bg-gray-50 dark:bg-gray-900 transition-colors duration-200`}>
      {/* Header */}
      <header className="fixed top-0 left-0 right-0 z-50 bg-white/95 dark:bg-gray-800/95 backdrop-blur-md shadow-sm">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="flex items-center justify-between h-16 md:h-20">
            {/* Logo */}
            <Link to="/" className="flex items-center space-x-3">
              {settings.logo_url ? (
                <img src={settings.logo_url} alt={settings.church_name} className="h-10 w-10 rounded-full object-cover" />
              ) : (
                <div className="h-10 w-10 rounded-full bg-gradient-to-br from-blue-600 to-green-500 flex items-center justify-center text-white font-bold text-lg">
                  D
                </div>
              )}
              <div className="hidden sm:block">
                <h1 className="text-lg font-bold text-gray-900 dark:text-white">{settings.church_name}</h1>
                <p className="text-xs text-gray-500 dark:text-gray-400">{settings.motto}</p>
              </div>
            </Link>

            {/* Desktop Navigation */}
            <nav className="hidden lg:flex items-center space-x-1">
              {navLinks.map((link) => (
                <Link
                  key={link.path}
                  to={link.path}
                  className={`px-3 py-2 rounded-lg text-sm font-medium transition-all duration-200 ${
                    link.highlight
                      ? 'bg-blue-600 text-white hover:bg-blue-700'
                      : isActive(link.path)
                      ? 'bg-blue-50 dark:bg-blue-900/20 text-blue-600 dark:text-blue-400'
                      : 'text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700 hover:text-gray-900 dark:hover:text-white'
                  }`}
                >
                  {link.label}
                </Link>
              ))}
            </nav>

            {/* Actions */}
            <div className="flex items-center space-x-2">
              <button
                onClick={toggleDarkMode}
                className="p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700 dark:text-gray-400 transition-colors"
              >
                {darkMode ? <Sun className="h-5 w-5" /> : <Moon className="h-5 w-5" />}
              </button>
              <Link
                to="/admin"
                className="hidden md:inline-flex px-4 py-2 text-sm font-medium text-gray-600 dark:text-gray-300 hover:text-gray-900 dark:hover:text-white transition-colors"
              >
                Admin
              </Link>
              <button
                onClick={() => setMobileMenuOpen(!mobileMenuOpen)}
                className="lg:hidden p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700 dark:text-gray-400"
              >
                {mobileMenuOpen ? <X className="h-6 w-6" /> : <Menu className="h-6 w-6" />}
              </button>
            </div>
          </div>
        </div>

        {/* Mobile Menu */}
        {mobileMenuOpen && (
          <div className="lg:hidden bg-white dark:bg-gray-800 border-t border-gray-100 dark:border-gray-700">
            <nav className="px-4 py-3 space-y-1">
              {navLinks.map((link) => (
                <Link
                  key={link.path}
                  to={link.path}
                  onClick={() => setMobileMenuOpen(false)}
                  className={`flex items-center space-x-3 px-3 py-3 rounded-lg font-medium transition-colors ${
                    link.highlight
                      ? 'bg-blue-600 text-white'
                      : isActive(link.path)
                      ? 'bg-blue-50 dark:bg-blue-900/20 text-blue-600 dark:text-blue-400'
                      : 'text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700'
                  }`}
                >
                  <link.icon className="h-5 w-5" />
                  <span>{link.label}</span>
                </Link>
              ))}
              <Link
                to="/admin"
                onClick={() => setMobileMenuOpen(false)}
                className="flex items-center space-x-3 px-3 py-3 rounded-lg font-medium text-gray-600 dark:text-gray-300 hover:bg-gray-100 dark:hover:bg-gray-700"
              >
                <Users className="h-5 w-5" />
                <span>Admin Portal</span>
              </Link>
            </nav>
          </div>
        )}
      </header>

      {/* Main Content */}
      <main className="pt-16 md:pt-20">
        {children}
      </main>

      {/* Footer */}
      <footer className="bg-gray-900 text-white py-12 mt-auto">
        <div className="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
          <div className="grid md:grid-cols-4 gap-8">
            {/* About */}
            <div className="md:col-span-2">
              <div className="flex items-center space-x-3 mb-4">
                {settings.logo_url ? (
                  <img src={settings.logo_url} alt={settings.church_name} className="h-12 w-12 rounded-full" />
                ) : (
                  <div className="h-12 w-12 rounded-full bg-gradient-to-br from-blue-600 to-green-500 flex items-center justify-center text-white font-bold text-xl">
                    D
                  </div>
                )}
                <div>
                  <h3 className="text-xl font-bold">{settings.church_name}</h3>
                  <p className="text-gray-400 text-sm">{settings.motto}</p>
                </div>
              </div>
              <p className="text-gray-400 text-sm max-w-md">
                {settings.welcome_message}
              </p>
            </div>

            {/* Quick Links */}
            <div>
              <h4 className="font-semibold mb-4">Quick Links</h4>
              <ul className="space-y-2 text-gray-400 text-sm">
                <li><Link to="/about" className="hover:text-white transition-colors">About Us</Link></li>
                <li><Link to="/events" className="hover:text-white transition-colors">Events</Link></li>
                <li><Link to="/sermons" className="hover:text-white transition-colors">Sermons</Link></li>
                <li><Link to="/register" className="hover:text-white transition-colors">Become a Member</Link></li>
              </ul>
            </div>

            {/* Contact */}
            <div>
              <h4 className="font-semibold mb-4">Contact Us</h4>
              <ul className="space-y-2 text-gray-400 text-sm">
                {settings.address && <li>{settings.address}</li>}
                {settings.phone && <li>{settings.phone}</li>}
                {settings.email && <li>{settings.email}</li>}
              </ul>
              <div className="flex space-x-3 mt-4">
                {settings.social_links?.facebook && (
                  <a href={settings.social_links.facebook} target="_blank" rel="noopener noreferrer" className="text-gray-400 hover:text-white transition-colors">
                    <svg className="h-5 w-5" fill="currentColor" viewBox="0 0 24 24"><path d="M18.77,7.46H14.5v-1.9c0-.9.6-1.1,1-1.1h3V.5h-4.33C10.24.5,9.5,1.56,9.5,5.07v2.39H7v4h2.5v10h5v-10H18.1l.67-4Z"/></svg>
                  </a>
                )}
                {settings.social_links?.youtube && (
                  <a href={settings.social_links.youtube} target="_blank" rel="noopener noreferrer" className="text-gray-400 hover:text-white transition-colors">
                    <svg className="h-5 w-5" fill="currentColor" viewBox="0 0 24 24"><path d="M19.615,3.184c-0.464-0.058-7.118-0.536-9.615,0.694C7.5,5.1,5.5,7.1,4.878,9.615c-1.23,2.495-0.752,9.151-0.694,9.615c0.058,0.464,0.536,7.118,0.694,9.615c0.622,2.515,2.622,4.515,5.122,5.138c2.497,1.23,9.151,0.752,9.615,0.694c0.464-0.058,7.118-0.536,9.615-0.694c2.515-0.622,4.515-2.622,5.138-5.122c1.23-2.497,0.752-9.151,0.694-9.615c-0.058-0.464-0.536-7.118-0.694-9.615c-0.622-2.515-2.622-4.515-5.122-5.138C22.117,2.954,20.079,3.126,19.615,3.184z M12,16.5c-2.485,0-4.5-2.015-4.5-4.5S9.515,7.5,12,7.5s4.5,2.015,4.5,4.5S14.485,16.5,12,16.5z M16.5,12c0,2.485-2.015,4.5-4.5,4.5S7.5,14.485,7.5,12S9.515,7.5,12,7.5s4.5,2.015,4.5,4.5z"/></svg>
                  </a>
                )}
                {settings.social_links?.instagram && (
                  <a href={settings.social_links.instagram} target="_blank" rel="noopener noreferrer" className="text-gray-400 hover:text-white transition-colors">
                    <svg className="h-5 w-5" fill="currentColor" viewBox="0 0 24 24"><path d="M12,2.163c3.204,0,3.584,0.012,4.85,0.07c3.252,0.148,4.771,1.691,4.919,4.919c0.058,1.265,0.069,1.645,0.069,4.849c0,3.205-0.012,3.584-0.069,4.849c-0.149,3.225-1.664,4.771-4.919,4.919c-1.266,0.058-1.644,0.07-4.85,0.07s-3.584-0.012-4.849-0.07c-3.255-0.148-4.771-1.694-4.919-4.919C2.163,15.584,2.15,15.205,2.15,12s0.012-3.584,0.069-4.849c0.148-3.228,1.664-4.771,4.919-4.919c1.265-0.058,1.645-0.069,4.849-0.069 M12,0C8.741,0,8.333,0.014,7.053,0.072C2.695,0.272,0.272,2.695,0.072,7.053C0.014,8.333,0,8.741,0,12s0.014,3.667,0.072,4.947c0.2,4.358,2.623,6.781,6.981,6.981c1.28,0.058,1.689,0.072,4.947,0.072s3.667-0.014,4.947-0.072c4.358-0.2,6.781-2.623,6.981-6.981c0.058-1.28,0.072-1.689,0.072-4.947s-0.014-3.667-0.072-4.947c-0.2-4.358-2.623-6.781-6.981-6.981C15.333,0.014,14.259,0,12,0z M12,5.838c-3.403,0-6.162,2.759-6.162,6.162s2.759,6.162,6.162,6.162s6.162-2.759,6.162-6.162S15.403,5.838,12,5.838z M12,16c-2.209,0-4-1.791-4-4s1.791-4,4-4s4,1.791,4,4S14.209,16,12,16z M18.406,4.155c-0.796,0-1.441,0.645-1.441,1.441s0.645,1.441,1.441,1.441s1.441-0.645,1.441-1.441S19.202,4.155,18.406,4.155z"/></svg>
                  </a>
                )}
              </div>
            </div>
          </div>

          <div className="border-t border-gray-800 mt-8 pt-8 text-center text-gray-400 text-sm">
            <p>&copy; {new Date().getFullYear()} {settings.church_name}. All rights reserved.</p>
          </div>
        </div>
      </footer>
    </div>
  );
}

```

## `./src/components/ui/index.tsx`

```tsx
import React from 'react';

interface ButtonProps extends React.ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger';
  size?: 'sm' | 'md' | 'lg';
  loading?: boolean;
  children: React.ReactNode;
}

export function Button({
  variant = 'primary',
  size = 'md',
  loading = false,
  children,
  className = '',
  disabled,
  ...props
}: ButtonProps) {
  const baseStyles = 'inline-flex items-center justify-center font-medium rounded-lg transition-all duration-200 focus:outline-none focus:ring-2 focus:ring-offset-2 disabled:opacity-50 disabled:cursor-not-allowed';

  const variants = {
    primary: 'bg-blue-600 text-white hover:bg-blue-700 focus:ring-blue-500',
    secondary: 'bg-gray-600 text-white hover:bg-gray-700 focus:ring-gray-500',
    outline: 'border-2 border-blue-600 text-blue-600 hover:bg-blue-50 focus:ring-blue-500',
    ghost: 'text-gray-600 hover:bg-gray-100 focus:ring-gray-500',
    danger: 'bg-red-600 text-white hover:bg-red-700 focus:ring-red-500'
  };

  const sizes = {
    sm: 'px-3 py-1.5 text-sm',
    md: 'px-4 py-2 text-sm',
    lg: 'px-6 py-3 text-base'
  };

  return (
    <button
      className={`${baseStyles} ${variants[variant]} ${sizes[size]} ${className}`}
      disabled={disabled || loading}
      {...props}
    >
      {loading && (
        <svg className="animate-spin -ml-1 mr-2 h-4 w-4" fill="none" viewBox="0 0 24 24">
          <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" />
          <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z" />
        </svg>
      )}
      {children}
    </button>
  );
}

interface CardProps {
  children: React.ReactNode;
  className?: string;
  onClick?: () => void;
}

export function Card({ children, className = '', onClick }: CardProps) {
  return (
    <div
      className={`bg-white dark:bg-gray-800 rounded-xl shadow-lg overflow-hidden ${onClick ? 'cursor-pointer hover:shadow-xl transition-shadow' : ''} ${className}`}
      onClick={onClick}
    >
      {children}
    </div>
  );
}

interface InputProps extends React.InputHTMLAttributes<HTMLInputElement> {
  label?: string;
  error?: string;
  helpText?: string;
}

export function Input({ label, error, helpText, className = '', ...props }: InputProps) {
  return (
    <div className="space-y-1">
      {label && (
        <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          {label}
        </label>
      )}
      <input
        className={`block w-full rounded-lg border ${error ? 'border-red-500' : 'border-gray-300 dark:border-gray-600'} px-3 py-2 text-gray-900 dark:text-white placeholder-gray-400 focus:outline-none focus:ring-2 ${error ? 'focus:ring-red-500' : 'focus:ring-blue-500'} focus:border-transparent dark:bg-gray-700 ${className}`}
        {...props}
      />
      {helpText && !error && (
        <p className="text-sm text-gray-500">{helpText}</p>
      )}
      {error && (
        <p className="text-sm text-red-500">{error}</p>
      )}
    </div>
  );
}

interface TextAreaProps extends React.TextareaHTMLAttributes<HTMLTextAreaElement> {
  label?: string;
  error?: string;
}

export function TextArea({ label, error, className = '', ...props }: TextAreaProps) {
  return (
    <div className="space-y-1">
      {label && (
        <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          {label}
        </label>
      )}
      <textarea
        className={`block w-full rounded-lg border ${error ? 'border-red-500' : 'border-gray-300 dark:border-gray-600'} px-3 py-2 text-gray-900 dark:text-white placeholder-gray-400 focus:outline-none focus:ring-2 ${error ? 'focus:ring-red-500' : 'focus:ring-blue-500'} focus:border-transparent dark:bg-gray-700 ${className}`}
        {...props}
      />
      {error && <p className="text-sm text-red-500">{error}</p>}
    </div>
  );
}

interface SelectProps extends React.SelectHTMLAttributes<HTMLSelectElement> {
  label?: string;
  error?: string;
  options: { value: string; label: string }[];
}

export function Select({ label, error, options, className = '', ...props }: SelectProps) {
  return (
    <div className="space-y-1">
      {label && (
        <label className="block text-sm font-medium text-gray-700 dark:text-gray-300">
          {label}
        </label>
      )}
      <select
        className={`block w-full rounded-lg border ${error ? 'border-red-500' : 'border-gray-300 dark:border-gray-600'} px-3 py-2 text-gray-900 dark:text-white focus:outline-none focus:ring-2 ${error ? 'focus:ring-red-500' : 'focus:ring-blue-500'} focus:border-transparent dark:bg-gray-700 ${className}`}
        {...props}
      >
        {options.map((opt) => (
          <option key={opt.value} value={opt.value}>
            {opt.label}
          </option>
        ))}
      </select>
      {error && <p className="text-sm text-red-500">{error}</p>}
    </div>
  );
}

interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title?: string;
  children: React.ReactNode;
  size?: 'sm' | 'md' | 'lg' | 'xl' | 'full';
}

export function Modal({ isOpen, onClose, title, children, size = 'md' }: ModalProps) {
  if (!isOpen) return null;

  const sizes = {
    sm: 'max-w-sm',
    md: 'max-w-md',
    lg: 'max-w-lg',
    xl: 'max-w-4xl',
    full: 'max-w-full mx-4'
  };

  return (
    <div className="fixed inset-0 z-50 overflow-y-auto">
      <div className="flex min-h-screen items-center justify-center p-4">
        <div className="fixed inset-0 bg-black/50 transition-opacity" onClick={onClose} />
        <div className={`relative w-full ${sizes[size]} transform rounded-xl bg-white dark:bg-gray-800 shadow-2xl transition-all`}>
          {title && (
            <div className="flex items-center justify-between border-b border-gray-200 dark:border-gray-700 px-6 py-4">
              <h3 className="text-lg font-semibold text-gray-900 dark:text-white">{title}</h3>
              <button
                onClick={onClose}
                className="rounded-lg p-1 text-gray-400 hover:bg-gray-100 dark:hover:bg-gray-700 hover:text-gray-500"
              >
                <svg className="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                  <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" />
                </svg>
              </button>
            </div>
          )}
          <div className="p-6">{children}</div>
        </div>
      </div>
    </div>
  );
}

interface BadgeProps {
  children: React.ReactNode;
  variant?: 'default' | 'success' | 'warning' | 'error' | 'info';
  className?: string;
}

export function Badge({ children, variant = 'default', className = '' }: BadgeProps) {
  const variants = {
    default: 'bg-gray-100 text-gray-800 dark:bg-gray-700 dark:text-gray-300',
    success: 'bg-green-100 text-green-800 dark:bg-green-900 dark:text-green-300',
    warning: 'bg-yellow-100 text-yellow-800 dark:bg-yellow-900 dark:text-yellow-300',
    error: 'bg-red-100 text-red-800 dark:bg-red-900 dark:text-red-300',
    info: 'bg-blue-100 text-blue-800 dark:bg-blue-900 dark:text-blue-300'
  };

  return (
    <span className={`inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium ${variants[variant]} ${className}`}>
      {children}
    </span>
  );
}

interface LoadingSpinnerProps {
  size?: 'sm' | 'md' | 'lg';
  className?: string;
}

export function LoadingSpinner({ size = 'md', className = '' }: LoadingSpinnerProps) {
  const sizes = {
    sm: 'h-4 w-4',
    md: 'h-8 w-8',
    lg: 'h-12 w-12'
  };

  return (
    <div className={`flex items-center justify-center ${className}`}>
      <svg
        className={`animate-spin ${sizes[size]} text-blue-600`}
        fill="none"
        viewBox="0 0 24 24"
      >
        <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4" />
        <path className="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z" />
      </svg>
    </div>
  );
}

interface EmptyStateProps {
  icon?: React.ReactNode;
  title: string;
  description?: string;
  action?: React.ReactNode;
}

export function EmptyState({ icon, title, description, action }: EmptyStateProps) {
  return (
    <div className="flex flex-col items-center justify-center py-12 text-center">
      {icon && <div className="mb-4 text-gray-400">{icon}</div>}
      <h3 className="text-lg font-medium text-gray-900 dark:text-white">{title}</h3>
      {description && (
        <p className="mt-1 text-sm text-gray-500 dark:text-gray-400">{description}</p>
      )}
      {action && <div className="mt-4">{action}</div>}
    </div>
  );
}

interface TabsProps {
  tabs: { id: string; label: string; count?: number }[];
  activeTab: string;
  onChange: (tabId: string) => void;
}

export function Tabs({ tabs, activeTab, onChange }: TabsProps) {
  return (
    <div className="border-b border-gray-200 dark:border-gray-700">
      <nav className="flex space-x-8" aria-label="Tabs">
        {tabs.map((tab) => (
          <button
            key={tab.id}
            onClick={() => onChange(tab.id)}
            className={`whitespace-nowrap py-4 px-1 border-b-2 font-medium text-sm transition-colors ${
              activeTab === tab.id
                ? 'border-blue-500 text-blue-600 dark:text-blue-400'
                : 'border-transparent text-gray-500 hover:text-gray-700 hover:border-gray-300 dark:text-gray-400 dark:hover:text-gray-300'
            }`}
          >
            {tab.label}
            {tab.count !== undefined && (
              <span className={`ml-2 rounded-full px-2 py-0.5 text-xs ${
                activeTab === tab.id ? 'bg-blue-100 text-blue-600 dark:bg-blue-900 dark:text-blue-300' : 'bg-gray-100 text-gray-600 dark:bg-gray-700 dark:text-gray-400'
              }`}>
                {tab.count}
              </span>
            )}
          </button>
        ))}
      </nav>
    </div>
  );
}

interface DataTableProps<T> {
  data: T[];
  columns: {
    key: keyof T | string;
    header: string;
    render?: (item: T) => React.ReactNode;
    className?: string;
  }[];
  onRowClick?: (item: T) => void;
  emptyMessage?: string;
}

export function DataTable<T extends { id: string }>({
  data,
  columns,
  onRowClick,
  emptyMessage = 'No data available'
}: DataTableProps<T>) {
  if (data.length === 0) {
    return (
      <div className="text-center py-12 text-gray-500">
        {emptyMessage}
      </div>
    );
  }

  const getValue = (item: T, key: keyof T | string): React.ReactNode => {
    if (typeof key === 'string' && key.includes('.')) {
      const [first, ...rest] = key.split('.');
      let value = item[first as keyof T];
      for (const k of rest) {
        if (value && typeof value === 'object') {
          value = (value as Record<string, unknown>)[k];
        }
      }
      return value as React.ReactNode;
    }
    return item[key as keyof T] as React.ReactNode;
  };

  return (
    <div className="overflow-x-auto">
      <table className="min-w-full divide-y divide-gray-200 dark:divide-gray-700">
        <thead className="bg-gray-50 dark:bg-gray-800">
          <tr>
            {columns.map((col) => (
              <th
                key={String(col.key)}
                scope="col"
                className={`px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase tracking-wider ${col.className || ''}`}
              >
                {col.header}
              </th>
            ))}
          </tr>
        </thead>
        <tbody className="bg-white dark:bg-gray-900 divide-y divide-gray-200 dark:divide-gray-700">
          {data.map((item) => (
            <tr
              key={item.id}
              onClick={() => onRowClick?.(item)}
              className={`${onRowClick ? 'cursor-pointer hover:bg-gray-50 dark:hover:bg-gray-800' : ''} transition-colors`}
            >
              {columns.map((col) => (
                <td
                  key={String(col.key)}
                  className={`px-6 py-4 whitespace-nowrap text-sm text-gray-900 dark:text-white ${col.className || ''}`}
                >
                  {col.render ? col.render(item) : getValue(item, col.key)}
                </td>
              ))}
            </tr>
          ))}
        </tbody>
      </table>
    </div>
  );
}

interface FileUploadProps {
  onFileSelect: (file: File) => void;
  accept?: string;
  preview?: string;
  label?: string;
  className?: string;
}

export function FileUpload({ onFileSelect, accept = 'image/*', preview, label = 'Upload Image', className = '' }: FileUploadProps) {
  const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
    const file = e.target.files?.[0];
    if (file) {
      onFileSelect(file);
    }
  };

  return (
    <div className={className}>
      <label className="block cursor-pointer">
        <div className={`flex flex-col items-center justify-center w-full h-40 border-2 border-dashed rounded-lg transition-colors ${
          preview ? 'border-green-500 bg-green-50 dark:bg-green-900/20' : 'border-gray-300 dark:border-gray-600 hover:border-blue-500 hover:bg-blue-50 dark:hover:bg-blue-900/20'
        }`}>
          {preview ? (
            <img src={preview} alt="Preview" className="h-full w-full object-contain rounded-lg" />
          ) : (
            <div className="text-center">
              <svg className="mx-auto h-12 w-12 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path strokeLinecap="round" strokeLinejoin="round" strokeWidth={1.5} d="M4 16l4.586-4.586a2 2 0 012.828 0L16 16m-2-2l1.586-1.586a2 2 0 012.828 0L20 14m-6-6h.01M6 20h12a2 2 0 002-2V6a2 2 0 00-2-2H6a2 2 0 00-2 2v12a2 2 0 002 2z" />
              </svg>
              <p className="mt-1 text-sm text-gray-600 dark:text-gray-400">{label}</p>
              <p className="text-xs text-gray-500">Click or drag to upload</p>
            </div>
          )}
        </div>
        <input type="file" accept={accept} className="hidden" onChange={handleChange} />
      </label>
    </div>
  );
}

```

## `./src/contexts/AuthContext.tsx`

```tsx
import React, { createContext, useContext, useState, useEffect, useCallback, useMemo } from 'react';
import { AdminUser } from '../lib/supabase';

interface AuthContextType {
  user: AdminUser | null;
  loading: boolean;
  login: (password: string) => Promise<{ success: boolean; error?: string }>;
  logout: () => void;
  hasPermission: (permission: string) => boolean;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

const SESSION_KEY = 'dwm_admin_session';
const ADMIN_PASSWORD = 'admin123';

const ROLE_PERMISSIONS: Record<string, string[]> = {
  admin: ['all'],
  pastor: ['members', 'attendance', 'sermons', 'events', 'announcements', 'gallery', 'settings', 'reports', 'pastors'],
  secretary: ['members', 'attendance', 'events', 'announcements', 'contact', 'donations'],
  media: ['gallery', 'sermons', 'announcements', 'events', 'adverts']
};

const DEFAULT_ADMIN: AdminUser = {
  id: 'default-admin',
  email: 'admin@defencewordministry.org',
  full_name: 'Admin User',
  role: 'admin',
  two_factor_enabled: false,
  active: true,
  created_at: new Date().toISOString()
};

export function AuthProvider({ children }: { children: React.ReactNode }) {
  const [user, setUser] = useState<AdminUser | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const savedSession = localStorage.getItem(SESSION_KEY);
    if (savedSession) {
      try {
        const session = JSON.parse(savedSession);
        if (session.expires > Date.now()) {
          setUser(session.user);
        } else {
          localStorage.removeItem(SESSION_KEY);
        }
      } catch {
        localStorage.removeItem(SESSION_KEY);
      }
    }
    setLoading(false);
  }, []);

  const saveSession = useCallback((userData: AdminUser) => {
    const session = {
      user: userData,
      expires: Date.now() + 24 * 60 * 60 * 1000 // 24 hours
    };
    localStorage.setItem(SESSION_KEY, JSON.stringify(session));
  }, []);

  const login = useCallback(async (password: string) => {
    setLoading(true);

    // Simple password check
    if (password === ADMIN_PASSWORD) {
      setUser(DEFAULT_ADMIN);
      saveSession(DEFAULT_ADMIN);
      setLoading(false);
      return { success: true };
    }

    setLoading(false);
    return { success: false, error: 'Invalid password' };
  }, [saveSession]);

  const logout = useCallback(() => {
    setUser(null);
    localStorage.removeItem(SESSION_KEY);
    window.location.href = '/';
  }, []);

  const hasPermission = useCallback((permission: string): boolean => {
    if (!user) return false;
    const permissions = ROLE_PERMISSIONS[user.role] || [];
    return permissions.includes('all') || permissions.includes(permission);
  }, [user]);

  const value = useMemo(() => ({
    user,
    loading,
    login,
    logout,
    hasPermission
  }), [user, loading, login, logout, hasPermission]);

  return (
    <AuthContext.Provider value={value}>
      {children}
    </AuthContext.Provider>
  );
}

export function useAuth() {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
}

```

## `./src/contexts/ThemeContext.tsx`

```tsx
import React, { createContext, useContext, useState, useEffect, useMemo } from 'react';
import { getChurchSettings, ChurchSettings } from '../lib/supabase';

interface ThemeContextType {
  settings: ChurchSettings | null;
  darkMode: boolean;
  loading: boolean;
  refreshSettings: () => Promise<void>;
  toggleDarkMode: () => void;
}

const ThemeContext = createContext<ThemeContextType | undefined>(undefined);

const DEFAULT_SETTINGS: ChurchSettings = {
  id: '',
  church_name: 'Defence Word Ministry',
  motto: 'God of Possibilities',
  slogan: 'God of Possibilities',
  primary_color: '#1e40af',
  secondary_color: '#ffffff',
  accent_color: '#16a34a',
  logo_url: null,
  address: '',
  phone: '',
  email: '',
  website: '',
  service_times: [
    { day: 'Sunday', time: '8:00 AM', name: 'First Service' },
    { day: 'Sunday', time: '10:30 AM', name: 'Second Service' },
    { day: 'Wednesday', time: '6:00 PM', name: 'Midweek Service' },
    { day: 'Friday', time: '6:00 PM', name: 'Prayer Service' }
  ],
  theme_of_year: '',
  verse_of_year: '',
  welcome_message: 'Welcome to Defence Word Ministries International Ghana',
  social_links: {},
  created_at: '',
  updated_at: ''
};

export function ThemeProvider({ children }: { children: React.ReactNode }) {
  const [settings, setSettings] = useState<ChurchSettings | null>(null);
  const [darkMode, setDarkMode] = useState(() => {
    const saved = localStorage.getItem('dwm_dark_mode');
    return saved === 'true';
  });
  const [loading, setLoading] = useState(true);

  const refreshSettings = async () => {
    const data = await getChurchSettings();
    setSettings(data || DEFAULT_SETTINGS);
  };

  useEffect(() => {
    refreshSettings().finally(() => setLoading(false));
  }, []);

  useEffect(() => {
    localStorage.setItem('dwm_dark_mode', String(darkMode));
    if (darkMode) {
      document.documentElement.classList.add('dark');
    } else {
      document.documentElement.classList.remove('dark');
    }
  }, [darkMode]);

  const toggleDarkMode = () => setDarkMode(!darkMode);

  useEffect(() => {
    if (settings) {
      document.documentElement.style.setProperty('--color-primary', settings.primary_color);
      document.documentElement.style.setProperty('--color-secondary', settings.secondary_color);
      document.documentElement.style.setProperty('--color-accent', settings.accent_color);
    }
  }, [settings]);

  const value = useMemo(() => ({
    settings: settings || DEFAULT_SETTINGS,
    darkMode,
    loading,
    refreshSettings,
    toggleDarkMode
  }), [settings, darkMode, loading]);

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

export function useTheme() {
  const context = useContext(ThemeContext);
  if (context === undefined) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
}

```

## `./src/hooks/useData.ts`

```ts
import { useState, useEffect, useCallback } from 'react';
import { supabase } from '../lib/supabase';

export function useDataFetcher<T>(
  fetcher: () => Promise<T[]>,
  deps: React.DependencyList = []
) {
  const [data, setData] = useState<T[]>([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    try {
      const result = await fetcher();
      setData(result);
    } catch (err) {
      setError(err instanceof Error ? err.message : 'An error occurred');
    } finally {
      setLoading(false);
    }
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, deps);

  useEffect(() => {
    fetchData();
  }, [fetchData]);

  return { data, loading, error, refetch: fetchData };
}

export function useChurchSettings() {
  const [settings, setSettings] = useState<Record<string, unknown> | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    supabase
      .from('church_settings')
      .select('*')
      .limit(1)
      .single()
      .then(({ data, error }) => {
        if (!error && data) {
          setSettings(data);
        }
        setLoading(false);
      });
  }, []);

  const updateSettings = async (updates: Record<string, unknown>) => {
    if (!settings?.id) return false;

    const { error } = await supabase
      .from('church_settings')
      .update({ ...updates, updated_at: new Date().toISOString() })
      .eq('id', settings.id as string);

    if (!error) {
      setSettings(prev => prev ? { ...prev, ...updates } : null);
      return true;
    }
    return false;
  };

  return { settings, loading, updateSettings };
}

export function useStats() {
  const [stats, setStats] = useState({
    totalMembers: 0,
    approvedMembers: 0,
    pendingMembers: 0,
    totalEvents: 0,
    upcomingEvents: 0,
    todayAttendance: 0,
    totalDonations: 0
  });
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchStats() {
      const [
        { count: totalMembers },
        { count: approvedMembers },
        { count: pendingMembers },
        { data: todayAttendance }
      ] = await Promise.all([
        supabase.from('members').select('id', { count: 'exact', head: true }),
        supabase.from('members').select('id', { count: 'exact', head: true }).eq('status', 'approved'),
        supabase.from('members').select('id', { count: 'exact', head: true }).eq('status', 'pending'),
        supabase.from('attendance').select('id').eq('service_date', new Date().toISOString().split('T')[0])
      ]);

      setStats({
        totalMembers: totalMembers || 0,
        approvedMembers: approvedMembers || 0,
        pendingMembers: pendingMembers || 0,
        totalEvents: 0,
        upcomingEvents: 0,
        todayAttendance: todayAttendance?.length || 0,
        totalDonations: 0
      });
      setLoading(false);
    }
    fetchStats();
  }, []);

  return { stats, loading };
}

// Generic CRUD operations
export function useCRUD<T extends { id: string }>(tableName: string) {
  const create = async (data: Omit<T, 'id'>): Promise<T | null> => {
    const { data: result, error } = await supabase
      .from(tableName)
      .insert(data as Record<string, unknown>)
      .select()
      .single();

    if (error) {
      console.error(`Error creating ${tableName}:`, error);
      return null;
    }
    return result as T;
  };

  const update = async (id: string, data: Partial<T>): Promise<T | null> => {
    const { data: result, error } = await supabase
      .from(tableName)
      .update(data as Record<string, unknown>)
      .eq('id', id)
      .select()
      .single();

    if (error) {
      console.error(`Error updating ${tableName}:`, error);
      return null;
    }
    return result as T;
  };

  const remove = async (id: string): Promise<boolean> => {
    const { error } = await supabase
      .from(tableName)
      .delete()
      .eq('id', id);

    if (error) {
      console.error(`Error deleting ${tableName}:`, error);
      return false;
    }
    return true;
  };

  const softDelete = async (id: string, recordData: Record<string, unknown>): Promise<boolean> => {
    // First save to deleted_records
    const { error: saveError } = await supabase
      .from('deleted_records')
      .insert({
        table_name: tableName,
        record_id: id,
        record_data: recordData
      });

    if (saveError) {
      console.error('Error saving deleted record:', saveError);
      // Continue with delete anyway
    }

    return remove(id);
  };

  const restore = async (deletedRecordId: string): Promise<T | null> => {
    const { data: deleted, error: fetchError } = await supabase
      .from('deleted_records')
      .select('*')
      .eq('id', deletedRecordId)
      .single();

    if (fetchError || !deleted) {
      console.error('Error fetching deleted record:', fetchError);
      return null;
    }

    const { record_data } = deleted;
    const { id: _, ...data } = record_data as Record<string, unknown>;

    const { data: restored, error: restoreError } = await supabase
      .from(tableName)
      .insert(data)
      .select()
      .single();

    if (restoreError) {
      console.error('Error restoring record:', restoreError);
      return null;
    }

    // Delete from deleted_records
    await supabase.from('deleted_records').delete().eq('id', deletedRecordId);

    return restored as T;
  };

  return { create, update, remove, softDelete, restore };
}

```

## `./src/index.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

/* Custom styles */
:root {
  --color-primary: #1e40af;
  --color-secondary: #ffffff;
  --color-accent: #16a34a;
}

/* Smooth scrolling */
html {
  scroll-behavior: smooth;
}

/* Hide scrollbar but keep functionality */
.hide-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.hide-scrollbar::-webkit-scrollbar {
  display: none;
}

/* Animations */
@keyframes fade-in {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes slide-up {
  from {
    opacity: 0;
    transform: translateY(40px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

@keyframes scroll {
  0%, 100% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(8px);
  }
}

.animate-fade-in {
  animation: fade-in 0.6s ease-out forwards;
}

.animate-slide-up {
  animation: slide-up 0.8s ease-out forwards;
}

.animate-scroll {
  animation: scroll 2s ease-in-out infinite;
}

/* Line clamp */
.line-clamp-2 {
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.line-clamp-3 {
  display: -webkit-box;
  -webkit-line-clamp: 3;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* Custom scrollbar for webkit browsers */
::-webkit-scrollbar {
  width: 8px;
  height: 8px;
}

::-webkit-scrollbar-track {
  background: #f1f1f1;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb {
  background: #c1c1c1;
  border-radius: 4px;
}

::-webkit-scrollbar-thumb:hover {
  background: #a1a1a1;
}

/* Dark mode scrollbar */
.dark ::-webkit-scrollbar-track {
  background: #374151;
}

.dark ::-webkit-scrollbar-thumb {
  background: #6b7280;
}

.dark ::-webkit-scrollbar-thumb:hover {
  background: #9ca3af;
}

/* Focus styles */
button:focus-visible,
a:focus-visible,
input:focus-visible,
textarea:focus-visible,
select:focus-visible {
  outline: 2px solid #3b82f6;
  outline-offset: 2px;
}

/* Print styles for member cards */
@media print {
  body {
    margin: 0;
    padding: 0;
  }

  .no-print {
    display: none !important;
  }
}

```

## `./src/lib/supabase.ts`

```ts
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL;
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY;

export const supabase = createClient(supabaseUrl, supabaseAnonKey);

// Types
export interface ChurchSettings {
  id: string;
  church_name: string;
  motto: string;
  slogan: string;
  primary_color: string;
  secondary_color: string;
  accent_color: string;
  logo_url: string | null;
  address: string;
  phone: string;
  email: string;
  website: string;
  service_times: ServiceTime[];
  theme_of_year: string;
  verse_of_year: string;
  welcome_message: string;
  social_links: SocialLinks;
  created_at: string;
  updated_at: string;
}

export interface ServiceTime {
  day: string;
  time: string;
  name: string;
}

export interface SocialLinks {
  facebook?: string;
  twitter?: string;
  instagram?: string;
  youtube?: string;
  whatsapp?: string;
}

export interface Department {
  id: string;
  name: string;
  description: string | null;
  created_at: string;
}

export interface AdminUser {
  id: string;
  email: string;
  full_name: string;
  role: 'pastor' | 'secretary' | 'media' | 'admin';
  two_factor_enabled: boolean;
  active: boolean;
  created_at: string;
}

export interface Member {
  id: string;
  member_id: string;
  full_name: string;
  date_of_birth: string;
  gender: 'Male' | 'Female';
  phone: string | null;
  email: string | null;
  address: string | null;
  occupation: string | null;
  department_id: string | null;
  photo_url: string | null;
  fellowship: string | null;
  referral_source: string | null;
  referral_name: string | null;
  prayer_request: string | null;
  status: 'pending' | 'approved' | 'rejected';
  created_at: string;
  updated_at: string;
  departments?: Department;
}

export interface Event {
  id: string;
  title: string;
  description: string | null;
  event_date: string;
  end_date: string | null;
  start_time: string | null;
  end_time: string | null;
  location: string | null;
  image_url: string | null;
  video_url: string | null;
  status: 'upcoming' | 'completed' | 'cancelled';
  created_at: string;
  updated_at: string;
}

export interface Sermon {
  id: string;
  title: string;
  speaker: string;
  scripture_reference: string | null;
  sermon_date: string;
  end_date: string | null;
  description: string | null;
  audio_url: string | null;
  video_url: string | null;
  thumbnail_url: string | null;
  created_at: string;
  updated_at: string;
}

export interface GalleryItem {
  id: string;
  title: string | null;
  type: 'image' | 'video';
  url: string;
  thumbnail_url: string | null;
  event_id: string | null;
  end_date: string | null;
  created_at: string;
}

export interface Announcement {
  id: string;
  title: string;
  content: string;
  image_url: string | null;
  video_url: string | null;
  link_url: string | null;
  priority: number;
  active: boolean;
  start_date: string | null;
  end_date: string | null;
  created_at: string;
  updated_at: string;
}

export const isExpired = (endDate: string | null): boolean => {
  if (!endDate) return false;
  const expireDate = new Date(endDate);
  expireDate.setDate(expireDate.getDate() + 2);
  return new Date() > expireDate;
};

export interface Advert {
  id: string;
  title: string;
  description: string | null;
  company_name: string | null;
  contact_info: string | null;
  website_url: string | null;
  social_url: string | null;
  image_url: string | null;
  video_url: string | null;
  image_size: 'small' | 'medium' | 'large';
  active: boolean;
  end_date: string | null;
  created_at: string;
  updated_at: string;
}

export interface Attendance {
  id: string;
  member_id: string;
  service_date: string;
  service_type: string;
  status: 'present' | 'absent' | 'late';
  notes: string | null;
  created_at: string;
  members?: Member;
}

export interface Donation {
  id: string;
  member_id: string | null;
  amount: number;
  currency: string;
  donation_type: string | null;
  payment_method: string | null;
  reference_number: string | null;
  donation_date: string;
  end_date: string | null;
  notes: string | null;
  created_at: string;
}

export interface PastorHistory {
  id: string;
  full_name: string;
  title: string | null;
  role: string;
  bio: string | null;
  quote: string | null;
  photo_url: string | null;
  start_date: string | null;
  end_date: string | null;
  current: boolean;
  created_at: string;
  updated_at: string;
}

export interface ContactMessage {
  id: string;
  name: string;
  email: string;
  phone: string | null;
  subject: string | null;
  message: string;
  status: 'unread' | 'read' | 'replied';
  created_at: string;
}

export interface DeletedRecord {
  id: string;
  table_name: string;
  record_id: string;
  record_data: Record<string, unknown>;
  deleted_at: string;
}

export interface SpecialDate {
  id: string;
  member_id: string;
  date_type: 'birthday' | 'anniversary' | 'other';
  date_value: string;
  reminder_days: number;
  last_notified: string | null;
  created_at: string;
}

// Helper functions
export function calculateAge(dateOfBirth: string): number {
  const today = new Date();
  const dob = new Date(dateOfBirth);
  let age = today.getFullYear() - dob.getFullYear();
  const monthDiff = today.getMonth() - dob.getMonth();
  if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < dob.getDate())) {
    age--;
  }
  return age;
}

export function calculateFellowship(dateOfBirth: string, gender: string): string {
  const age = calculateAge(dateOfBirth);
  if (age < 30) return 'Youth Fellowship';
  if (gender === 'Male') return "Men's Fellowship";
  return "Women's Fellowship";
}

export function generateMemberId(): string {
  const year = new Date().getFullYear();
  const random = Math.floor(Math.random() * 10000).toString().padStart(4, '0');
  return `DWM-${year}-${random}`;
}

// Database helpers
export async function getChurchSettings(): Promise<ChurchSettings | null> {
  const { data, error } = await supabase
    .from('church_settings')
    .select('*')
    .limit(1)
    .single();

  if (error) {
    console.error('Error fetching church settings:', error);
    return null;
  }
  return data;
}

export async function updateChurchSettings(settings: Partial<ChurchSettings>): Promise<boolean> {
  const { data: existing } = await supabase
    .from('church_settings')
    .select('id')
    .limit(1)
    .single();

  if (!existing) return false;

  const { error } = await supabase
    .from('church_settings')
    .update({ ...settings, updated_at: new Date().toISOString() })
    .eq('id', existing.id);

  return !error;
}

export async function getDepartments(): Promise<Department[]> {
  const { data, error } = await supabase
    .from('departments')
    .select('*')
    .order('name');

  if (error) {
    console.error('Error fetching departments:', error);
    return [];
  }
  return data || [];
}

export async function getMembers(status?: string): Promise<Member[]> {
  let query = supabase
    .from('members')
    .select('*, departments(*)')
    .order('created_at', { ascending: false });

  if (status) {
    query = query.eq('status', status);
  }

  const { data, error } = await query;

  if (error) {
    console.error('Error fetching members:', error);
    return [];
  }
  return data || [];
}

export async function getEvents(status?: string): Promise<Event[]> {
  let query = supabase
    .from('events')
    .select('*')
    .order('event_date', { ascending: true });

  if (status) {
    query = query.eq('status', status);
  }

  const { data, error } = await query;

  if (error) {
    console.error('Error fetching events:', error);
    return [];
  }
  return data || [];
}

export async function getSermons(): Promise<Sermon[]> {
  const { data, error } = await supabase
    .from('sermons')
    .select('*')
    .order('sermon_date', { ascending: false });

  if (error) {
    console.error('Error fetching sermons:', error);
    return [];
  }
  return data || [];
}

export async function getGallery(): Promise<GalleryItem[]> {
  const { data, error } = await supabase
    .from('gallery')
    .select('*')
    .order('created_at', { ascending: false });

  if (error) {
    console.error('Error fetching gallery:', error);
    return [];
  }
  return data || [];
}

export async function getAnnouncements(activeOnly = false): Promise<Announcement[]> {
  let query = supabase
    .from('announcements')
    .select('*')
    .order('priority', { ascending: false });

  if (activeOnly) {
    query = query.eq('active', true);
  }

  const { data, error } = await query;

  if (error) {
    console.error('Error fetching announcements:', error);
    return [];
  }
  return data || [];
}

export async function getAdverts(activeOnly = false): Promise<Advert[]> {
  let query = supabase
    .from('adverts')
    .select('*')
    .order('created_at', { ascending: false });

  if (activeOnly) {
    query = query.eq('active', true);
  }

  const { data, error } = await query;

  if (error) {
    console.error('Error fetching adverts:', error);
    return [];
  }
  return data || [];
}

export async function getPastors(): Promise<PastorHistory[]> {
  const { data, error } = await supabase
    .from('pastor_history')
    .select('*')
    .order('current', { ascending: false })
    .order('start_date', { ascending: false });

  if (error) {
    console.error('Error fetching pastors:', error);
    return [];
  }
  return data || [];
}

export async function getAttendance(date?: string): Promise<Attendance[]> {
  let query = supabase
    .from('attendance')
    .select('*, members(*)')
    .order('service_date', { ascending: false });

  if (date) {
    query = query.eq('service_date', date);
  }

  const { data, error } = await query;

  if (error) {
    console.error('Error fetching attendance:', error);
    return [];
  }
  return data || [];
}

export async function getDonations(): Promise<Donation[]> {
  const { data, error } = await supabase
    .from('donations')
    .select('*')
    .order('donation_date', { ascending: false });

  if (error) {
    console.error('Error fetching donations:', error);
    return [];
  }
  return data || [];
}

export async function getContactMessages(): Promise<ContactMessage[]> {
  const { data, error } = await supabase
    .from('contact_messages')
    .select('*')
    .order('created_at', { ascending: false });

  if (error) {
    console.error('Error fetching contact messages:', error);
    return [];
  }
  return data || [];
}

export async function getDeletedRecords(): Promise<DeletedRecord[]> {
  const { data, error } = await supabase
    .from('deleted_records')
    .select('*')
    .order('deleted_at', { ascending: false });

  if (error) {
    console.error('Error fetching deleted records:', error);
    return [];
  }
  return data || [];
}

// Upload helper for Supabase storage
export async function uploadFile(file: File, bucket: string, path: string): Promise<string | null> {
  const { data, error } = await supabase.storage
    .from(bucket)
    .upload(path, file, {
      cacheControl: '3600',
      upsert: true
    });

  if (error) {
    console.error('Error uploading file:', error);
    return null;
  }

  const { data: urlData } = supabase.storage
    .from(bucket)
    .getPublicUrl(data.path);

  return urlData.publicUrl;
}

```

## `./src/main.tsx`

```tsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import App from './App.tsx';
import './index.css';

createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>
);

```

## `./src/pages/AboutPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { useTheme } from '../contexts/ThemeContext';
import { supabase, PastorHistory } from '../lib/supabase';
import { Card, LoadingSpinner } from '../components/ui';
import { Heart, Target, Users, BookOpen, Quote } from 'lucide-react';

export function AboutPage() {
  const { settings } = useTheme();
  const [pastors, setPastors] = useState<PastorHistory[]>([]);
  const [loading, setLoading] = useState(true);
  const [aboutContent, setAboutContent] = useState<string | null>(null);

  useEffect(() => {
    Promise.all([
      supabase.from('pastor_history').select('*').order('current', { ascending: false }).order('start_date', { ascending: false }),
      supabase.from('church_settings').select('about_text').single()
    ]).then(([pastorData, settingsData]) => {
      if (pastorData.data) setPastors(pastorData.data);
      if (settingsData.data?.about_text) setAboutContent(settingsData.data.about_text);
      setLoading(false);
    });
  }, []);

  if (loading) return <div className="min-h-screen flex items-center justify-center"><LoadingSpinner size="lg" /></div>;

  // Check if admin has set custom content
  const hasCustomContent = aboutContent && aboutContent.trim().length > 0;

  return (
    <div className="min-h-screen">
      {/* Hero Section */}
      <section className="relative py-20 bg-gradient-to-br from-blue-900 to-blue-700 text-white">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <h1 className="text-4xl md:text-5xl font-bold mb-4">About Us</h1>
          <p className="text-xl text-blue-100">{settings.motto}</p>
        </div>
      </section>

      {/* Custom About Content from Admin OR Default Content */}
      <section className="py-16 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-4xl mx-auto px-4">
          {hasCustomContent ? (
            <div className="prose prose-lg max-w-none dark:prose-invert">
              <div dangerouslySetInnerHTML={{ __html: aboutContent.replace(/\n/g, '<br/>') }} />
            </div>
          ) : (
            <>
              <h2 className="text-3xl font-bold text-center text-gray-900 dark:text-white mb-8">Our History</h2>
              <div className="prose prose-lg max-w-none">
                <p className="text-gray-600 dark:text-gray-400 text-lg leading-relaxed mb-6">
                  Defence Word Ministry was established with a divine mandate to bring the transformative power of God's Word to every corner of our community. Founded on the principles of faith, love, and service, we have grown from a small gathering to a vibrant community of believers.
                </p>
                <p className="text-gray-600 dark:text-gray-400 text-lg leading-relaxed mb-6">
                  Our motto, "{settings.motto}", reflects our unwavering belief that with God, nothing is impossible. Through the years, we have witnessed countless testimonies of God's faithfulness and miraculous interventions in the lives of our members.
                </p>
                <p className="text-gray-600 dark:text-gray-400 text-lg leading-relaxed">
                  Today, we continue to be a place where people from all walks of life can encounter God's love, find their purpose, and grow in their faith journey.
                </p>
              </div>
            </>
          )}
        </div>
      </section>

      {/* Mission & Vision */}
      <section className="py-16 bg-white dark:bg-gray-800">
        <div className="max-w-7xl mx-auto px-4">
          <div className="grid md:grid-cols-3 gap-8">
            <Card className="p-8 text-center hover:shadow-xl transition-shadow">
              <div className="w-16 h-16 mx-auto mb-6 rounded-full bg-blue-100 dark:bg-blue-900/20 flex items-center justify-center">
                <Heart className="h-8 w-8 text-blue-600" />
              </div>
              <h3 className="text-xl font-bold text-gray-900 dark:text-white mb-4">Our Mission</h3>
              <p className="text-gray-600 dark:text-gray-400">To spread the Gospel of Jesus Christ and transform lives through the power of God's Word, fostering a community of love, faith, and service.</p>
            </Card>

            <Card className="p-8 text-center hover:shadow-xl transition-shadow">
              <div className="w-16 h-16 mx-auto mb-6 rounded-full bg-green-100 dark:bg-green-900/20 flex items-center justify-center">
                <Target className="h-8 w-8 text-green-600" />
              </div>
              <h3 className="text-xl font-bold text-gray-900 dark:text-white mb-4">Our Vision</h3>
              <p className="text-gray-600 dark:text-gray-400">To be a beacon of hope in our community, raising disciples who will impact generations and transform nations through the teachings of Christ.</p>
            </Card>

            <Card className="p-8 text-center hover:shadow-xl transition-shadow">
              <div className="w-16 h-16 mx-auto mb-6 rounded-full bg-purple-100 dark:bg-purple-900/20 flex items-center justify-center">
                <Users className="h-8 w-8 text-purple-600" />
              </div>
              <h3 className="text-xl font-bold text-gray-900 dark:text-white mb-4">Our Values</h3>
              <p className="text-gray-600 dark:text-gray-400">Faith, Love, Integrity, Excellence, and Community - guiding principles that shape everything we do as a church family.</p>
            </Card>
          </div>
        </div>
      </section>

      {/* Pastoral Team */}
      <section className="py-16 bg-white dark:bg-gray-800">
        <div className="max-w-7xl mx-auto px-4">
          <h2 className="text-3xl font-bold text-center text-gray-900 dark:text-white mb-12">Our Leadership</h2>
          {pastors.length > 0 ? (
            <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
              {pastors.map((pastor) => (
                <Card key={pastor.id} className="overflow-hidden hover:shadow-xl transition-all duration-300">
                  <div className="h-64 relative">
                    {pastor.photo_url ? (
                      <img src={pastor.photo_url} alt={pastor.full_name} className="w-full h-full object-cover" />
                    ) : (
                      <div className="w-full h-full bg-gradient-to-br from-blue-600 to-green-600 flex items-center justify-center text-white text-6xl font-bold">{pastor.full_name.charAt(0)}</div>
                    )}
                    {pastor.current && <div className="absolute top-4 right-4 bg-green-500 text-white px-3 py-1 rounded-full text-sm font-medium">Current</div>}
                  </div>
                  <div className="p-6">
                    <h3 className="text-xl font-bold text-gray-900 dark:text-white mb-1">{pastor.full_name}</h3>
                    <p className="text-blue-600 dark:text-blue-400 font-medium mb-3">{pastor.title} {pastor.role}</p>
                    {pastor.bio && <p className="text-gray-600 dark:text-gray-400 text-sm line-clamp-3 mb-4">{pastor.bio}</p>}
                    {pastor.quote && <div className="flex items-start space-x-2 text-gray-500 text-sm italic"><Quote className="h-4 w-4 flex-shrink-0 mt-1" /><span>"{pastor.quote}"</span></div>}
                  </div>
                </Card>
              ))}
            </div>
          ) : <div className="text-center text-gray-500 dark:text-gray-400">Leadership profiles will be available soon.</div>}
        </div>
      </section>

      {/* What We Believe */}
      <section className="py-16 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-7xl mx-auto px-4">
          <h2 className="text-3xl font-bold text-center text-gray-900 dark:text-white mb-12">What We Believe</h2>
          <div className="grid md:grid-cols-2 gap-8">
            <Card className="p-6">
              <div className="flex items-start space-x-4">
                <div className="w-12 h-12 rounded-full bg-blue-100 dark:bg-blue-900/20 flex items-center justify-center flex-shrink-0"><BookOpen className="h-6 w-6 text-blue-600" /></div>
                <div>
                  <h3 className="font-bold text-gray-900 dark:text-white mb-2">The Bible</h3>
                  <p className="text-gray-600 dark:text-gray-400 text-sm">We believe the Bible is the inspired, infallible Word of God and is the authoritative rule of faith and conduct.</p>
                </div>
              </div>
            </Card>

            <Card className="p-6">
              <div className="flex items-start space-x-4">
                <div className="w-12 h-12 rounded-full bg-green-100 dark:bg-green-900/20 flex items-center justify-center flex-shrink-0"><Heart className="h-6 w-6 text-green-600" /></div>
                <div>
                  <h3 className="font-bold text-gray-900 dark:text-white mb-2">Salvation</h3>
                  <p className="text-gray-600 dark:text-gray-400 text-sm">Salvation is received through repentance toward God and faith in the Lord Jesus Christ. It is a gift of grace, not works.</p>
                </div>
              </div>
            </Card>

            <Card className="p-6">
              <div className="flex items-start space-x-4">
                <div className="w-12 h-12 rounded-full bg-purple-100 dark:bg-purple-900/20 flex items-center justify-center flex-shrink-0"><Users className="h-6 w-6 text-purple-600" /></div>
                <div>
                  <h3 className="font-bold text-gray-900 dark:text-white mb-2">The Church</h3>
                  <p className="text-gray-600 dark:text-gray-400 text-sm">The Church is the body of Christ, consisting of all believers who have been born again and united in faith.</p>
                </div>
              </div>
            </Card>

            <Card className="p-6">
              <div className="flex items-start space-x-4">
                <div className="w-12 h-12 rounded-full bg-yellow-100 dark:bg-yellow-900/20 flex items-center justify-center flex-shrink-0"><Target className="h-6 w-6 text-yellow-600" /></div>
                <div>
                  <h3 className="font-bold text-gray-900 dark:text-white mb-2">The Great Commission</h3>
                  <p className="text-gray-600 dark:text-gray-400 text-sm">We are called to go and make disciples of all nations, sharing the Gospel and demonstrating God's love.</p>
                </div>
              </div>
            </Card>
          </div>
        </div>
      </section>
    </div>
  );
}

```

## `./src/pages/ContactPage.tsx`

```tsx
import React, { useState } from 'react';
import { useTheme } from '../contexts/ThemeContext';
import { supabase } from '../lib/supabase';
import { Button, Card, Input, TextArea } from '../components/ui';
import { Phone, Mail, MapPin, Send, CheckCircle } from 'lucide-react';

export function ContactPage() {
  const { settings } = useTheme();
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    phone: '',
    subject: '',
    message: ''
  });
  const [loading, setLoading] = useState(false);
  const [success, setSuccess] = useState(false);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);

    const { error } = await supabase
      .from('contact_messages')
      .insert(formData);

    if (!error) {
      setSuccess(true);
      setFormData({ name: '', email: '', phone: '', subject: '', message: '' });
    } else {
      alert('Failed to send message. Please try again.');
    }

    setLoading(false);
  };

  return (
    <div className="min-h-screen">
      {/* Hero Section */}
      <section className="relative py-20 bg-gradient-to-br from-blue-900 to-blue-700 text-white">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <Mail className="w-16 h-16 mx-auto mb-4 opacity-80" />
          <h1 className="text-4xl md:text-5xl font-bold mb-4">Contact Us</h1>
          <p className="text-xl opacity-90">We'd love to hear from you</p>
        </div>
      </section>

      {/* Main Content */}
      <section className="py-16 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-6xl mx-auto px-4">
          <div className="grid lg:grid-cols-2 gap-12">
            {/* Contact Form */}
            <div>
              <h2 className="text-2xl font-bold text-gray-900 dark:text-white mb-6">
                Send Us a Message
              </h2>
              <Card className="p-6">
                {success ? (
                  <div className="text-center py-8">
                    <CheckCircle className="h-16 w-16 mx-auto text-green-500 mb-4" />
                    <h3 className="text-xl font-medium text-gray-900 dark:text-white mb-2">
                      Message Sent!
                    </h3>
                    <p className="text-gray-600 dark:text-gray-400 mb-6">
                      Thank you for reaching out. We'll get back to you soon.
                    </p>
                    <Button variant="outline" onClick={() => setSuccess(false)}>
                      Send Another Message
                    </Button>
                  </div>
                ) : (
                  <form onSubmit={handleSubmit} className="space-y-5">
                    <div className="grid md:grid-cols-2 gap-4">
                      <Input
                        label="Name *"
                        name="name"
                        value={formData.name}
                        onChange={handleChange}
                        placeholder="Your name"
                        required
                      />
                      <Input
                        label="Email *"
                        name="email"
                        type="email"
                        value={formData.email}
                        onChange={handleChange}
                        placeholder="your@email.com"
                        required
                      />
                    </div>
                    <div className="grid md:grid-cols-2 gap-4">
                      <Input
                        label="Phone"
                        name="phone"
                        value={formData.phone}
                        onChange={handleChange}
                        placeholder="+233..."
                      />
                      <Input
                        label="Subject"
                        name="subject"
                        value={formData.subject}
                        onChange={handleChange}
                        placeholder="What's this about?"
                      />
                    </div>
                    <TextArea
                      label="Message *"
                      name="message"
                      value={formData.message}
                      onChange={handleChange}
                      placeholder="Your message..."
                      rows={5}
                      required
                    />
                    <Button type="submit" className="w-full" loading={loading}>
                      <Send className="h-4 w-4 mr-2" />
                      Send Message
                    </Button>
                  </form>
                )}
              </Card>
            </div>

            {/* Contact Info */}
            <div>
              <h2 className="text-2xl font-bold text-gray-900 dark:text-white mb-6">
                Get in Touch
              </h2>
              <div className="space-y-4">
                {settings.address && (
                  <Card className="p-6">
                    <div className="flex items-start space-x-4">
                      <div className="w-12 h-12 rounded-full bg-blue-100 dark:bg-blue-900/20 flex items-center justify-center flex-shrink-0">
                        <MapPin className="h-6 w-6 text-blue-600" />
                      </div>
                      <div>
                        <h3 className="font-bold text-gray-900 dark:text-white mb-1">Address</h3>
                        <p className="text-gray-600 dark:text-gray-400">{settings.address}</p>
                      </div>
                    </div>
                  </Card>
                )}

                {settings.phone && (
                  <Card className="p-6">
                    <div className="flex items-start space-x-4">
                      <div className="w-12 h-12 rounded-full bg-green-100 dark:bg-green-900/20 flex items-center justify-center flex-shrink-0">
                        <Phone className="h-6 w-6 text-green-600" />
                      </div>
                      <div>
                        <h3 className="font-bold text-gray-900 dark:text-white mb-1">Phone</h3>
                        <p className="text-gray-600 dark:text-gray-400">{settings.phone}</p>
                      </div>
                    </div>
                  </Card>
                )}

                {settings.email && (
                  <Card className="p-6">
                    <div className="flex items-start space-x-4">
                      <div className="w-12 h-12 rounded-full bg-purple-100 dark:bg-purple-900/20 flex items-center justify-center flex-shrink-0">
                        <Mail className="h-6 w-6 text-purple-600" />
                      </div>
                      <div>
                        <h3 className="font-bold text-gray-900 dark:text-white mb-1">Email</h3>
                        <p className="text-gray-600 dark:text-gray-400">{settings.email}</p>
                      </div>
                    </div>
                  </Card>
                )}
              </div>

              {/* Map Placeholder */}
              <div className="mt-8 rounded-xl overflow-hidden h-64 bg-gray-200 dark:bg-gray-700">
                <iframe
                  src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3970.7126610184283!2d-0.18656842505859376!3d5.603729494393965!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x0%3A0x0!2zNcKwMzYnMTMuNCJOIDDCsDExJzExLjciVw!5e0!3m2!1sen!2sgh!4v1234567890"
                  className="w-full h-full"
                  style={{ border: 0 }}
                  allowFullScreen
                  loading="lazy"
                  referrerPolicy="no-referrer-when-downgrade"
                  title="Church Location"
                />
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  );
}

```

## `./src/pages/DonationPage.tsx`

```tsx
import React, { useState } from 'react';
import { useTheme } from '../contexts/ThemeContext';
import { Button, Card, Input } from '../components/ui';
import { Heart, Phone, Mail, MapPin, ExternalLink } from 'lucide-react';

export function DonationPage() {
  const { settings } = useTheme();
  const [amount, setAmount] = useState('');

  const predefinedAmounts = [50, 100, 200, 500, 1000];

  return (
    <div className="min-h-screen">
      {/* Hero Section */}
      <section className="relative py-20 bg-gradient-to-br from-green-700 to-blue-600 text-white">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <Heart className="w-16 h-16 mx-auto mb-4 opacity-80" />
          <h1 className="text-4xl md:text-5xl font-bold mb-4">Support the Ministry</h1>
          <p className="text-xl opacity-90">Your generosity helps us spread God's love</p>
        </div>
      </section>

      {/* Main Content */}
      <section className="py-16 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-5xl mx-auto px-4">
          <div className="grid lg:grid-cols-2 gap-12">
            {/* Donation Form */}
            <div>
              <h2 className="text-2xl font-bold text-gray-900 dark:text-white mb-6">
                Make a Donation
              </h2>
              <Card className="p-6">
                <p className="text-gray-600 dark:text-gray-400 mb-6">
                  Support our mission to spread the Gospel and serve our community. Your contribution makes a difference.
                </p>

                <div className="mb-6">
                  <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-3">
                    Select Amount (GHS)
                  </label>
                  <div className="grid grid-cols-3 gap-3 mb-4">
                    {predefinedAmounts.map((amt) => (
                      <button
                        key={amt}
                        onClick={() => setAmount(amt.toString())}
                        className={`py-3 rounded-lg font-medium transition-colors ${
                          amount === amt.toString()
                            ? 'bg-green-600 text-white'
                            : 'bg-gray-100 dark:bg-gray-700 text-gray-700 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-600'
                        }`}
                      >
                        {amt}
                      </button>
                    ))}
                  </div>
                  <Input
                    type="number"
                    placeholder="Or enter custom amount"
                    value={amount}
                    onChange={(e) => setAmount(e.target.value)}
                  />
                </div>

                <div className="space-y-4 mb-6">
                  <h3 className="font-medium text-gray-900 dark:text-white">Payment Options</h3>

                  {/* Mobile Money */}
                  <Card className="p-4 border-2 border-green-200 dark:border-green-800">
                    <div className="flex items-center justify-between">
                      <div>
                        <h4 className="font-medium text-gray-900 dark:text-white">Mobile Money</h4>
                        <p className="text-sm text-gray-600 dark:text-gray-400">MTN, Vodafone, AirtelTigo</p>
                      </div>
                      <div className="text-right">
                        <p className="font-bold text-green-600">{settings.phone || '+233 XX XXX XXXX'}</p>
                        <p className="text-xs text-gray-500">Reference: DWM Donation</p>
                      </div>
                    </div>
                  </Card>

                  {/* Bank Transfer */}
                  <Card className="p-4">
                    <h4 className="font-medium text-gray-900 dark:text-white mb-2">Bank Transfer</h4>
                    <div className="text-sm text-gray-600 dark:text-gray-400 space-y-1">
                      <p>Bank: [Bank Name]</p>
                      <p>Account: [Account Number]</p>
                      <p>Name: {settings.church_name}</p>
                    </div>
                  </Card>

                  {/* Online Payment */}
                  <Button
                    className="w-full"
                    onClick={() => {
                      if (!amount) {
                        alert('Please enter an amount');
                        return;
                      }
                      alert(`Thank you! Redirect to payment gateway for GHS ${amount}`);
                    }}
                  >
                    Pay Online (GHS {amount || '0'})
                  </Button>
                </div>
              </Card>
            </div>

            {/* Info Section */}
            <div>
              <h2 className="text-2xl font-bold text-gray-900 dark:text-white mb-6">
                How Your Donation Helps
              </h2>
              <div className="space-y-4">
                <Card className="p-6">
                  <h3 className="font-bold text-gray-900 dark:text-white mb-3">Support Our Programs</h3>
                  <p className="text-gray-600 dark:text-gray-400 text-sm">
                    Your donations support our youth programs, community outreach, and charitable initiatives.
                  </p>
                </Card>

                <Card className="p-6">
                  <h3 className="font-bold text-gray-900 dark:text-white mb-3">Church Operations</h3>
                  <p className="text-gray-600 dark:text-gray-400 text-sm">
                    Help us maintain our facilities, pay staff, and keep our doors open to everyone.
                  </p>
                </Card>

                <Card className="p-6">
                  <h3 className="font-bold text-gray-900 dark:text-white mb-3">Mission Work</h3>
                  <p className="text-gray-600 dark:text-gray-400 text-sm">
                    Fund missionary work and help us reach more communities with the Gospel.
                  </p>
                </Card>
              </div>

              {/* Contact Info */}
              <div className="mt-8 p-6 bg-gradient-to-r from-blue-600 to-green-600 rounded-xl text-white">
                <h3 className="font-bold mb-4">Get in Touch</h3>
                <div className="space-y-3 text-sm">
                  {settings.address && (
                    <div className="flex items-center">
                      <MapPin className="h-4 w-4 mr-3 flex-shrink-0" />
                      <span>{settings.address}</span>
                    </div>
                  )}
                  {settings.phone && (
                    <div className="flex items-center">
                      <Phone className="h-4 w-4 mr-3 flex-shrink-0" />
                      <span>{settings.phone}</span>
                    </div>
                  )}
                  {settings.email && (
                    <div className="flex items-center">
                      <Mail className="h-4 w-4 mr-3 flex-shrink-0" />
                      <span>{settings.email}</span>
                    </div>
                  )}
                </div>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  );
}

```

## `./src/pages/EventsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, Event } from '../lib/supabase';
import { Card, LoadingSpinner } from '../components/ui';
import { Calendar, MapPin, Clock, Filter, Video } from 'lucide-react';

const isExpired = (endDate: string | null): boolean => {
  if (!endDate) return false;
  const expireDate = new Date(endDate);
  expireDate.setDate(expireDate.getDate() + 2);
  return new Date() > expireDate;
};

export function EventsPage() {
  const [events, setEvents] = useState<Event[]>([]);
  const [loading, setLoading] = useState(true);
  const [filter, setFilter] = useState<'all' | 'upcoming' | 'completed'>('upcoming');

  useEffect(() => {
    let query = supabase.from('events').select('*').order('event_date', { ascending: filter === 'upcoming' });
    if (filter !== 'all') query = query.eq('status', filter);

    query.then(({ data }) => {
      if (data) {
        const filtered = data.filter(e => !isExpired(e.end_date));
        if (filter === 'upcoming') {
          setEvents(filtered.filter(e => new Date(e.event_date) >= new Date(new Date().toDateString())));
        } else {
          setEvents(filtered);
        }
      }
      setLoading(false);
    });
  }, [filter]);

  if (loading) return <div className="min-h-screen flex items-center justify-center"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="min-h-screen">
      <section className="relative py-20 bg-gradient-to-br from-blue-900 to-green-700 text-white">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <Calendar className="w-16 h-16 mx-auto mb-4 opacity-80" />
          <h1 className="text-4xl md:text-5xl font-bold mb-4">Events</h1>
          <p className="text-xl opacity-90">Join us for worship, fellowship, and spiritual growth</p>
        </div>
      </section>

      <section className="py-6 bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700">
        <div className="max-w-7xl mx-auto px-4">
          <div className="flex items-center justify-between">
            <div className="flex items-center space-x-2">
              <Filter className="h-5 w-5 text-gray-400" />
              <span className="text-sm text-gray-500 dark:text-gray-400">Filter:</span>
            </div>
            <div className="flex space-x-2">
              {(['all', 'upcoming', 'completed'] as const).map((f) => (
                <button key={f} onClick={() => setFilter(f)} className={`px-4 py-2 rounded-lg text-sm font-medium transition-colors ${filter === f ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 hover:bg-gray-200 dark:hover:bg-gray-600'}`}>{f.charAt(0).toUpperCase() + f.slice(1)}</button>
              ))}
            </div>
          </div>
        </div>
      </section>

      <section className="py-12 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-7xl mx-auto px-4">
          {events.length > 0 ? (
            <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
              {events.map((event) => (
                <Card key={event.id} className="overflow-hidden hover:shadow-xl transition-all duration-300 group">
                  <div className="relative">
                    {event.image_url ? (
                      <img src={event.image_url} alt={event.title} className="w-full h-48 object-cover group-hover:scale-105 transition-transform duration-300" />
                    ) : (
                      <div className="w-full h-48 bg-gradient-to-br from-blue-600 to-green-600 flex items-center justify-center">
                        <div className="text-center text-white">
                          <div className="text-5xl font-bold">{new Date(event.event_date).getDate()}</div>
                          <div className="text-sm uppercase">{new Date(event.event_date).toLocaleDateString('en-US', { month: 'short' })}</div>
                        </div>
                      </div>
                    )}
                    {event.video_url && (
                      <div className="absolute top-4 left-4 bg-purple-600 text-white px-2 py-1 rounded text-xs flex items-center"><Video className="h-3 w-3 mr-1" /> Video</div>
                    )}
                  </div>
                  <div className="p-6">
                    <h3 className="text-lg font-bold text-gray-900 dark:text-white mb-2 group-hover:text-blue-600 transition-colors">{event.title}</h3>
                    <div className="space-y-2 text-sm text-gray-500 dark:text-gray-400">
                      <div className="flex items-center"><Calendar className="h-4 w-4 mr-2" />{new Date(event.event_date).toLocaleDateString('en-US', { weekday: 'long', month: 'long', day: 'numeric', year: 'numeric' })}</div>
                      {event.start_time && <div className="flex items-center"><Clock className="h-4 w-4 mr-2" />{event.start_time}{event.end_time && ` - ${event.end_time}`}</div>}
                      {event.location && <div className="flex items-center"><MapPin className="h-4 w-4 mr-2" />{event.location}</div>}
                    </div>
                    {event.description && <p className="mt-4 text-gray-600 dark:text-gray-400 text-sm line-clamp-2">{event.description}</p>}
                  </div>
                </Card>
              ))}
            </div>
          ) : (
            <div className="text-center py-16">
              <Calendar className="h-16 w-16 mx-auto text-gray-300 dark:text-gray-600 mb-4" />
              <h3 className="text-xl font-medium text-gray-900 dark:text-white mb-2">No Events</h3>
              <p className="text-gray-500 dark:text-gray-400">Check back later for upcoming events.</p>
            </div>
          )}
        </div>
      </section>
    </div>
  );
}

```

## `./src/pages/GalleryPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, GalleryItem } from '../lib/supabase';
import { Card, LoadingSpinner } from '../components/ui';
import { Image, Play, X } from 'lucide-react';

const isExpired = (endDate: string | null): boolean => {
  if (!endDate) return false;
  const expireDate = new Date(endDate);
  expireDate.setDate(expireDate.getDate() + 2);
  return new Date() > expireDate;
};

export function GalleryPage() {
  const [gallery, setGallery] = useState<GalleryItem[]>([]);
  const [loading, setLoading] = useState(true);
  const [selectedItem, setSelectedItem] = useState<GalleryItem | null>(null);
  const [filter, setFilter] = useState<'all' | 'image' | 'video'>('all');

  useEffect(() => {
    supabase.from('gallery').select('*').order('created_at', { ascending: false }).then(({ data }) => {
      if (data) setGallery(data.filter(item => !isExpired(item.end_date)));
      setLoading(false);
    });
  }, []);

  const filteredGallery = filter === 'all' ? gallery : gallery.filter(item => item.type === filter);

  if (loading) return <div className="min-h-screen flex items-center justify-center"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="min-h-screen">
      <section className="relative py-20 bg-gradient-to-br from-blue-900 to-green-700 text-white">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <Image className="w-16 h-16 mx-auto mb-4 opacity-80" />
          <h1 className="text-4xl md:text-5xl font-bold mb-4">Gallery</h1>
          <p className="text-xl opacity-90">Capture moments of faith, fellowship, and celebration</p>
        </div>
      </section>

      <section className="py-6 bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700">
        <div className="max-w-7xl mx-auto px-4">
          <div className="flex justify-center space-x-2">
            {(['all', 'image', 'video'] as const).map((f) => (
              <button key={f} onClick={() => setFilter(f)} className={`px-6 py-2 rounded-lg text-sm font-medium transition-colors ${filter === f ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300 hover:bg-gray-200'}`}>{f.charAt(0).toUpperCase() + f.slice(1)}</button>
            ))}
          </div>
        </div>
      </section>

      <section className="py-12 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-7xl mx-auto px-4">
          {filteredGallery.length > 0 ? (
            <div className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-4">
              {filteredGallery.map((item) => (
                <div key={item.id} onClick={() => setSelectedItem(item)} className="relative aspect-square cursor-pointer overflow-hidden rounded-xl group">
                  {item.type === 'video' ? (
                    <>
                      {item.thumbnail_url || item.url ? (
                        <img src={item.thumbnail_url || item.url} alt={item.title || ''} className="w-full h-full object-cover group-hover:scale-110 transition-transform duration-300" />
                      ) : (
                        <div className="w-full h-full bg-gray-300 dark:bg-gray-700" />
                      )}
                      <div className="absolute inset-0 bg-black/30 flex items-center justify-center">
                        <div className="w-12 h-12 rounded-full bg-white/90 flex items-center justify-center"><Play className="h-6 w-6 text-blue-600 ml-1" /></div>
                      </div>
                    </>
                  ) : (
                    <img src={item.url} alt={item.title || ''} className="w-full h-full object-cover group-hover:scale-110 transition-transform duration-300" />
                  )}
                  <div className="absolute inset-0 bg-gradient-to-t from-black/70 via-transparent to-transparent opacity-0 group-hover:opacity-100 transition-opacity">
                    <div className="absolute bottom-0 left-0 right-0 p-4 text-white"><p className="text-sm line-clamp-2">{item.title}</p></div>
                  </div>
                </div>
              ))}
            </div>
          ) : (
            <div className="text-center py-16">
              <Image className="h-16 w-16 mx-auto text-gray-300 dark:text-gray-600 mb-4" />
              <h3 className="text-xl font-medium text-gray-900 dark:text-white mb-2">No Gallery Items</h3>
              <p className="text-gray-500 dark:text-gray-400">Gallery items will be available soon.</p>
            </div>
          )}
        </div>
      </section>

      {selectedItem && (
        <div className="fixed inset-0 z-50 bg-black/90 flex items-center justify-center p-4" onClick={() => setSelectedItem(null)}>
          <button className="absolute top-4 right-4 text-white hover:text-gray-300" onClick={() => setSelectedItem(null)}><X className="h-8 w-8" /></button>
          <div onClick={e => e.stopPropagation()}>
            {selectedItem.type === 'video' ? (
              <video className="max-w-full max-h-[80vh] rounded-lg" src={selectedItem.url} controls autoPlay />
            ) : (
              <img className="max-w-full max-h-[80vh] rounded-lg" src={selectedItem.url} alt={selectedItem.title || ''} />
            )}
            {selectedItem.title && <p className="text-white text-center mt-4">{selectedItem.title}</p>}
          </div>
        </div>
      )}
    </div>
  );
}

```

## `./src/pages/HomePage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';
import { useTheme } from '../contexts/ThemeContext';
import { supabase, Event, Announcement, Sermon } from '../lib/supabase';
import { Button, Card, LoadingSpinner } from '../components/ui';
import { Calendar, MapPin, Clock, Play, ChevronRight, Mic } from 'lucide-react';

const isExpired = (endDate: string | null): boolean => {
  if (!endDate) return false;
  const expireDate = new Date(endDate);
  expireDate.setDate(expireDate.getDate() + 2);
  return new Date() > expireDate;
};

export function HomePage() {
  const { settings } = useTheme();
  const [events, setEvents] = useState<Event[]>([]);
  const [announcements, setAnnouncements] = useState<Announcement[]>([]);
  const [sermons, setSermons] = useState<Sermon[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchData() {
      const [
        { data: eventsData },
        { data: announcementsData },
        { data: sermonsData }
      ] = await Promise.all([
        supabase.from('events').select('*').eq('status', 'upcoming').order('event_date', { ascending: true }).limit(3),
        supabase.from('announcements').select('*').eq('active', true).order('priority', { ascending: false }).limit(5),
        supabase.from('sermons').select('*').order('sermon_date', { ascending: false }).limit(4)
      ]);

      setEvents((eventsData || []).filter(e => !isExpired(e.end_date)));
      setAnnouncements((announcementsData || []).filter(a => !isExpired(a.end_date)));
      setSermons((sermonsData || []).filter(s => !isExpired(s.end_date)));
      setLoading(false);
    }
    fetchData();
  }, []);

  if (loading) return <div className="min-h-screen flex items-center justify-center"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="min-h-screen">
      {/* Hero Section */}
      <section className="relative min-h-[90vh] flex items-center justify-center bg-gradient-to-br from-blue-900 via-blue-800 to-blue-600 overflow-hidden">
        <div className="absolute inset-0 opacity-10">
          <div className="absolute inset-0" style={{ backgroundImage: `url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.4'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E")` }} />
        </div>
        <div className="relative z-10 max-w-7xl mx-auto px-4 text-center text-white">
          <div className="animate-fade-in">
            {settings.logo_url ? (
              <img src={settings.logo_url} alt="" className="h-24 w-24 mx-auto mb-6 rounded-full shadow-2xl" />
            ) : (
              <div className="h-24 w-24 mx-auto mb-6 rounded-full bg-white/20 backdrop-blur flex items-center justify-center"><span className="text-4xl font-bold">D</span></div>
            )}
            <h1 className="text-4xl md:text-6xl lg:text-7xl font-bold mb-4 leading-tight">{settings.church_name}</h1>
            <p className="text-xl md:text-2xl font-light mb-3 text-blue-100">"{settings.motto}"</p>
            {settings.theme_of_year && (
              <div className="mb-8">
                <p className="text-lg font-semibold text-green-300">{settings.theme_of_year}</p>
                {settings.verse_of_year && <p className="text-sm text-blue-200 mt-2 italic">{settings.verse_of_year}</p>}
              </div>
            )}
            <div className="flex flex-col sm:flex-row items-center justify-center space-y-4 sm:space-y-0 sm:space-x-4 mt-8">
              <Link to="/register"><Button size="lg" className="bg-white text-blue-600 hover:bg-blue-50 w-full sm:w-auto">Join Our Community</Button></Link>
              <Link to="/about"><Button size="lg" variant="outline" className="border-white text-white hover:bg-white/10 w-full sm:w-auto">Learn More</Button></Link>
            </div>
          </div>
        </div>
        <div className="absolute bottom-8 left-1/2 transform -translate-x-1/2 animate-bounce">
          <div className="w-6 h-10 border-2 border-white/50 rounded-full flex justify-center pt-2"><div className="w-1 h-3 bg-white/70 rounded-full animate-scroll" /></div>
        </div>
      </section>

      {/* Welcome Section */}
      <section className="py-16 bg-white dark:bg-gray-800">
        <div className="max-w-7xl mx-auto px-4">
          <div className="text-center max-w-3xl mx-auto">
            <h2 className="text-3xl md:text-4xl font-bold text-gray-900 dark:text-white mb-4">{settings.welcome_message}</h2>
            <p className="text-lg text-gray-600 dark:text-gray-400">We are a community of believers dedicated to worship, fellowship, and service. Join us as we grow together in faith and love.</p>
          </div>
        </div>
      </section>

      {/* Service Times */}
      <section className="py-16 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-7xl mx-auto px-4">
          <h2 className="text-3xl font-bold text-center text-gray-900 dark:text-white mb-12">Service Times</h2>
          <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
            {(settings.service_times || []).map((service, index) => (
              <Card key={index} className="p-6 text-center hover:shadow-xl transition-shadow">
                <div className="w-14 h-14 mx-auto mb-4 rounded-full bg-blue-100 dark:bg-blue-900/20 flex items-center justify-center"><Clock className="h-7 w-7 text-blue-600 dark:text-blue-400" /></div>
                <h3 className="text-lg font-semibold text-gray-900 dark:text-white mb-2">{service.name || `${service.day} Service`}</h3>
                <p className="text-gray-600 dark:text-gray-400">{service.day}</p>
                <p className="text-blue-600 dark:text-blue-400 font-medium">{service.time}</p>
              </Card>
            ))}
          </div>
        </div>
      </section>

      {/* Announcements Banner */}
      {announcements.length > 0 && (
        <section className="py-12 bg-gradient-to-r from-blue-600 to-green-600 text-white">
          <div className="max-w-7xl mx-auto px-4">
            <div className="flex items-center justify-between mb-6">
              <h2 className="text-2xl font-bold">Announcements</h2>
              <Link to="/events" className="text-sm underline hover:no-underline flex items-center">View all <ChevronRight className="h-4 w-4 ml-1" /></Link>
            </div>
            <div className="overflow-x-auto">
              <div className="flex space-x-4 pb-4">
                {announcements.map((announcement) => (
                  <div key={announcement.id} className="flex-shrink-0 w-80 bg-white/10 backdrop-blur rounded-xl p-5">
                    <h3 className="font-semibold mb-2">{announcement.title}</h3>
                    <p className="text-sm text-blue-100 line-clamp-2">{announcement.content}</p>
                  </div>
                ))}
              </div>
            </div>
          </div>
        </section>
      )}

      {/* Upcoming Events */}
      <section className="py-16 bg-white dark:bg-gray-800">
        <div className="max-w-7xl mx-auto px-4">
          <div className="flex items-center justify-between mb-12">
            <h2 className="text-3xl font-bold text-gray-900 dark:text-white">Upcoming Events</h2>
            <Link to="/events"><Button variant="outline">View All Events</Button></Link>
          </div>
          {events.length > 0 ? (
            <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
              {events.map((event) => (
                <Card key={event.id} className="overflow-hidden hover:shadow-xl transition-all duration-300 group">
                  {event.image_url ? (
                    <img src={event.image_url} alt={event.title} className="w-full h-48 object-cover group-hover:scale-105 transition-transform duration-300" />
                  ) : (
                    <div className="w-full h-48 bg-gradient-to-br from-blue-600 to-green-600 flex items-center justify-center text-white text-6xl font-bold">{new Date(event.event_date).getDate()}</div>
                  )}
                  <div className="p-6">
                    <div className="flex items-center text-sm text-gray-500 dark:text-gray-400 mb-2">
                      <Calendar className="h-4 w-4 mr-2" />
                      {new Date(event.event_date).toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' })}
                      {event.start_time && <><Clock className="h-4 w-4 ml-4 mr-2" />{event.start_time}</>}
                    </div>
                    <h3 className="text-lg font-semibold text-gray-900 dark:text-white mb-2">{event.title}</h3>
                    {event.location && <div className="flex items-center text-sm text-gray-500 dark:text-gray-400"><MapPin className="h-4 w-4 mr-2" />{event.location}</div>}
                  </div>
                </Card>
              ))}
            </div>
          ) : (
            <div className="text-center py-12 text-gray-500 dark:text-gray-400">No upcoming events at this time.</div>
          )}
        </div>
      </section>

      {/* Recent Sermons */}
      <section className="py-16 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-7xl mx-auto px-4">
          <div className="flex items-center justify-between mb-12">
            <h2 className="text-3xl font-bold text-gray-900 dark:text-white">Recent Sermons</h2>
            <Link to="/sermons"><Button variant="outline">View All Sermons</Button></Link>
          </div>
          {sermons.length > 0 ? (
            <div className="grid md:grid-cols-2 lg:grid-cols-4 gap-6">
              {sermons.map((sermon) => (
                <Card key={sermon.id} className="overflow-hidden hover:shadow-xl transition-all duration-300">
                  <div className="relative">
                    {sermon.video_url ? (
                      <div className="relative aspect-video bg-gray-200 dark:bg-gray-700 flex items-center justify-center">
                        {sermon.thumbnail_url ? (
                          <img src={sermon.thumbnail_url} alt={sermon.title} className="w-full h-full object-cover" />
                        ) : (
                          <div className="w-full h-full bg-gradient-to-br from-blue-600 to-green-600" />
                        )}
                        <div className="absolute inset-0 flex items-center justify-center bg-black/30">
                          <div className="w-16 h-16 rounded-full bg-white/90 flex items-center justify-center"><Play className="h-8 w-8 text-blue-600 ml-1" /></div>
                        </div>
                      </div>
                    ) : (
                      <div className="h-24 bg-gradient-to-br from-blue-600 to-green-600 flex items-center justify-center"><Mic className="h-8 w-8 text-white" /></div>
                    )}
                  </div>
                  <div className="p-4">
                    <p className="text-xs text-gray-500 dark:text-gray-400 mb-1">{new Date(sermon.sermon_date).toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' })}</p>
                    <h3 className="font-semibold text-gray-900 dark:text-white line-clamp-2 mb-2">{sermon.title}</h3>
                    <p className="text-sm text-blue-600 dark:text-blue-400">{sermon.speaker}</p>
                  </div>
                </Card>
              ))}
            </div>
          ) : (
            <div className="text-center py-12 text-gray-500 dark:text-gray-400">No sermons available yet.</div>
          )}
        </div>
      </section>

      {/* Call to Action */}
      <section className="py-20 bg-gradient-to-br from-blue-600 via-blue-700 to-green-600 text-white">
        <div className="max-w-4xl mx-auto px-4 text-center">
          <h2 className="text-3xl md:text-4xl font-bold mb-6">Experience God's Love With Us</h2>
          <p className="text-lg text-blue-100 mb-8 max-w-2xl mx-auto">Whether you're seeking answers, looking for community, or wanting to deepen your faith, there's a place for you at Defence Word Ministry.</p>
          <div className="flex flex-col sm:flex-row items-center justify-center space-y-4 sm:space-y-0 sm:space-x-4">
            <Link to="/register"><Button size="lg" className="bg-white text-blue-600 hover:bg-blue-50 w-full sm:w-auto">Become a Member</Button></Link>
            <Link to="/contact"><Button size="lg" variant="outline" className="border-white text-white hover:bg-white/10 w-full sm:w-auto">Get in Touch</Button></Link>
          </div>
        </div>
      </section>
    </div>
  );
}

```

## `./src/pages/RegistrationPage.tsx`

```tsx
import React, { useState, useEffect, useRef, useCallback } from 'react';
import { supabase, Department, calculateAge, calculateFellowship } from '../lib/supabase';
import { useTheme } from '../contexts/ThemeContext';
import { Button, Card, Input, Select, TextArea, FileUpload, LoadingSpinner } from '../components/ui';
import { UserPlus, Check, Download, Printer } from 'lucide-react';

interface FormData {
  full_name: string;
  date_of_birth: string;
  gender: 'Male' | 'Female';
  phone: string;
  email: string;
  address: string;
  occupation: string;
  department_id: string;
  referral_source: string;
  referral_name: string;
  prayer_request: string;
}

export function RegistrationPage() {
  const { settings } = useTheme();
  const [formData, setFormData] = useState<FormData>({
    full_name: '',
    date_of_birth: '',
    gender: 'Male',
    phone: '',
    email: '',
    address: '',
    occupation: '',
    department_id: '',
    referral_source: '',
    referral_name: '',
    prayer_request: ''
  });
  const [photoFile, setPhotoFile] = useState<File | null>(null);
  const [photoPreview, setPhotoPreview] = useState<string>('');
  const [departments, setDepartments] = useState<Department[]>([]);
  const [loading, setLoading] = useState(false);
  const [submitSuccess, setSubmitSuccess] = useState(false);
  const [memberData, setMemberData] = useState<Record<string, unknown> | null>(null);
  const [errors, setErrors] = useState<Record<string, string>>({});

  const cardRef = useRef<HTMLDivElement>(null);

  useEffect(() => {
    supabase.from('departments').select('*').order('name').then(({ data }) => {
      if (data) setDepartments(data);
    });
  }, []);

  const handleChange = (e: React.ChangeEvent<HTMLInputElement | HTMLSelectElement | HTMLTextAreaElement>) => {
    const { name, value } = e.target;
    setFormData(prev => ({ ...prev, [name]: value }));
    if (errors[name]) setErrors(prev => ({ ...prev, [name]: '' }));
  };

  const handlePhotoSelect = (file: File) => {
    setPhotoFile(file);
    const reader = new FileReader();
    reader.onload = (e) => setPhotoPreview(e.target?.result as string);
    reader.readAsDataURL(file);
  };

  const validateForm = (): boolean => {
    const newErrors: Record<string, string> = {};
    if (!formData.full_name.trim()) newErrors.full_name = 'Full name is required';
    if (!formData.date_of_birth) newErrors.date_of_birth = 'Date of birth is required';
    if (!formData.gender) newErrors.gender = 'Gender is required';
    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e: React.FormEvent) => {
    e.preventDefault();
    if (!validateForm()) return;

    setLoading(true);
    try {
      let photoUrl = '';
      if (photoFile) {
        const fileName = `members/${Date.now()}_${photoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, photoFile);
        if (uploadError) {
          console.warn('Photo upload failed, continuing without photo:', uploadError);
        } else if (uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          photoUrl = urlData.publicUrl;
        }
      }

      const fellowship = calculateFellowship(formData.date_of_birth, formData.gender);
      const age = calculateAge(formData.date_of_birth);

      // Prepare member data - convert empty department_id to null
      const memberData = {
        full_name: formData.full_name,
        date_of_birth: formData.date_of_birth,
        gender: formData.gender,
        phone: formData.phone || null,
        email: formData.email || null,
        address: formData.address || null,
        occupation: formData.occupation || null,
        department_id: formData.department_id || null,
        referral_source: formData.referral_source || null,
        referral_name: formData.referral_name || null,
        prayer_request: formData.prayer_request || null,
        photo_url: photoUrl || null,
        fellowship,
        status: 'pending' as const
      };

      const { data, error } = await supabase.from('members').insert(memberData).select().single();

      if (error) {
        console.error('Registration error:', error);
        throw new Error(error.message || 'Registration failed');
      }

      setMemberData({ ...data, age, fellowship });
      setSubmitSuccess(true);
    } catch (err: unknown) {
      console.error('Registration error:', err);
      const message = err instanceof Error ? err.message : 'Registration failed. Please try again.';
      alert(message);
    } finally {
      setLoading(false);
    }
  };

  const downloadCard = useCallback(() => {
    if (!cardRef.current) return;

    const printWindow = window.open('', '_blank');
    if (!printWindow) return;

    // 4x4 inches = 384px at 96dpi, but we'll use 400px for better quality
    const cardHtml = `
      <!DOCTYPE html>
      <html>
      <head>
        <title>Member Card - ${memberData?.member_id || formData.full_name}</title>
        <style>
          * { margin: 0; padding: 0; box-sizing: border-box; }
          body { font-family: Arial, sans-serif; }
          .card { width: 384px; height: 384px; border-radius: 16px; overflow: hidden; box-shadow: 0 4px 20px rgba(0,0,0,0.2); }
          .header { background: ${settings.primary_color}; padding: 16px; text-align: center; color: white; }
          .header h2 { font-size: 14px; margin: 0; }
          .header p { font-size: 10px; opacity: 0.9; margin-top: 4px; }
          .photo-container { padding: 16px; text-align: center; background: #f3f4f6; }
          .photo { width: 100px; height: 100px; border-radius: 50%; object-fit: cover; border: 3px solid white; box-shadow: 0 2px 10px rgba(0,0,0,0.1); background: #e5e7eb; }
          .photo-placeholder { width: 100px; height: 100px; border-radius: 50%; background: #e5e7eb; display: inline-flex; align-items: center; justify-content: center; font-size: 40px; color: #9ca3af; border: 3px solid white; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
          .info { padding: 16px; background: white; }
          .name { font-size: 16px; font-weight: bold; text-align: center; margin-bottom: 8px; color: #111827; }
          .id { text-align: center; color: #6b7280; font-size: 12px; margin-bottom: 12px; }
          .details { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }
          .detail-item { background: #f9fafb; padding: 8px; border-radius: 6px; }
          .detail-label { font-size: 9px; color: #9ca3af; }
          .detail-value { font-size: 11px; font-weight: 600; color: #374151; }
          .footer { background: ${settings.accent_color}; padding: 12px; text-align: center; color: white; font-size: 10px; }
          @media print { body { margin: 0; } .card { box-shadow: none; } }
        </style>
      </head>
      <body>
        <div class="card">
          <div class="header">
            <h2>${settings.church_name}</h2>
            <p>${settings.motto}</p>
          </div>
          <div class="photo-container">
            ${photoPreview || memberData?.photo_url
              ? `<img src="${memberData?.photo_url || photoPreview}" class="photo" alt="Photo" />`
              : `<div class="photo-placeholder">?</div>`
            }
          </div>
          <div class="info">
            <div class="name">${memberData?.full_name || formData.full_name}</div>
            <div class="id">ID: ${memberData?.member_id || 'Pending...'}</div>
            <div class="details">
              <div class="detail-item"><div class="detail-label">DOB</div><div class="detail-value">${memberData?.date_of_birth || formData.date_of_birth}</div></div>
              <div class="detail-item"><div class="detail-label">Age</div><div class="detail-value">${memberData?.age || calculateAge(formData.date_of_birth)} yrs</div></div>
              <div class="detail-item"><div class="detail-label">Fellowship</div><div class="detail-value">${memberData?.fellowship || calculateFellowship(formData.date_of_birth, formData.gender)}</div></div>
              <div class="detail-item"><div class="detail-label">Gender</div><div class="detail-value">${memberData?.gender || formData.gender}</div></div>
            </div>
          </div>
          <div class="footer">${settings.welcome_message}</div>
        </div>
      </body>
      </html>
    `;

    printWindow.document.write(cardHtml);
    printWindow.document.close();

    setTimeout(() => {
      printWindow.print();
    }, 500);
  }, [settings, memberData, photoPreview, formData]);

  const referralOptions = [
    { value: '', label: 'Select referral source' },
    { value: 'social_media', label: 'Social Media' },
    { value: 'flyer', label: 'Flyer/Poster' },
    { value: 'friend', label: 'Friend/Family' },
    { value: 'online', label: 'Online Search' },
    { value: 'event', label: 'Church Event' },
    { value: 'other', label: 'Other' }
  ];

  const departmentOptions = [
    { value: '', label: 'Select department (optional)' },
    ...departments.map(d => ({ value: d.id, label: d.name }))
  ];

  return (
    <div className="min-h-screen bg-gray-50 dark:bg-gray-900 py-16">
      <div className="max-w-6xl mx-auto px-4">
        {/* Header */}
        <div className="text-center mb-12">
          <div className="inline-flex items-center justify-center w-16 h-16 rounded-full bg-blue-100 dark:bg-blue-900/20 mb-4">
            <UserPlus className="h-8 w-8 text-blue-600" />
          </div>
          <h1 className="text-3xl md:text-4xl font-bold text-gray-900 dark:text-white mb-4">Join Our Church Family</h1>
          <p className="text-lg text-gray-600 dark:text-gray-400 max-w-2xl mx-auto">{settings.welcome_message}</p>
        </div>

        <div className="grid lg:grid-cols-2 gap-8">
          {/* Form */}
          <Card className="p-6 md:p-8">
            <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-6">Member Registration Form</h2>
            <form onSubmit={handleSubmit} className="space-y-5">
              <Input label="Full Name *" name="full_name" value={formData.full_name} onChange={handleChange} placeholder="Enter your full name" error={errors.full_name} />

              <div className="grid md:grid-cols-2 gap-4">
                <Input label="Date of Birth *" name="date_of_birth" type="date" value={formData.date_of_birth} onChange={handleChange} error={errors.date_of_birth} />
                <Select label="Gender *" name="gender" value={formData.gender} onChange={handleChange} options={[{ value: 'Male', label: 'Male' }, { value: 'Female', label: 'Female' }]} error={errors.gender} />
              </div>

              <div className="grid md:grid-cols-2 gap-4">
                <Input label="Phone Number" name="phone" value={formData.phone} onChange={handleChange} placeholder="+233..." />
                <Input label="Email" name="email" type="email" value={formData.email} onChange={handleChange} placeholder="your@email.com" />
              </div>

              <Input label="Occupation" name="occupation" value={formData.occupation} onChange={handleChange} placeholder="e.g., Teacher, Engineer, Student" />

              <Input label="Address" name="address" value={formData.address} onChange={handleChange} placeholder="Your address" />

              <Select label="Department" name="department_id" value={formData.department_id} onChange={handleChange} options={departmentOptions} />

              <Select label="How did you hear about us?" name="referral_source" value={formData.referral_source} onChange={handleChange} options={referralOptions} />

              {formData.referral_source === 'friend' && (
                <Input label="Who referred you?" name="referral_name" value={formData.referral_name} onChange={handleChange} placeholder="Enter their name" />
              )}

              <TextArea label="Prayer Request (Optional)" name="prayer_request" value={formData.prayer_request} onChange={handleChange} placeholder="Any prayer requests you'd like to share..." rows={3} />

              <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-6 text-center">
                <div className="w-32 h-32 mx-auto rounded-full overflow-hidden border-4 border-gray-200 dark:border-gray-700 bg-gray-100 dark:bg-gray-800">
                  {photoPreview ? (
                    <img src={photoPreview} alt="Preview" className="w-full h-full object-cover" />
                  ) : (
                    <div className="w-full h-full flex items-center justify-center text-gray-400 text-sm">Photo</div>
                  )}
                </div>
                <label className="mt-4 inline-block cursor-pointer">
                  <span className="text-sm text-blue-600 hover:underline">Click to upload photo</span>
                  <input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) handlePhotoSelect(file); }} />
                </label>
                <p className="text-xs text-gray-500 mt-1">Square photos work best</p>
              </div>

              <Button type="submit" size="lg" className="w-full" loading={loading}>Submit Registration</Button>
            </form>
          </Card>

          {/* Member Card Preview - 4x4 inches */}
          <div className="sticky top-24">
            <h2 className="text-xl font-semibold text-gray-900 dark:text-white mb-6">Your Membership Card Preview (4x4 inches)</h2>
            <Card className="p-6">
              <div
                ref={cardRef}
                className="w-[384px] h-[384px] mx-auto bg-white rounded-xl overflow-hidden shadow-2xl"
                style={{ aspectRatio: '1/1' }}
              >
                {/* Card Header */}
                <div className="p-3 text-white text-center" style={{ backgroundColor: settings.primary_color }}>
                  <h3 className="text-sm font-bold">{settings.church_name}</h3>
                  <p className="text-xs opacity-90">{settings.motto}</p>
                </div>

                {/* Photo Section */}
                <div className="p-4 flex justify-center bg-gray-50">
                  <div className="w-24 h-24 rounded-full overflow-hidden border-4 border-white shadow-lg bg-gray-200">
                    {photoPreview ? (
                      <img src={photoPreview} alt="Member" className="w-full h-full object-cover" />
                    ) : (
                      <div className="w-full h-full flex items-center justify-center text-gray-400 text-3xl">?</div>
                    )}
                  </div>
                </div>

                {/* Member Details */}
                <div className="px-4 pb-4 bg-white">
                  <div className="text-center mb-3">
                    <p className="font-bold text-base text-gray-900">{formData.full_name || 'Your Name'}</p>
                    <p className="text-gray-500 text-xs">{submitSuccess && memberData?.member_id ? `ID: ${memberData.member_id}` : 'ID: Pending...'}</p>
                  </div>
                  <div className="grid grid-cols-2 gap-2 text-xs">
                    <div className="bg-gray-50 rounded p-2">
                      <p className="text-gray-400 text-[10px]">DOB</p>
                      <p className="font-medium text-gray-900">{formData.date_of_birth || '---'}</p>
                    </div>
                    <div className="bg-gray-50 rounded p-2">
                      <p className="text-gray-400 text-[10px]">Age</p>
                      <p className="font-medium text-gray-900">{formData.date_of_birth ? `${calculateAge(formData.date_of_birth)} yrs` : '---'}</p>
                    </div>
                    <div className="bg-gray-50 rounded p-2">
                      <p className="text-gray-400 text-[10px]">Fellowship</p>
                      <p className="font-medium text-gray-900">{formData.date_of_birth && formData.gender ? calculateFellowship(formData.date_of_birth, formData.gender) : '---'}</p>
                    </div>
                    <div className="bg-gray-50 rounded p-2">
                      <p className="text-gray-400 text-[10px]">Gender</p>
                      <p className="font-medium text-gray-900">{formData.gender || '---'}</p>
                    </div>
                  </div>
                </div>

                {/* Card Footer */}
                <div className="px-3 py-2 text-white text-center text-[10px]" style={{ backgroundColor: settings.accent_color }}>
                  {settings.welcome_message}
                </div>
              </div>

              {submitSuccess && (
                <div className="mt-6">
                  <div className="flex items-center justify-center space-x-2 text-green-600 mb-4">
                    <Check className="h-5 w-5" />
                    <span className="font-medium">Registration Successful!</span>
                  </div>
                  <div className="flex flex-wrap justify-center gap-3">
                    <Button variant="outline" size="sm" onClick={downloadCard}>
                      <Download className="h-4 w-4 mr-2" /> Download
                    </Button>
                    <Button variant="outline" size="sm" onClick={downloadCard}>
                      <Printer className="h-4 w-4 mr-2" /> Print
                    </Button>
                  </div>
                  <p className="text-center text-sm text-gray-500 mt-4">Your membership is pending approval. You will receive a notification once approved.</p>
                </div>
              )}
            </Card>
          </div>
        </div>
      </div>
    </div>
  );
}

```

## `./src/pages/SermonsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, Sermon } from '../lib/supabase';
import { Card, LoadingSpinner, Input } from '../components/ui';
import { Mic, Play, Search, BookOpen } from 'lucide-react';

const isExpired = (endDate: string | null): boolean => {
  if (!endDate) return false;
  const expireDate = new Date(endDate);
  expireDate.setDate(expireDate.getDate() + 2);
  return new Date() > expireDate;
};

export function SermonsPage() {
  const [sermons, setSermons] = useState<Sermon[]>([]);
  const [loading, setLoading] = useState(true);
  const [searchTerm, setSearchTerm] = useState('');
  const [selectedSermon, setSelectedSermon] = useState<Sermon | null>(null);

  useEffect(() => {
    supabase.from('sermons').select('*').order('sermon_date', { ascending: false }).then(({ data }) => {
      if (data) setSermons(data.filter(s => !isExpired(s.end_date)));
      setLoading(false);
    });
  }, []);

  const filteredSermons = sermons.filter(sermon =>
    sermon.title.toLowerCase().includes(searchTerm.toLowerCase()) ||
    sermon.speaker.toLowerCase().includes(searchTerm.toLowerCase()) ||
    (sermon.scripture_reference?.toLowerCase() || '').includes(searchTerm.toLowerCase())
  );

  if (loading) return <div className="min-h-screen flex items-center justify-center"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="min-h-screen">
      <section className="relative py-20 bg-gradient-to-br from-blue-900 to-blue-700 text-white">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <Mic className="w-16 h-16 mx-auto mb-4 opacity-80" />
          <h1 className="text-4xl md:text-5xl font-bold mb-4">Sermons</h1>
          <p className="text-xl opacity-90">Listen to messages that will transform your life</p>
        </div>
      </section>

      <section className="py-6 bg-white dark:bg-gray-800 border-b border-gray-200 dark:border-gray-700">
        <div className="max-w-7xl mx-auto px-4">
          <div className="relative max-w-md">
            <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 h-5 w-5 text-gray-400" />
            <Input placeholder="Search sermons..." value={searchTerm} onChange={(e) => setSearchTerm(e.target.value)} className="pl-10" />
          </div>
        </div>
      </section>

      <section className="py-12 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-7xl mx-auto px-4">
          {filteredSermons.length > 0 ? (
            <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-8">
              {filteredSermons.map((sermon) => (
                <Card key={sermon.id} className="overflow-hidden hover:shadow-xl transition-all duration-300 group">
                  <div className="relative cursor-pointer" onClick={() => setSelectedSermon(sermon)}>
                    {sermon.thumbnail_url ? (
                      <img src={sermon.thumbnail_url} alt={sermon.title} className="w-full h-48 object-cover" />
                    ) : sermon.video_url ? (
                      <div className="w-full h-48 bg-gradient-to-br from-blue-600 to-green-600 flex items-center justify-center"><Play className="h-16 w-16 text-white opacity-80" /></div>
                    ) : (
                      <div className="w-full h-48 bg-gradient-to-br from-gray-600 to-gray-800 flex items-center justify-center"><Mic className="h-16 w-16 text-white opacity-80" /></div>
                    )}
                    {sermon.video_url && (
                      <div className="absolute inset-0 bg-black/30 flex items-center justify-center opacity-0 group-hover:opacity-100 transition-opacity">
                        <div className="w-16 h-16 rounded-full bg-white/90 flex items-center justify-center"><Play className="h-8 w-8 text-blue-600 ml-1" /></div>
                      </div>
                    )}
                  </div>
                  <div className="p-6">
                    <p className="text-sm text-gray-500 dark:text-gray-400 mb-2">{new Date(sermon.sermon_date).toLocaleDateString('en-US', { month: 'long', day: 'numeric', year: 'numeric' })}</p>
                    <h3 className="text-lg font-bold text-gray-900 dark:text-white mb-2 line-clamp-2">{sermon.title}</h3>
                    <p className="text-blue-600 dark:text-blue-400 font-medium mb-2">{sermon.speaker}</p>
                    {sermon.scripture_reference && <div className="flex items-center text-sm text-gray-500 dark:text-gray-400"><BookOpen className="h-4 w-4 mr-2" />{sermon.scripture_reference}</div>}
                    {sermon.description && <p className="mt-3 text-gray-600 dark:text-gray-400 text-sm line-clamp-2">{sermon.description}</p>}
                  </div>
                </Card>
              ))}
            </div>
          ) : (
            <div className="text-center py-16">
              <Mic className="h-16 w-16 mx-auto text-gray-300 dark:text-gray-600 mb-4" />
              <h3 className="text-xl font-medium text-gray-900 dark:text-white mb-2">No Sermons Found</h3>
              <p className="text-gray-500 dark:text-gray-400">{searchTerm ? 'Try adjusting your search terms.' : 'Sermons will be available soon.'}</p>
            </div>
          )}
        </div>
      </section>

      {selectedSermon?.video_url && (
        <div className="fixed inset-0 z-50 bg-black/80 flex items-center justify-center p-4" onClick={() => setSelectedSermon(null)}>
          <div className="w-full max-w-4xl bg-gray-900 rounded-xl overflow-hidden" onClick={e => e.stopPropagation()}>
            <div className="aspect-video">
              {selectedSermon.video_url.includes('youtube') || selectedSermon.video_url.includes('vimeo') ? (
                <iframe className="w-full h-full" src={selectedSermon.video_url} allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowFullScreen />
              ) : (
                <video className="w-full h-full" src={selectedSermon.video_url} controls autoPlay />
              )}
            </div>
            <div className="p-4 text-white">
              <h3 className="font-bold mb-1">{selectedSermon.title}</h3>
              <p className="text-sm text-gray-400">{selectedSermon.speaker}</p>
              <button onClick={() => setSelectedSermon(null)} className="mt-4 w-full py-2 bg-gray-700 rounded-lg hover:bg-gray-600 transition-colors">Close</button>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

```

## `./src/pages/ServicesPage.tsx`

```tsx
import React from 'react';
import { useTheme } from '../contexts/ThemeContext';
import { Card } from '../components/ui';
import { Clock, Users, BookOpen, Music, Heart } from 'lucide-react';

export function ServicesPage() {
  const { settings } = useTheme();

  const defaultServices = [
    {
      name: 'First Sunday Service',
      day: 'Sunday',
      time: '7:00 AM',
      description: 'Early morning worship service',
      icon: BookOpen
    },
    {
      name: 'Second Sunday Service',
      day: 'Sunday',
      time: '9:30 AM',
      description: 'Main worship service with Sunday School',
      icon: Users
    },
    {
      name: 'Midweek Service',
      day: 'Wednesday',
      time: '6:00 PM',
      description: 'Bible study and prayer meeting',
      icon: BookOpen
    },
    {
      name: 'Prayer & Fasting',
      day: 'Friday',
      time: '6:00 PM',
      description: 'Corporate prayer and intercession',
      icon: Heart
    },
    {
      name: 'Youth Service',
      day: 'Saturday',
      time: '4:00 PM',
      description: 'Service for young people',
      icon: Users
    },
    {
      name: 'Choir Practice',
      day: 'Thursday',
      time: '5:00 PM',
      description: 'Rehearsal for the choir ministration',
      icon: Music
    }
  ];

  const services = (settings.service_times?.length > 0
    ? settings.service_times.map((s, i) => ({
        name: s.name || `Service ${i + 1}`,
        day: s.day,
        time: s.time,
        description: '',
        icon: Clock
      }))
    : defaultServices) as typeof defaultServices;

  return (
    <div className="min-h-screen">
      {/* Hero Section */}
      <section className="relative py-20 bg-gradient-to-br from-blue-900 to-blue-700 text-white">
        <div className="max-w-7xl mx-auto px-4 text-center">
          <Clock className="w-16 h-16 mx-auto mb-4 opacity-80" />
          <h1 className="text-4xl md:text-5xl font-bold mb-4">Service Times</h1>
          <p className="text-xl opacity-90">Join us for worship and fellowship</p>
        </div>
      </section>

      {/* Services Grid */}
      <section className="py-16 bg-gray-50 dark:bg-gray-900">
        <div className="max-w-7xl mx-auto px-4">
          <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
            {services.map((service, index) => (
              <Card key={index} className="p-6 hover:shadow-xl transition-shadow">
                <div className="flex items-start space-x-4">
                  <div className="w-14 h-14 rounded-full bg-blue-100 dark:bg-blue-900/20 flex items-center justify-center flex-shrink-0">
                    <service.icon className="h-7 w-7 text-blue-600" />
                  </div>
                  <div>
                    <h3 className="text-lg font-bold text-gray-900 dark:text-white mb-1">
                      {service.name}
                    </h3>
                    <p className="text-blue-600 dark:text-blue-400 font-medium">
                      {service.day} at {service.time}
                    </p>
                    {service.description && (
                      <p className="text-gray-600 dark:text-gray-400 text-sm mt-2">
                        {service.description}
                      </p>
                    )}
                  </div>
                </div>
              </Card>
            ))}
          </div>
        </div>
      </section>

      {/* What to Expect */}
      <section className="py-16 bg-white dark:bg-gray-800">
        <div className="max-w-4xl mx-auto px-4">
          <h2 className="text-3xl font-bold text-center text-gray-900 dark:text-white mb-12">
            What to Expect
          </h2>
          <div className="space-y-8">
            <div className="flex items-start space-x-4">
              <div className="w-10 h-10 rounded-full bg-green-100 dark:bg-green-900/20 flex items-center justify-center flex-shrink-0">
                <Music className="h-5 w-5 text-green-600" />
              </div>
              <div>
                <h3 className="font-bold text-gray-900 dark:text-white mb-2">Inspiring Worship</h3>
                <p className="text-gray-600 dark:text-gray-400">
                  Experience heartfelt praise and worship led by our talented music team.
                  We blend contemporary songs with traditional hymns.
                </p>
              </div>
            </div>

            <div className="flex items-start space-x-4">
              <div className="w-10 h-10 rounded-full bg-blue-100 dark:bg-blue-900/20 flex items-center justify-center flex-shrink-0">
                <BookOpen className="h-5 w-5 text-blue-600" />
              </div>
              <div>
                <h3 className="font-bold text-gray-900 dark:text-white mb-2">Powerful Teaching</h3>
                <p className="text-gray-600 dark:text-gray-400">
                  Receive practical, Bible-based teachings that will transform your life
                  and help you grow in your faith.
                </p>
              </div>
            </div>

            <div className="flex items-start space-x-4">
              <div className="w-10 h-10 rounded-full bg-purple-100 dark:bg-purple-900/20 flex items-center justify-center flex-shrink-0">
                <Users className="h-5 w-5 text-purple-600" />
              </div>
              <div>
                <h3 className="font-bold text-gray-900 dark:text-white mb-2">Warm Fellowship</h3>
                <p className="text-gray-600 dark:text-gray-400">
                  Connect with friendly people from all walks of life. You'll find a
                  welcoming community ready to embrace you.
                </p>
              </div>
            </div>

            <div className="flex items-start space-x-4">
              <div className="w-10 h-10 rounded-full bg-red-100 dark:bg-red-900/20 flex items-center justify-center flex-shrink-0">
                <Heart className="h-5 w-5 text-red-600" />
              </div>
              <div>
                <h3 className="font-bold text-gray-900 dark:text-white mb-2">Prayer & Ministry</h3>
                <p className="text-gray-600 dark:text-gray-400">
                  Our prayer team is available to pray with you and support you in
                  whatever you may be going through.
                </p>
              </div>
            </div>
          </div>
        </div>
      </section>
    </div>
  );
}

```

## `./src/pages/admin/AdvertsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, Advert } from '../../lib/supabase';
import { Card, Button, Input, TextArea, Select, Modal, Badge, LoadingSpinner } from '../../components/ui';
import { Megaphone, Plus, Edit, Trash2, Video, Upload } from 'lucide-react';

export function AdvertsPage() {
  const [adverts, setAdverts] = useState<Advert[]>([]);
  const [loading, setLoading] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [editItem, setEditItem] = useState<Advert | null>(null);
  const [saving, setSaving] = useState(false);
  const [imageFile, setImageFile] = useState<File | null>(null);
  const [videoFile, setVideoFile] = useState<File | null>(null);
  const [imagePreview, setImagePreview] = useState('');
  const [videoPreview, setVideoPreview] = useState('');

  const [formData, setFormData] = useState({
    title: '',
    description: '',
    company_name: '',
    contact_info: '',
    website_url: '',
    social_url: '',
    video_url: '',
    image_size: 'medium' as 'small' | 'medium' | 'large',
    active: true,
    end_date: ''
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('adverts').select('*').order('created_at', { ascending: false });
    setAdverts(data || []);
    setLoading(false);
  };

  const openModal = (item: Advert | null) => {
    if (item) {
      setFormData({
        title: item.title,
        description: item.description || '',
        company_name: item.company_name || '',
        contact_info: item.contact_info || '',
        website_url: item.website_url || '',
        social_url: item.social_url || '',
        video_url: item.video_url || '',
        image_size: item.image_size,
        active: item.active,
        end_date: item.end_date || ''
      });
      setImagePreview(item.image_url || '');
      setVideoPreview(item.video_url || '');
      setEditItem(item);
    } else {
      setFormData({ title: '', description: '', company_name: '', contact_info: '', website_url: '', social_url: '', video_url: '', image_size: 'medium', active: true, end_date: '' });
      setImagePreview('');
      setVideoPreview('');
      setEditItem(null);
    }
    setImageFile(null);
    setVideoFile(null);
    setShowModal(true);
  };

  const handleSave = async () => {
    if (!formData.title) { alert('Title is required'); return; }
    setSaving(true);

    try {
      let imageUrl = editItem?.image_url || '';
      let videoUrl = formData.video_url || '';

      if (imageFile) {
        const fileName = `adverts/${Date.now()}_${imageFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, imageFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          imageUrl = urlData.publicUrl;
        }
      }

      if (videoFile) {
        const fileName = `videos/adverts/${Date.now()}_${videoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, videoFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          videoUrl = urlData.publicUrl;
        }
      }

      const dataToSave = {
        title: formData.title,
        description: formData.description || null,
        company_name: formData.company_name || null,
        contact_info: formData.contact_info || null,
        website_url: formData.website_url || null,
        social_url: formData.social_url || null,
        image_url: imageUrl || null,
        video_url: videoUrl || null,
        image_size: formData.image_size,
        active: formData.active,
        end_date: formData.end_date || null
      };

      let error;
      if (editItem) {
        ({ error } = await supabase.from('adverts').update({ ...dataToSave, updated_at: new Date().toISOString() }).eq('id', editItem.id));
      } else {
        ({ error } = await supabase.from('adverts').insert(dataToSave));
      }

      if (error) throw error;

      setShowModal(false);
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const handleDelete = async (item: Advert) => {
    if (!confirm('Delete this advert?')) return;
    await supabase.from('deleted_records').insert({ table_name: 'adverts', record_id: item.id, record_data: item as unknown as Record<string, unknown> });
    await supabase.from('adverts').delete().eq('id', item.id);
    fetchData();
  };

  const toggleActive = async (item: Advert) => {
    await supabase.from('adverts').update({ active: !item.active }).eq('id', item.id);
    fetchData();
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Adverts</h1>
          <p className="text-gray-500 text-sm mt-1">Manage promotional content</p>
        </div>
        <Button onClick={() => openModal(null)}><Plus className="h-4 w-4 mr-2" /> Add Advert</Button>
      </div>

      <Card>
        <div className="divide-y divide-gray-200 dark:divide-gray-700">
          {adverts.length > 0 ? adverts.map((item) => (
            <div key={item.id} className="p-4 flex items-center justify-between hover:bg-gray-50 dark:hover:bg-gray-800">
              <div className="flex items-center space-x-4">
                {item.image_url ? (
                  <img src={item.image_url} alt="" className="w-20 h-16 rounded-lg object-cover" />
                ) : (
                  <div className="w-20 h-16 rounded-lg bg-purple-100 dark:bg-purple-900/20 flex items-center justify-center"><Megaphone className="h-8 w-8 text-purple-600" /></div>
                )}
                <div>
                  <h3 className="font-medium text-gray-900 dark:text-white">{item.title}</h3>
                  <p className="text-sm text-gray-500">{item.company_name || 'No company'}</p>
                  <div className="flex gap-2 items-center mt-1">
                    <Badge variant={item.image_size === 'large' ? 'success' : item.image_size === 'medium' ? 'info' : 'default'}>{item.image_size}</Badge>
                    {item.video_url && <Video className="h-3 w-3 text-purple-500" />}
                    {item.end_date && <span className="text-xs text-orange-500">Expires: {new Date(item.end_date).toLocaleDateString()}</span>}
                  </div>
                </div>
              </div>
              <div className="flex items-center space-x-4">
                <Badge variant={item.active ? 'success' : 'default'}>{item.active ? 'Active' : 'Inactive'}</Badge>
                <button onClick={() => toggleActive(item)} className="text-sm text-blue-600 hover:underline">{item.active ? 'Deactivate' : 'Activate'}</button>
                <button onClick={() => openModal(item)} className="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><Edit className="h-4 w-4 text-gray-600" /></button>
                <button onClick={() => handleDelete(item)} className="p-2 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/20"><Trash2 className="h-4 w-4 text-red-600" /></button>
              </div>
            </div>
          )) : <div className="p-8 text-center text-gray-500">No adverts yet. Click "Add Advert" to create one.</div>}
        </div>
      </Card>

      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title={editItem ? 'Edit Advert' : 'New Advert'} size="lg">
        <div className="space-y-5">
          <Input label="Title *" value={formData.title} onChange={(e) => setFormData(prev => ({ ...prev, title: e.target.value }))} />
          <TextArea label="Description" value={formData.description} onChange={(e) => setFormData(prev => ({ ...prev, description: e.target.value }))} rows={2} />
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Company Name" value={formData.company_name} onChange={(e) => setFormData(prev => ({ ...prev, company_name: e.target.value }))} />
            <Input label="Contact Info" value={formData.contact_info} onChange={(e) => setFormData(prev => ({ ...prev, contact_info: e.target.value }))} />
          </div>
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Website URL" value={formData.website_url} onChange={(e) => setFormData(prev => ({ ...prev, website_url: e.target.value }))} placeholder="https://" />
            <Input label="Social URL" value={formData.social_url} onChange={(e) => setFormData(prev => ({ ...prev, social_url: e.target.value }))} />
          </div>
          <div className="grid md:grid-cols-2 gap-4">
            <Select label="Image Size" value={formData.image_size} onChange={(e) => setFormData(prev => ({ ...prev, image_size: e.target.value as 'small' | 'medium' | 'large' }))} options={[{ value: 'small', label: 'Small' }, { value: 'medium', label: 'Medium' }, { value: 'large', label: 'Large' }]} />
            <Input label="End Date (Auto-remove)" type="date" value={formData.end_date} onChange={(e) => setFormData(prev => ({ ...prev, end_date: e.target.value }))} />
          </div>
          <div className="flex items-center space-x-2">
            <input type="checkbox" id="active" checked={formData.active} onChange={(e) => setFormData(prev => ({ ...prev, active: e.target.checked }))} className="rounded" />
            <label htmlFor="active" className="text-sm text-gray-700 dark:text-gray-300">Active</label>
          </div>

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Image</p>
            {imagePreview && <img src={imagePreview} alt="" className="max-w-full rounded-lg mb-2 max-h-32" />}
            <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
              <Upload className="h-4 w-4 mr-2" /> Upload Image
              <input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setImageFile(file); const reader = new FileReader(); reader.onload = (ev) => setImagePreview(ev.target?.result as string); reader.readAsDataURL(file); } }} />
            </label>
          </div>

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Video</p>
            {videoPreview && videoPreview.startsWith('blob:') && <video src={videoPreview} controls className="max-w-full rounded-lg mb-2 max-h-32" />}
            <div className="flex gap-2 flex-wrap">
              <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700">
                <Video className="h-4 w-4 mr-2" /> Upload Video
                <input type="file" accept="video/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setVideoFile(file); setVideoPreview(URL.createObjectURL(file)); } }} />
              </label>
              <Input value={formData.video_url} onChange={(e) => { setFormData(prev => ({ ...prev, video_url: e.target.value })); setVideoPreview(e.target.value); }} placeholder="Or YouTube URL" className="flex-1 min-w-48" />
            </div>
          </div>

          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>{editItem ? 'Save Changes' : 'Add Advert'}</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/AnnouncementsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, Announcement } from '../../lib/supabase';
import { Card, Button, Input, TextArea, Modal, Badge, LoadingSpinner } from '../../components/ui';
import { Megaphone, Plus, Edit, Trash2, Video, Upload } from 'lucide-react';

export function AnnouncementsAdminPage() {
  const [announcements, setAnnouncements] = useState<Announcement[]>([]);
  const [loading, setLoading] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [editItem, setEditItem] = useState<Announcement | null>(null);
  const [saving, setSaving] = useState(false);
  const [imageFile, setImageFile] = useState<File | null>(null);
  const [videoFile, setVideoFile] = useState<File | null>(null);
  const [imagePreview, setImagePreview] = useState('');
  const [videoPreview, setVideoPreview] = useState('');

  const [formData, setFormData] = useState({
    title: '',
    content: '',
    link_url: '',
    video_url: '',
    priority: 0,
    active: true,
    start_date: '',
    end_date: ''
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('announcements').select('*').order('priority', { ascending: false });
    setAnnouncements(data || []);
    setLoading(false);
  };

  const openModal = (item: Announcement | null) => {
    if (item) {
      setFormData({
        title: item.title,
        content: item.content,
        link_url: item.link_url || '',
        video_url: item.video_url || '',
        priority: item.priority,
        active: item.active,
        start_date: item.start_date || '',
        end_date: item.end_date || ''
      });
      setImagePreview(item.image_url || '');
      setVideoPreview(item.video_url || '');
      setEditItem(item);
    } else {
      setFormData({ title: '', content: '', link_url: '', video_url: '', priority: 0, active: true, start_date: '', end_date: '' });
      setImagePreview('');
      setVideoPreview('');
      setEditItem(null);
    }
    setImageFile(null);
    setVideoFile(null);
    setShowModal(true);
  };

  const handleSave = async () => {
    if (!formData.title || !formData.content) { alert('Title and content are required'); return; }
    setSaving(true);

    try {
      let imageUrl = editItem?.image_url || '';
      let videoUrl = formData.video_url || '';

      if (imageFile) {
        const fileName = `announcements/${Date.now()}_${imageFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, imageFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          imageUrl = urlData.publicUrl;
        }
      }

      if (videoFile) {
        const fileName = `videos/announcements/${Date.now()}_${videoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, videoFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          videoUrl = urlData.publicUrl;
        }
      }

      const dataToSave = {
        title: formData.title,
        content: formData.content,
        link_url: formData.link_url || null,
        image_url: imageUrl || null,
        video_url: videoUrl || null,
        priority: formData.priority,
        active: formData.active,
        start_date: formData.start_date || null,
        end_date: formData.end_date || null
      };

      let error;
      if (editItem) {
        ({ error } = await supabase.from('announcements').update({ ...dataToSave, updated_at: new Date().toISOString() }).eq('id', editItem.id));
      } else {
        ({ error } = await supabase.from('announcements').insert(dataToSave));
      }

      if (error) throw error;

      setShowModal(false);
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const handleDelete = async (item: Announcement) => {
    if (!confirm('Delete this announcement?')) return;
    await supabase.from('deleted_records').insert({ table_name: 'announcements', record_id: item.id, record_data: item as unknown as Record<string, unknown> });
    await supabase.from('announcements').delete().eq('id', item.id);
    fetchData();
  };

  const toggleActive = async (item: Announcement) => {
    await supabase.from('announcements').update({ active: !item.active }).eq('id', item.id);
    fetchData();
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Announcements</h1>
          <p className="text-gray-500 text-sm mt-1">Manage church announcements</p>
        </div>
        <Button onClick={() => openModal(null)}><Plus className="h-4 w-4 mr-2" /> Add Announcement</Button>
      </div>

      <Card>
        <div className="divide-y divide-gray-200 dark:divide-gray-700">
          {announcements.length > 0 ? announcements.map((item) => (
            <div key={item.id} className="p-4 flex items-center justify-between hover:bg-gray-50 dark:hover:bg-gray-800">
              <div className="flex items-center space-x-4">
                {item.image_url ? (
                  <img src={item.image_url} alt="" className="w-16 h-12 rounded-lg object-cover" />
                ) : (
                  <div className="w-16 h-12 rounded-lg bg-blue-100 dark:bg-blue-900/20 flex items-center justify-center"><Megaphone className="h-6 w-6 text-blue-600" /></div>
                )}
                <div>
                  <h3 className="font-medium text-gray-900 dark:text-white">{item.title}</h3>
                  <p className="text-sm text-gray-500 line-clamp-1">{item.content}</p>
                  <div className="flex gap-2 items-center mt-1">
                    {item.video_url && <Video className="h-3 w-3 text-purple-500" />}
                    {item.end_date && <span className="text-xs text-orange-500">Expires: {new Date(item.end_date).toLocaleDateString()}</span>}
                  </div>
                </div>
              </div>
              <div className="flex items-center space-x-4">
                <Badge variant={item.active ? 'success' : 'default'}>{item.active ? 'Active' : 'Inactive'}</Badge>
                <button onClick={() => toggleActive(item)} className="text-sm text-blue-600 hover:underline">{item.active ? 'Deactivate' : 'Activate'}</button>
                <button onClick={() => openModal(item)} className="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><Edit className="h-4 w-4 text-gray-600" /></button>
                <button onClick={() => handleDelete(item)} className="p-2 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/20"><Trash2 className="h-4 w-4 text-red-600" /></button>
              </div>
            </div>
          )) : <div className="p-8 text-center text-gray-500">No announcements yet</div>}
        </div>
      </Card>

      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title={editItem ? 'Edit Announcement' : 'New Announcement'} size="lg">
        <div className="space-y-5">
          <Input label="Title *" value={formData.title} onChange={(e) => setFormData(prev => ({ ...prev, title: e.target.value }))} />
          <TextArea label="Content *" value={formData.content} onChange={(e) => setFormData(prev => ({ ...prev, content: e.target.value }))} rows={3} />
          <Input label="Link URL" value={formData.link_url} onChange={(e) => setFormData(prev => ({ ...prev, link_url: e.target.value }))} placeholder="Optional link" />
          <Input label="Priority" type="number" value={formData.priority.toString()} onChange={(e) => setFormData(prev => ({ ...prev, priority: parseInt(e.target.value) || 0 }))} helpText="Higher priority appears first" />
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Start Date" type="date" value={formData.start_date} onChange={(e) => setFormData(prev => ({ ...prev, start_date: e.target.value }))} />
            <Input label="End Date (Auto-remove)" type="date" value={formData.end_date} onChange={(e) => setFormData(prev => ({ ...prev, end_date: e.target.value }))} />
          </div>
          <div className="flex items-center space-x-2">
            <input type="checkbox" id="active" checked={formData.active} onChange={(e) => setFormData(prev => ({ ...prev, active: e.target.checked }))} className="rounded" />
            <label htmlFor="active" className="text-sm text-gray-700 dark:text-gray-300">Active</label>
          </div>

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Image</p>
            {imagePreview && <img src={imagePreview} alt="" className="max-w-full rounded-lg mb-2 max-h-32" />}
            <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
              <Upload className="h-4 w-4 mr-2" /> Upload Image
              <input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setImageFile(file); const reader = new FileReader(); reader.onload = (ev) => setImagePreview(ev.target?.result as string); reader.readAsDataURL(file); } }} />
            </label>
          </div>

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Video</p>
            {videoPreview && videoPreview.startsWith('blob:') && <video src={videoPreview} controls className="max-w-full rounded-lg mb-2 max-h-32" />}
            <div className="flex gap-2 flex-wrap">
              <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700">
                <Video className="h-4 w-4 mr-2" /> Upload Video
                <input type="file" accept="video/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setVideoFile(file); setVideoPreview(URL.createObjectURL(file)); } }} />
              </label>
              <Input value={formData.video_url} onChange={(e) => { setFormData(prev => ({ ...prev, video_url: e.target.value })); setVideoPreview(e.target.value); }} placeholder="Or YouTube URL" className="flex-1 min-w-48" />
            </div>
          </div>

          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>{editItem ? 'Save Changes' : 'Add Announcement'}</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/AttendancePage.tsx`

```tsx
import React, { useState, useEffect, useMemo, useCallback } from 'react';
import { supabase, Member, Attendance } from '../../lib/supabase';
import { Card, Button, Input, Badge, LoadingSpinner, Select } from '../../components/ui';
import { Clock, Calendar, Check, X, AlertCircle, Users, Download, Save } from 'lucide-react';

export function AttendancePage() {
  const [members, setMembers] = useState<Member[]>([]);
  const [attendance, setAttendance] = useState<Attendance[]>([]);
  const [loading, setLoading] = useState(true);
  const [selectedDate, setSelectedDate] = useState(new Date().toISOString().split('T')[0]);
  const [serviceType, setServiceType] = useState('Sunday Service');
  const [attendanceStates, setAttendanceStates] = useState<Record<string, 'present' | 'absent' | 'late' | null>>({});
  const [saving, setSaving] = useState(false);

  useEffect(() => {
    fetchData();
  }, [selectedDate, serviceType]);

  const fetchData = async () => {
    setLoading(true);
    const [{ data: membersData }, { data: attendanceData }] = await Promise.all([
      supabase.from('members').select('*').eq('status', 'approved').order('full_name'),
      supabase.from('attendance').select('*').eq('service_date', selectedDate).eq('service_type', serviceType)
    ]);

    setMembers(membersData || []);
    setAttendance(attendanceData || []);

    // Initialize attendance states
    const states: Record<string, 'present' | 'absent' | 'late' | null> = {};
    (attendanceData || []).forEach((att) => {
      states[att.member_id] = att.status as 'present' | 'absent' | 'late';
    });
    setAttendanceStates(states);
    setLoading(false);
  };

  const handleMarkAttendance = async (memberId: string, status: 'present' | 'absent' | 'late') => {
    // Check if already marked for this date/service
    const existing = attendance.find(a => a.member_id === memberId);

    if (existing) {
      // Don't allow updates once marked
      return;
    }

    setAttendanceStates(prev => ({ ...prev, [memberId]: status }));
  };

  const handleSaveAttendance = async () => {
    setSaving(true);
    const records = Object.entries(attendanceStates)
      .filter(([_, status]) => status !== null)
      .map(([memberId, status]) => ({
        member_id: memberId,
        service_date: selectedDate,
        service_type: serviceType,
        status
      }));

    if (records.length > 0) {
      await supabase.from('attendance').insert(records);
    }

    setSaving(false);
    fetchData();
  };

  const stats = useMemo(() => {
    const present = Object.values(attendanceStates).filter(s => s === 'present').length || attendance.filter(a => a.status === 'present').length;
    const absent = Object.values(attendanceStates).filter(s => s === 'absent').length || attendance.filter(a => a.status === 'absent').length;
    const late = Object.values(attendanceStates).filter(s => s === 'late').length || attendance.filter(a => a.status === 'late').length;
    return { present, absent, late, total: members.length };
  }, [attendanceStates, attendance, members]);

  const exportAttendance = useCallback(() => {
    const headers = ['Name', 'Member ID', 'Status'];
    const rows = members.map(m => [
      m.full_name,
      m.member_id,
      attendanceStates[m.id] || attendance.find(a => a.member_id === m.id)?.status || 'not marked'
    ]);
    const csv = [headers.join(','), ...rows.map(r => r.join(','))].join('\n');
    const blob = new Blob([csv], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `attendance_${selectedDate}_${serviceType.replace(/\s/g, '_')}.csv`;
    a.click();
    URL.revokeObjectURL(url);
  }, [members, attendanceStates, attendance, selectedDate, serviceType]);

  const isMarked = (memberId: string) => {
    return attendance.some(a => a.member_id === memberId) || attendanceStates[memberId] !== undefined;
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      {/* Header */}
      <div className="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Attendance</h1>
          <p className="text-gray-500 text-sm mt-1">Mark and track member attendance</p>
        </div>
        <div className="flex space-x-2">
          <Button variant="outline" size="sm" onClick={exportAttendance}>
            <Download className="h-4 w-4 mr-2" /> Export
          </Button>
          <Button onClick={handleSaveAttendance} loading={saving}>
            <Save className="h-4 w-4 mr-2" /> Save Attendance
          </Button>
        </div>
      </div>

      {/* Filters */}
      <Card className="p-4">
        <div className="grid md:grid-cols-3 gap-4">
          <Input
            label="Date"
            type="date"
            value={selectedDate}
            onChange={(e) => setSelectedDate(e.target.value)}
          />
          <Select
            label="Service Type"
            value={serviceType}
            onChange={(e) => setServiceType(e.target.value)}
            options={[
              { value: 'Sunday Service', label: 'Sunday Service' },
              { value: 'Midweek Service', label: 'Midweek Service' },
              { value: 'Prayer Meeting', label: 'Prayer Meeting' },
              { value: 'Youth Service', label: 'Youth Service' },
              { value: 'Special Event', label: 'Special Event' }
            ]}
          />
          <div className="flex items-end">
            <p className="text-sm text-gray-500">
              {members.length} approved members
            </p>
          </div>
        </div>
      </Card>

      {/* Stats */}
      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        <Card className="p-4 bg-green-50 dark:bg-green-900/20">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-sm text-green-600 dark:text-green-400">Present</p>
              <p className="text-3xl font-bold text-green-700 dark:text-green-300">{stats.present}</p>
            </div>
            <Check className="h-8 w-8 text-green-600 opacity-50" />
          </div>
        </Card>
        <Card className="p-4 bg-red-50 dark:bg-red-900/20">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-sm text-red-600 dark:text-red-400">Absent</p>
              <p className="text-3xl font-bold text-red-700 dark:text-red-300">{stats.absent}</p>
            </div>
            <X className="h-8 w-8 text-red-600 opacity-50" />
          </div>
        </Card>
        <Card className="p-4 bg-yellow-50 dark:bg-yellow-900/20">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-sm text-yellow-600 dark:text-yellow-400">Late</p>
              <p className="text-3xl font-bold text-yellow-700 dark:text-yellow-300">{stats.late}</p>
            </div>
            <AlertCircle className="h-8 w-8 text-yellow-600 opacity-50" />
          </div>
        </Card>
        <Card className="p-4 bg-blue-50 dark:bg-blue-900/20">
          <div className="flex items-center justify-between">
            <div>
              <p className="text-sm text-blue-600 dark:text-blue-400">Total</p>
              <p className="text-3xl font-bold text-blue-700 dark:text-blue-300">{stats.total}</p>
            </div>
            <Users className="h-8 w-8 text-blue-600 opacity-50" />
          </div>
        </Card>
      </div>

      {/* Attendance List */}
      <Card>
        <div className="p-4 border-b border-gray-200 dark:border-gray-700">
          <h3 className="font-medium text-gray-900 dark:text-white">Mark Attendance</h3>
          <p className="text-sm text-gray-500">Once marked, attendance cannot be changed</p>
        </div>
        <div className="divide-y divide-gray-200 dark:divide-gray-700">
          {members.map((member) => {
            const marked = isMarked(member.id);
            const currentStatus = attendanceStates[member.id] || attendance.find(a => a.member_id === member.id)?.status;

            return (
              <div key={member.id} className="p-4 flex items-center justify-between hover:bg-gray-50 dark:hover:bg-gray-800">
                <div className="flex items-center space-x-4">
                  <div className="w-10 h-10 rounded-full bg-gray-200 dark:bg-gray-700 overflow-hidden flex items-center justify-center">
                    {member.photo_url ? (
                      <img src={member.photo_url} alt="" className="w-full h-full object-cover" />
                    ) : (
                      <span className="text-gray-600 font-medium">{member.full_name.charAt(0)}</span>
                    )}
                  </div>
                  <div>
                    <p className="font-medium text-gray-900 dark:text-white">{member.full_name}</p>
                    <p className="text-sm text-gray-500">{member.member_id} | {member.fellowship}</p>
                  </div>
                </div>
                <div className="flex items-center space-x-2">
                  {currentStatus ? (
                    <Badge variant={currentStatus === 'present' ? 'success' : currentStatus === 'late' ? 'warning' : 'error'}>
                      {currentStatus.charAt(0).toUpperCase() + currentStatus.slice(1)}
                    </Badge>
                  ) : (
                    <>
                      <Button
                        size="sm"
                        variant={attendanceStates[member.id] === 'present' ? 'primary' : 'outline'}
                        onClick={() => handleMarkAttendance(member.id, 'present')}
                      >
                        <Check className="h-4 w-4" />
                      </Button>
                      <Button
                        size="sm"
                        variant={attendanceStates[member.id] === 'late' ? 'primary' : 'outline'}
                        onClick={() => handleMarkAttendance(member.id, 'late')}
                        className="bg-yellow-500 text-white hover:bg-yellow-600"
                      >
                        <Clock className="h-4 w-4" />
                      </Button>
                      <Button
                        size="sm"
                        variant={attendanceStates[member.id] === 'absent' ? 'danger' : 'outline'}
                        onClick={() => handleMarkAttendance(member.id, 'absent')}
                      >
                        <X className="h-4 w-4" />
                      </Button>
                    </>
                  )}
                </div>
              </div>
            );
          })}
        </div>
      </Card>
    </div>
  );
}

```

## `./src/pages/admin/BackupPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase } from '../../lib/supabase';
import { Card, Button, LoadingSpinner } from '../../components/ui';
import { Database, Download, Upload } from 'lucide-react';

export function BackupPage() {
  const [loading, setLoading] = useState(true);
  const [exporting, setExporting] = useState(false);
  const [lastBackup, setLastBackup] = useState<string | null>(null);

  useEffect(() => {
    const stored = localStorage.getItem('dwm_last_backup');
    if (stored) setLastBackup(stored);
    setLoading(false);
  }, []);

  const exportData = async () => {
    setExporting(true);
    try {
      const tables = ['members', 'events', 'sermons', 'gallery', 'announcements', 'adverts', 'donations', 'pastor_history', 'attendance'];
      const backup: Record<string, unknown[]> = {};

      for (const table of tables) {
        const { data } = await supabase.from(table).select('*');
        backup[table] = data || [];
      }

      const blob = new Blob([JSON.stringify(backup, null, 2)], { type: 'application/json' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = `dwm_backup_${new Date().toISOString().split('T')[0]}.json`;
      a.click();
      URL.revokeObjectURL(url);

      const now = new Date().toISOString();
      localStorage.setItem('dwm_last_backup', now);
      setLastBackup(now);
    } catch (error) {
      alert('Export failed');
    }
    setExporting(false);
  };

  const handleImport = async (file: File) => {
    try {
      const text = await file.text();
      const data = JSON.parse(text);

      for (const [table, records] of Object.entries(data)) {
        if (Array.isArray(records) && records.length > 0) {
          await supabase.from(table).insert(records);
        }
      }
      alert('Data restored successfully!');
    } catch (error) {
      alert('Import failed. Please check the file format.');
    }
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Backup & Recovery</h1>
          <p className="text-gray-500 text-sm mt-1">Backup and restore your data</p>
        </div>
      </div>

      <Card className="p-6">
        <div className="flex items-center justify-between">
          <div>
            <h3 className="font-medium text-gray-900 dark:text-white">Last Backup</h3>
            <p className="text-sm text-gray-500">{lastBackup ? new Date(lastBackup).toLocaleString() : 'No backup yet'}</p>
          </div>
          <Button onClick={exportData} loading={exporting}><Download className="h-4 w-4 mr-2" /> Export Backup</Button>
        </div>
      </Card>

      <Card className="p-6">
        <h3 className="font-medium text-gray-900 dark:text-white mb-4">Restore Data</h3>
        <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-8 text-center">
          <Upload className="h-12 w-12 mx-auto text-gray-400 mb-4" />
          <p className="text-gray-600 dark:text-gray-400 mb-4">Upload a backup file to restore data</p>
          <label className="cursor-pointer">
            <Button variant="outline" type="button">Select Backup File</Button>
            <input type="file" accept=".json" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) handleImport(file); }} />
          </label>
        </div>
        <p className="text-xs text-red-500 mt-4">Warning: Restoring will add data to existing records!</p>
      </Card>

      <Card className="p-6">
        <div className="flex items-center justify-between">
          <div>
            <h3 className="font-medium text-gray-900 dark:text-white">Automatic Backups</h3>
            <p className="text-sm text-gray-500">Cloud backup feature coming soon</p>
          </div>
          <Button variant="outline" disabled>Coming Soon</Button>
        </div>
      </Card>
    </div>
  );
}

```

## `./src/pages/admin/ContentPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { useTheme } from '../../contexts/ThemeContext';
import { supabase } from '../../lib/supabase';
import { Card, Button, Input, TextArea, LoadingSpinner } from '../../components/ui';
import { Save } from 'lucide-react';

export function ContentPage() {
  const { settings, refreshSettings } = useTheme();
  const [saving, setSaving] = useState(false);
  const [loading, setLoading] = useState(true);

  const [formData, setFormData] = useState({
    church_name: '',
    motto: '',
    slogan: '',
    address: '',
    phone: '',
    email: '',
    website: '',
    theme_of_year: '',
    verse_of_year: '',
    welcome_message: '',
    about_text: '',
    donation_text: '',
    live_stream_url: ''
  });

  useEffect(() => {
    if (settings) {
      setFormData({
        church_name: settings.church_name || '',
        motto: settings.motto || '',
        slogan: settings.slogan || '',
        address: settings.address || '',
        phone: settings.phone || '',
        email: settings.email || '',
        website: settings.website || '',
        theme_of_year: settings.theme_of_year || '',
        verse_of_year: settings.verse_of_year || '',
        welcome_message: settings.welcome_message || '',
        about_text: (settings as unknown as Record<string, string>).about_text || '',
        donation_text: (settings as unknown as Record<string, string>).donation_text || 'Support our ministry with your generous donations.',
        live_stream_url: (settings as unknown as Record<string, string>).live_stream_url || ''
      });
      setLoading(false);
    }
  }, [settings]);

  const handleSave = async () => {
    setSaving(true);
    const { error } = await supabase.from('church_settings').update({
      church_name: formData.church_name,
      motto: formData.motto,
      slogan: formData.slogan,
      address: formData.address,
      phone: formData.phone,
      email: formData.email,
      website: formData.website,
      theme_of_year: formData.theme_of_year,
      verse_of_year: formData.verse_of_year,
      welcome_message: formData.welcome_message,
      about_text: formData.about_text,
      donation_text: formData.donation_text,
      live_stream_url: formData.live_stream_url,
      updated_at: new Date().toISOString()
    }).neq('id', '00000000-0000-0000-0000-000000000000');

    if (!error) {
      refreshSettings?.();
      alert('Content saved successfully!');
    } else {
      alert('Error saving content');
    }
    setSaving(false);
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Content Manager</h1>
          <p className="text-gray-500 text-sm mt-1">Edit website content and text</p>
        </div>
        <Button onClick={handleSave} loading={saving}><Save className="h-4 w-4 mr-2" /> Save All</Button>
      </div>

      <Card className="p-6">
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Church Identity</h2>
        <div className="space-y-4">
          <Input label="Church Name" value={formData.church_name} onChange={(e) => setFormData(prev => ({ ...prev, church_name: e.target.value }))} />
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Motto" value={formData.motto} onChange={(e) => setFormData(prev => ({ ...prev, motto: e.target.value }))} />
            <Input label="Slogan" value={formData.slogan} onChange={(e) => setFormData(prev => ({ ...prev, slogan: e.target.value }))} />
          </div>
        </div>
      </Card>

      <Card className="p-6">
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Contact Information</h2>
        <div className="space-y-4">
          <Input label="Address" value={formData.address} onChange={(e) => setFormData(prev => ({ ...prev, address: e.target.value }))} />
          <div className="grid md:grid-cols-3 gap-4">
            <Input label="Phone" value={formData.phone} onChange={(e) => setFormData(prev => ({ ...prev, phone: e.target.value }))} />
            <Input label="Email" value={formData.email} onChange={(e) => setFormData(prev => ({ ...prev, email: e.target.value }))} />
            <Input label="Website" value={formData.website} onChange={(e) => setFormData(prev => ({ ...prev, website: e.target.value }))} />
          </div>
        </div>
      </Card>

      <Card className="p-6">
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Home Page</h2>
        <div className="space-y-4">
          <TextArea label="Welcome Message" value={formData.welcome_message} onChange={(e) => setFormData(prev => ({ ...prev, welcome_message: e.target.value }))} rows={2} />
        </div>
      </Card>

      <Card className="p-6">
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Theme of the Year</h2>
        <div className="space-y-4">
          <Input label="Theme" value={formData.theme_of_year} onChange={(e) => setFormData(prev => ({ ...prev, theme_of_year: e.target.value }))} placeholder="e.g., Year of Breakthrough" />
          <Input label="Verse" value={formData.verse_of_year} onChange={(e) => setFormData(prev => ({ ...prev, verse_of_year: e.target.value }))} placeholder="e.g., Jeremiah 29:11" />
        </div>
      </Card>

      <Card className="p-6">
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">About Page (Editable)</h2>
        <TextArea label="About Content" value={formData.about_text} onChange={(e) => setFormData(prev => ({ ...prev, about_text: e.target.value }))} rows={8} placeholder="Write about the church, its history, mission, vision, values, etc." />
      </Card>

      <Card className="p-6">
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Donation Page</h2>
        <TextArea label="Donation Message" value={formData.donation_text} onChange={(e) => setFormData(prev => ({ ...prev, donation_text: e.target.value }))} rows={3} />
      </Card>

      <Card className="p-6">
        <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Live Stream</h2>
        <Input label="Live Stream URL" value={formData.live_stream_url} onChange={(e) => setFormData(prev => ({ ...prev, live_stream_url: e.target.value }))} placeholder="YouTube or Facebook Live embed URL" />
      </Card>
    </div>
  );
}

```

## `./src/pages/admin/DashboardPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { Link } from 'react-router-dom';
import { useAuth } from '../../contexts/AuthContext';
import { supabase, Member, Event } from '../../lib/supabase';
import { Card, Badge, LoadingSpinner, Button } from '../../components/ui';
import { Users, Calendar, Clock, UserPlus, AlertCircle, CheckCircle, XCircle, ExternalLink, UserCheck, Eye, Edit, Trash2 } from 'lucide-react';

interface Stats {
  totalMembers: number;
  pendingMembers: number;
  todayAttendance: number;
  upcomingEvents: number;
  totalDonations: number;
}

export function AdminDashboard() {
  const { user, hasPermission } = useAuth();
  const [stats, setStats] = useState<Stats>({
    totalMembers: 0,
    pendingMembers: 0,
    todayAttendance: 0,
    upcomingEvents: 0,
    totalDonations: 0
  });
  const [recentMembers, setRecentMembers] = useState<Member[]>([]);
  const [upcomingEvents, setUpcomingEvents] = useState<Event[]>([]);
  const [loading, setLoading] = useState(true);
  const [selectedMember, setSelectedMember] = useState<Member | null>(null);
  const [showMemberModal, setShowMemberModal] = useState(false);

  useEffect(() => {
    fetchData();
  }, []);

  const fetchData = async () => {
    const [
      { count: totalMembers },
      { data: pendingData },
      { data: todayAtt },
      { data: upcomingEvts },
      { data: recentMems }
    ] = await Promise.all([
      supabase.from('members').select('id', { count: 'exact', head: true }),
      supabase.from('members').select('id').eq('status', 'pending'),
      supabase.from('attendance').select('id').eq('service_date', new Date().toISOString().split('T')[0]),
      supabase.from('events').select('*').eq('status', 'upcoming').gte('event_date', new Date().toISOString().split('T')[0]).order('event_date', { ascending: true }).limit(5),
      supabase.from('members').select('*, departments(*)').order('created_at', { ascending: false }).limit(5)
    ]);

    setStats({
      totalMembers: totalMembers || 0,
      pendingMembers: pendingData?.length || 0,
      todayAttendance: todayAtt?.length || 0,
      upcomingEvents: upcomingEvts?.length || 0,
      totalDonations: 0
    });
    setRecentMembers(recentMems || []);
    setUpcomingEvents(upcomingEvts || []);
    setLoading(false);
  };

  const handleApproveMember = async (member: Member) => {
    const fellowship = member.date_of_birth && member.gender ?
      (new Date().getFullYear() - new Date(member.date_of_birth).getFullYear() < 30 ? 'Youth Fellowship' :
       member.gender === 'Male' ? "Men's Fellowship" : "Women's Fellowship") : null;

    await supabase.from('members').update({ status: 'approved', fellowship }).eq('id', member.id);
    fetchData();
  };

  const handleRejectMember = async (member: Member) => {
    await supabase.from('members').update({ status: 'rejected' }).eq('id', member.id);
    fetchData();
  };

  const handleDeleteMember = async (member: Member) => {
    if (!confirm('Delete this member permanently?')) return;
    await supabase.from('deleted_records').insert({ table_name: 'members', record_id: member.id, record_data: member as unknown as Record<string, unknown> });
    await supabase.from('members').delete().eq('id', member.id);
    setShowMemberModal(false);
    fetchData();
  };

  if (loading) {
    return <div className="flex items-center justify-center py-20"><LoadingSpinner size="lg" /></div>;
  }

  const statCards = [
    { title: 'Total Members', value: stats.totalMembers, icon: Users, color: 'from-blue-500 to-blue-600', link: '/admin/members' },
    { title: 'Pending Registrations', value: stats.pendingMembers, icon: UserPlus, color: 'from-yellow-500 to-orange-500', link: '/admin/members?status=pending' },
    { title: 'Today\'s Attendance', value: stats.todayAttendance, icon: Clock, color: 'from-green-500 to-green-600', link: '/admin/attendance' },
    { title: 'Upcoming Events', value: stats.upcomingEvents, icon: Calendar, color: 'from-purple-500 to-purple-600', link: '/admin/events' }
  ];

  return (
    <div className="space-y-8">
      {/* Welcome Header */}
      <div className="bg-gradient-to-r from-blue-600 to-green-600 rounded-2xl p-8 text-white">
        <div className="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
          <div>
            <h1 className="text-2xl md:text-3xl font-bold mb-2">Welcome, {user?.full_name}!</h1>
            <p className="text-blue-100">Ready to manage {user?.role === 'admin' ? 'everything' : `your ${user?.role} duties`}?</p>
            <div className="mt-4 flex items-center text-sm text-blue-100">
              <Badge variant="success" className="bg-white/20 text-white mr-2 capitalize">{user?.role}</Badge>
              <span>Last login: {new Date().toLocaleDateString()}</span>
            </div>
          </div>
          <div className="flex flex-col sm:flex-row gap-3">
            <Link to="/admin/members"><Button className="bg-white text-blue-600 hover:bg-blue-50"><UserPlus className="h-4 w-4 mr-2" /> Register Member</Button></Link>
            <Link to="/" target="_blank"><Button variant="outline" className="border-white text-white hover:bg-white/10"><ExternalLink className="h-4 w-4 mr-2" /> View Public Site</Button></Link>
          </div>
        </div>
      </div>

      {/* Stat Cards */}
      <div className="grid grid-cols-2 lg:grid-cols-4 gap-4">
        {statCards.map((stat) => (
          <Link key={stat.title} to={stat.link}>
            <Card className={`bg-gradient-to-r ${stat.color} p-6 text-white hover:shadow-lg transition-all hover:scale-[1.02]`}>
              <div className="flex items-center justify-between">
                <div>
                  <p className="text-sm opacity-80">{stat.title}</p>
                  <p className="text-3xl font-bold mt-1">{stat.value}</p>
                </div>
                <stat.icon className="h-10 w-10 opacity-50" />
              </div>
            </Card>
          </Link>
        ))}
      </div>

      {/* Recent Activity & Events */}
      <div className="grid lg:grid-cols-2 gap-6">
        {/* Recent Members */}
        <Card>
          <div className="p-6 border-b border-gray-200 dark:border-gray-700">
            <div className="flex items-center justify-between">
              <h2 className="text-lg font-semibold text-gray-900 dark:text-white">Recent Registrations</h2>
              <Link to="/admin/members"><Button variant="ghost" size="sm">View All</Button></Link>
            </div>
          </div>
          <div className="divide-y divide-gray-200 dark:divide-gray-700">
            {recentMembers.length > 0 ? recentMembers.map((member) => (
              <div key={member.id} className="p-4 hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
                <div className="flex items-center space-x-4">
                  <div className="w-10 h-10 rounded-full bg-gray-200 dark:bg-gray-700 flex items-center justify-center overflow-hidden">
                    {member.photo_url ? (
                      <img src={member.photo_url} alt="" className="w-full h-full object-cover" />
                    ) : (
                      <span className="text-gray-600 dark:text-gray-300 font-medium">{member.full_name.charAt(0)}</span>
                    )}
                  </div>
                  <div className="flex-1 min-w-0">
                    <p className="font-medium text-gray-900 dark:text-white truncate">{member.full_name}</p>
                    <p className="text-sm text-gray-500 truncate">{member.member_id}</p>
                  </div>
                  <div className="flex items-center gap-2">
                    {member.status === 'pending' && <Badge variant="warning"><AlertCircle className="h-3 w-3 mr-1" />Pending</Badge>}
                    {member.status === 'approved' && <Badge variant="success"><CheckCircle className="h-3 w-3 mr-1" />Approved</Badge>}
                    {member.status === 'rejected' && <Badge variant="error"><XCircle className="h-3 w-3 mr-1" />Rejected</Badge>}
                    <button onClick={() => { setSelectedMember(member); setShowMemberModal(true); }} className="p-1.5 rounded hover:bg-gray-200 dark:hover:bg-gray-600" title="View"><Eye className="h-4 w-4 text-gray-500" /></button>
                  </div>
                </div>
              </div>
            )) : <div className="p-8 text-center text-gray-500">No recent registrations</div>}
          </div>
        </Card>

        {/* Upcoming Events */}
        <Card>
          <div className="p-6 border-b border-gray-200 dark:border-gray-700">
            <div className="flex items-center justify-between">
              <h2 className="text-lg font-semibold text-gray-900 dark:text-white">Upcoming Events</h2>
              <Link to="/admin/events"><Button variant="ghost" size="sm">View All</Button></Link>
            </div>
          </div>
          <div className="divide-y divide-gray-200 dark:divide-gray-700">
            {upcomingEvents.length > 0 ? upcomingEvents.map((event) => (
              <div key={event.id} className="p-4 hover:bg-gray-50 dark:hover:bg-gray-700/50 transition-colors">
                <div className="flex items-start space-x-4">
                  <div className="bg-blue-100 dark:bg-blue-900/20 rounded-lg p-3 text-center min-w-[60px]">
                    <p className="text-2xl font-bold text-blue-600 dark:text-blue-400">{new Date(event.event_date).getDate()}</p>
                    <p className="text-xs text-gray-500 uppercase">{new Date(event.event_date).toLocaleDateString('en-US', { month: 'short' })}</p>
                  </div>
                  <div className="flex-1">
                    <p className="font-medium text-gray-900 dark:text-white">{event.title}</p>
                    {event.start_time && <p className="text-sm text-gray-500">{event.start_time}</p>}
                    {event.location && <p className="text-sm text-gray-500 mt-1">{event.location}</p>}
                  </div>
                </div>
              </div>
            )) : <div className="p-8 text-center text-gray-500">No upcoming events</div>}
          </div>
        </Card>
      </div>

      {/* Quick Actions */}
      <Card>
        <div className="p-6">
          <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Quick Actions</h2>
          <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
            {hasPermission('members') && (
              <Link to="/admin/members?status=pending">
                <div className="p-4 rounded-xl bg-yellow-50 dark:bg-yellow-900/20 text-center hover:bg-yellow-100 dark:hover:bg-yellow-900/30 transition-colors">
                  <UserPlus className="h-8 w-8 mx-auto text-yellow-600 mb-2" />
                  <p className="text-sm font-medium text-gray-900 dark:text-white">Pending Members</p>
                </div>
              </Link>
            )}
            {hasPermission('attendance') && (
              <Link to="/admin/attendance">
                <div className="p-4 rounded-xl bg-green-50 dark:bg-green-900/20 text-center hover:bg-green-100 dark:hover:bg-green-900/30 transition-colors">
                  <Clock className="h-8 w-8 mx-auto text-green-600 mb-2" />
                  <p className="text-sm font-medium text-gray-900 dark:text-white">Take Attendance</p>
                </div>
              </Link>
            )}
            {hasPermission('events') && (
              <Link to="/admin/events">
                <div className="p-4 rounded-xl bg-blue-50 dark:bg-blue-900/20 text-center hover:bg-blue-100 dark:hover:bg-blue-900/30 transition-colors">
                  <Calendar className="h-8 w-8 mx-auto text-blue-600 mb-2" />
                  <p className="text-sm font-medium text-gray-900 dark:text-white">Manage Events</p>
                </div>
              </Link>
            )}
            <Link to="/">
              <div className="p-4 rounded-xl bg-gray-50 dark:bg-gray-700 text-center hover:bg-gray-100 dark:hover:bg-gray-600 transition-colors cursor-pointer">
                <ExternalLink className="h-8 w-8 mx-auto text-gray-600 dark:text-gray-300 mb-2" />
                <p className="text-sm font-medium text-gray-900 dark:text-white">View Public Site</p>
              </div>
            </Link>
          </div>
        </div>
      </Card>

      {/* Member Detail Modal */}
      {showMemberModal && selectedMember && (
        <div className="fixed inset-0 z-50 bg-black/50 flex items-center justify-center p-4" onClick={() => setShowMemberModal(false)}>
          <div className="bg-white dark:bg-gray-800 rounded-xl max-w-lg w-full max-h-[90vh] overflow-y-auto" onClick={e => e.stopPropagation()}>
            <div className="p-6">
              <div className="flex justify-between items-start mb-4">
                <h2 className="text-xl font-bold text-gray-900 dark:text-white">Member Details</h2>
                <button onClick={() => setShowMemberModal(false)} className="text-gray-400 hover:text-gray-600">&times;</button>
              </div>
              <div className="flex items-center space-x-4 mb-6">
                <div className="w-20 h-20 rounded-full bg-gray-200 dark:bg-gray-700 overflow-hidden">
                  {selectedMember.photo_url ? <img src={selectedMember.photo_url} alt="" className="w-full h-full object-cover" /> : <span className="w-full h-full flex items-center justify-center text-2xl text-gray-500">{selectedMember.full_name.charAt(0)}</span>}
                </div>
                <div>
                  <h3 className="font-bold text-lg text-gray-900 dark:text-white">{selectedMember.full_name}</h3>
                  <p className="text-gray-500">{selectedMember.member_id}</p>
                  <Badge variant={selectedMember.status === 'approved' ? 'success' : selectedMember.status === 'pending' ? 'warning' : 'error'} className="mt-1">{selectedMember.status}</Badge>
                </div>
              </div>
              <div className="space-y-3 text-sm">
                <div className="flex justify-between"><span className="text-gray-500">DOB:</span><span className="text-gray-900 dark:text-white">{selectedMember.date_of_birth}</span></div>
                <div className="flex justify-between"><span className="text-gray-500">Gender:</span><span className="text-gray-900 dark:text-white">{selectedMember.gender}</span></div>
                <div className="flex justify-between"><span className="text-gray-500">Phone:</span><span className="text-gray-900 dark:text-white">{selectedMember.phone || 'N/A'}</span></div>
                <div className="flex justify-between"><span className="text-gray-500">Email:</span><span className="text-gray-900 dark:text-white">{selectedMember.email || 'N/A'}</span></div>
                <div className="flex justify-between"><span className="text-gray-500">Fellowship:</span><span className="text-gray-900 dark:text-white">{selectedMember.fellowship || 'N/A'}</span></div>
                <div className="flex justify-between"><span className="text-gray-500">Department:</span><span className="text-gray-900 dark:text-white">{(selectedMember.departments as { name: string })?.name || 'N/A'}</span></div>
                <div className="flex justify-between"><span className="text-gray-500">Address:</span><span className="text-gray-900 dark:text-white">{selectedMember.address || 'N/A'}</span></div>
              </div>
              <div className="flex gap-2 mt-6 pt-4 border-t dark:border-gray-700">
                {selectedMember.status === 'pending' && (
                  <>
                    <Button onClick={() => { handleApproveMember(selectedMember); setShowMemberModal(false); }} className="flex-1"><UserCheck className="h-4 w-4 mr-2" /> Approve</Button>
                    <Button variant="outline" onClick={() => { handleRejectMember(selectedMember); setShowMemberModal(false); }} className="flex-1"><XCircle className="h-4 w-4 mr-2" /> Reject</Button>
                  </>
                )}
                <Link to={`/admin/members`} className="flex-1"><Button variant="outline" className="w-full"><Edit className="h-4 w-4 mr-2" /> Edit</Button></Link>
                <Button variant="danger" onClick={() => handleDeleteMember(selectedMember)}><Trash2 className="h-4 w-4" /></Button>
              </div>
            </div>
          </div>
        </div>
      )}
    </div>
  );
}

```

## `./src/pages/admin/DonationsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, Donation } from '../../lib/supabase';
import { Card, Button, Input, Select, Modal, Badge, LoadingSpinner } from '../../components/ui';
import { DollarSign, Plus, Download, Trash2, Filter } from 'lucide-react';

export function DonationsPage() {
  const [donations, setDonations] = useState<Donation[]>([]);
  const [loading, setLoading] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [saving, setSaving] = useState(false);
  const [filter, setFilter] = useState('all');

  const [formData, setFormData] = useState({
    amount: '',
    currency: 'GHS',
    donation_type: 'offering',
    payment_method: 'cash',
    notes: '',
    end_date: ''
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('donations').select('*').order('donation_date', { ascending: false });
    setDonations(data || []);
    setLoading(false);
  };

  const handleSave = async () => {
    if (!formData.amount) { alert('Amount is required'); return; }
    setSaving(true);

    try {
      let endDate = formData.end_date || new Date(Date.now() + 30 * 24 * 60 * 60 * 1000).toISOString().split('T')[0];

      const { error } = await supabase.from('donations').insert({
        amount: parseFloat(formData.amount),
        currency: formData.currency,
        donation_type: formData.donation_type,
        payment_method: formData.payment_method,
        notes: formData.notes || null,
        end_date: endDate
      });

      if (error) throw error;

      setShowModal(false);
      setFormData({ amount: '', currency: 'GHS', donation_type: 'offering', payment_method: 'cash', notes: '', end_date: '' });
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const handleDelete = async (donation: Donation) => {
    if (!confirm('Delete this donation?')) return;
    await supabase.from('deleted_records').insert({ table_name: 'donations', record_id: donation.id, record_data: donation as unknown as Record<string, unknown> });
    await supabase.from('donations').delete().eq('id', donation.id);
    fetchData();
  };

  const exportCSV = () => {
    const headers = ['Date', 'Amount', 'Currency', 'Type', 'Method', 'Notes', 'Expires'];
    const rows = donations.map(d => [d.donation_date, d.amount, d.currency, d.donation_type || '', d.payment_method || '', d.notes || '', d.end_date || '']);
    const csv = [headers.join(','), ...rows.map(r => r.join(','))].join('\n');
    const blob = new Blob([csv], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `donations_${new Date().toISOString().split('T')[0]}.csv`;
    a.click();
    URL.revokeObjectURL(url);
  };

  const filteredDonations = filter === 'all' ? donations : donations.filter(d => d.donation_type === filter);
  const totalAmount = filteredDonations.reduce((sum, d) => sum + Number(d.amount), 0);

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Donations</h1>
          <p className="text-gray-500 text-sm mt-1">Track and manage donations</p>
        </div>
        <div className="flex space-x-2">
          <Button variant="outline" size="sm" onClick={exportCSV}><Download className="h-4 w-4 mr-2" /> Export</Button>
          <Button onClick={() => setShowModal(true)}><Plus className="h-4 w-4 mr-2" /> Add Donation</Button>
        </div>
      </div>

      <div className="grid grid-cols-2 md:grid-cols-4 gap-4">
        <Card className="p-4 bg-green-50 dark:bg-green-900/20">
          <p className="text-sm text-green-600 dark:text-green-400">Total Donations</p>
          <p className="text-2xl font-bold text-green-700 dark:text-green-300">GHS {totalAmount.toLocaleString()}</p>
        </Card>
        <Card className="p-4 bg-blue-50 dark:bg-blue-900/20">
          <p className="text-sm text-blue-600 dark:text-blue-400">This Month</p>
          <p className="text-2xl font-bold text-blue-700 dark:text-blue-300">GHS {donations.filter(d => new Date(d.donation_date).getMonth() === new Date().getMonth()).reduce((s, d) => s + Number(d.amount), 0).toLocaleString()}</p>
        </Card>
        <Card className="p-4">
          <p className="text-sm text-gray-600 dark:text-gray-400">Total Records</p>
          <p className="text-2xl font-bold text-gray-900 dark:text-white">{donations.length}</p>
        </Card>
        <Card className="p-4">
          <p className="text-sm text-gray-600 dark:text-gray-400">Average</p>
          <p className="text-2xl font-bold text-gray-900 dark:text-white">GHS {donations.length ? (totalAmount / donations.length).toFixed(0) : 0}</p>
        </Card>
      </div>

      <Card className="p-4">
        <div className="flex items-center space-x-4">
          <Filter className="h-5 w-5 text-gray-400" />
          <div className="flex flex-wrap gap-2">
            {['all', 'offering', 'tithe', 'thanksgiving', 'building', 'welfare'].map((type) => (
              <button key={type} onClick={() => setFilter(type)} className={`px-3 py-1 rounded-lg text-sm ${filter === type ? 'bg-blue-600 text-white' : 'bg-gray-100 dark:bg-gray-700 text-gray-600 dark:text-gray-300'}`}>{type.charAt(0).toUpperCase() + type.slice(1)}</button>
            ))}
          </div>
        </div>
      </Card>

      <Card>
        <div className="divide-y divide-gray-200 dark:divide-gray-700">
          {filteredDonations.length > 0 ? filteredDonations.map((donation) => (
            <div key={donation.id} className="p-4 flex items-center justify-between hover:bg-gray-50 dark:hover:bg-gray-800">
              <div className="flex items-center space-x-4">
                <div className="w-12 h-12 rounded-full bg-green-100 dark:bg-green-900/20 flex items-center justify-center">
                  <DollarSign className="h-6 w-6 text-green-600" />
                </div>
                <div>
                  <p className="font-medium text-gray-900 dark:text-white">GHS {donation.amount.toLocaleString()}</p>
                  <p className="text-sm text-gray-500">{donation.donation_type} | {donation.payment_method}</p>
                  {donation.end_date && <Badge variant="warning" className="mt-1">Expires: {new Date(donation.end_date).toLocaleDateString()}</Badge>}
                </div>
              </div>
              <div className="flex items-center space-x-4">
                <p className="text-sm text-gray-500">{new Date(donation.donation_date).toLocaleDateString()}</p>
                <button onClick={() => handleDelete(donation)} className="p-2 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/20"><Trash2 className="h-4 w-4 text-red-600" /></button>
              </div>
            </div>
          )) : <div className="p-8 text-center text-gray-500">No donations recorded. Click "Add Donation" to add one.</div>}
        </div>
      </Card>

      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title="Add Donation" size="md">
        <div className="space-y-5">
          <div className="grid grid-cols-2 gap-4">
            <Input label="Amount *" type="number" value={formData.amount} onChange={(e) => setFormData(prev => ({ ...prev, amount: e.target.value }))} />
            <Select label="Currency" value={formData.currency} onChange={(e) => setFormData(prev => ({ ...prev, currency: e.target.value }))} options={[{ value: 'GHS', label: 'GHS' }, { value: 'USD', label: 'USD' }]} />
          </div>
          <div className="grid grid-cols-2 gap-4">
            <Select label="Type" value={formData.donation_type} onChange={(e) => setFormData(prev => ({ ...prev, donation_type: e.target.value }))} options={[{ value: 'offering', label: 'Offering' }, { value: 'tithe', label: 'Tithe' }, { value: 'thanksgiving', label: 'Thanksgiving' }, { value: 'building', label: 'Building Fund' }, { value: 'welfare', label: 'Welfare' }]} />
            <Select label="Payment Method" value={formData.payment_method} onChange={(e) => setFormData(prev => ({ ...prev, payment_method: e.target.value }))} options={[{ value: 'cash', label: 'Cash' }, { value: 'mobile_money', label: 'Mobile Money' }, { value: 'bank_transfer', label: 'Bank Transfer' }, { value: 'card', label: 'Card' }]} />
          </div>
          <Input label="Notes" value={formData.notes} onChange={(e) => setFormData(prev => ({ ...prev, notes: e.target.value }))} />
          <Input label="End Date (Auto-remove)" type="date" value={formData.end_date} onChange={(e) => setFormData(prev => ({ ...prev, end_date: e.target.value }))} />
          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>Add Donation</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/EventsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, Event } from '../../lib/supabase';
import { Card, Button, Input, TextArea, Select, Badge, Modal, LoadingSpinner } from '../../components/ui';
import { Calendar, Plus, Edit, Trash2, Video, Upload } from 'lucide-react';

export function EventsAdminPage() {
  const [events, setEvents] = useState<Event[]>([]);
  const [loading, setLoading] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [editEvent, setEditEvent] = useState<Event | null>(null);
  const [saving, setSaving] = useState(false);
  const [imageFile, setImageFile] = useState<File | null>(null);
  const [videoFile, setVideoFile] = useState<File | null>(null);
  const [imagePreview, setImagePreview] = useState('');
  const [videoPreview, setVideoPreview] = useState('');

  const [formData, setFormData] = useState({
    title: '',
    description: '',
    event_date: '',
    end_date: '',
    start_time: '',
    end_time: '',
    location: '',
    video_url: '',
    status: 'upcoming' as 'upcoming' | 'completed' | 'cancelled'
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('events').select('*').order('event_date', { ascending: true });
    setEvents(data || []);
    setLoading(false);
  };

  const openModal = (event: Event | null) => {
    if (event) {
      setFormData({
        title: event.title,
        description: event.description || '',
        event_date: event.event_date,
        end_date: event.end_date || '',
        start_time: event.start_time || '',
        end_time: event.end_time || '',
        location: event.location || '',
        video_url: event.video_url || '',
        status: event.status
      });
      setImagePreview(event.image_url || '');
      setVideoPreview(event.video_url || '');
      setEditEvent(event);
    } else {
      setFormData({ title: '', description: '', event_date: '', end_date: '', start_time: '', end_time: '', location: '', video_url: '', status: 'upcoming' });
      setImagePreview('');
      setVideoPreview('');
      setEditEvent(null);
    }
    setImageFile(null);
    setVideoFile(null);
    setShowModal(true);
  };

  const handleSave = async () => {
    if (!formData.title || !formData.event_date) { alert('Title and date are required'); return; }
    setSaving(true);

    try {
      let imageUrl = editEvent?.image_url || '';
      let videoUrl = formData.video_url || '';

      if (imageFile) {
        const fileName = `events/${Date.now()}_${imageFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, imageFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          imageUrl = urlData.publicUrl;
        }
      }

      if (videoFile) {
        const fileName = `videos/events/${Date.now()}_${videoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, videoFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          videoUrl = urlData.publicUrl;
        }
      }

      const dataToSave = {
        title: formData.title,
        description: formData.description || null,
        event_date: formData.event_date,
        end_date: formData.end_date || null,
        start_time: formData.start_time || null,
        end_time: formData.end_time || null,
        location: formData.location || null,
        image_url: imageUrl || null,
        video_url: videoUrl || null,
        status: formData.status
      };

      let error;
      if (editEvent) {
        ({ error } = await supabase.from('events').update({ ...dataToSave, updated_at: new Date().toISOString() }).eq('id', editEvent.id));
      } else {
        ({ error } = await supabase.from('events').insert(dataToSave));
      }

      if (error) throw error;

      setShowModal(false);
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving event: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const handleDelete = async (event: Event) => {
    if (!confirm('Delete this event? It will be moved to trash.')) return;
    await supabase.from('deleted_records').insert({ table_name: 'events', record_id: event.id, record_data: event as unknown as Record<string, unknown> });
    await supabase.from('events').delete().eq('id', event.id);
    fetchData();
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Events Management</h1>
          <p className="text-gray-500 text-sm mt-1">Create and manage church events</p>
        </div>
        <Button onClick={() => openModal(null)}><Plus className="h-4 w-4 mr-2" /> Add Event</Button>
      </div>

      <Card>
        <div className="divide-y divide-gray-200 dark:divide-gray-700">
          {events.length > 0 ? events.map((event) => (
            <div key={event.id} className="p-4 flex items-center justify-between hover:bg-gray-50 dark:hover:bg-gray-800">
              <div className="flex items-center space-x-4">
                {event.image_url ? (
                  <img src={event.image_url} alt="" className="w-20 h-16 rounded-lg object-cover" />
                ) : (
                  <div className="w-20 h-16 rounded-lg bg-gradient-to-br from-blue-600 to-green-600 flex items-center justify-center text-white"><Calendar className="h-8 w-8" /></div>
                )}
                <div>
                  <h3 className="font-medium text-gray-900 dark:text-white">{event.title}</h3>
                  <p className="text-sm text-gray-500">{new Date(event.event_date).toLocaleDateString()} {event.start_time && `at ${event.start_time}`}</p>
                  {event.end_date && <p className="text-xs text-orange-500">Ends: {new Date(event.end_date).toLocaleDateString()}</p>}
                  {event.video_url && <p className="text-xs text-blue-500 flex items-center"><Video className="h-3 w-3 mr-1" /> Has video</p>}
                </div>
              </div>
              <div className="flex items-center space-x-4">
                <Badge variant={event.status === 'upcoming' ? 'info' : event.status === 'completed' ? 'success' : 'error'}>{event.status}</Badge>
                <div className="flex space-x-2">
                  <button onClick={() => openModal(event)} className="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><Edit className="h-4 w-4 text-gray-600" /></button>
                  <button onClick={() => handleDelete(event)} className="p-2 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/20"><Trash2 className="h-4 w-4 text-red-600" /></button>
                </div>
              </div>
            </div>
          )) : <div className="p-8 text-center text-gray-500">No events yet</div>}
        </div>
      </Card>

      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title={editEvent ? 'Edit Event' : 'New Event'} size="lg">
        <div className="space-y-5">
          <Input label="Title *" value={formData.title} onChange={(e) => setFormData(prev => ({ ...prev, title: e.target.value }))} />
          <TextArea label="Description" value={formData.description} onChange={(e) => setFormData(prev => ({ ...prev, description: e.target.value }))} rows={3} />
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Start Date *" type="date" value={formData.event_date} onChange={(e) => setFormData(prev => ({ ...prev, event_date: e.target.value }))} />
            <Input label="End Date (Deadline)" type="date" value={formData.end_date} onChange={(e) => setFormData(prev => ({ ...prev, end_date: e.target.value }))} />
          </div>
          <div className="grid md:grid-cols-3 gap-4">
            <Input label="Start Time" type="time" value={formData.start_time} onChange={(e) => setFormData(prev => ({ ...prev, start_time: e.target.value }))} />
            <Input label="End Time" type="time" value={formData.end_time} onChange={(e) => setFormData(prev => ({ ...prev, end_time: e.target.value }))} />
            <Select label="Status" value={formData.status} onChange={(e) => setFormData(prev => ({ ...prev, status: e.target.value as typeof formData.status }))} options={[{ value: 'upcoming', label: 'Upcoming' }, { value: 'completed', label: 'Completed' }, { value: 'cancelled', label: 'Cancelled' }]} />
          </div>
          <Input label="Location" value={formData.location} onChange={(e) => setFormData(prev => ({ ...prev, location: e.target.value }))} />

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Image</p>
            {imagePreview && <img src={imagePreview} alt="" className="max-w-full max-h-48 rounded-lg mb-2" />}
            <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
              <Upload className="h-4 w-4 mr-2" /> Upload Image
              <input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setImageFile(file); const reader = new FileReader(); reader.onload = (ev) => setImagePreview(ev.target?.result as string); reader.readAsDataURL(file); } }} />
            </label>
          </div>

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Video</p>
            {videoPreview && videoPreview.startsWith('blob:') && <video src={videoPreview} controls className="max-w-full max-h-48 rounded-lg mb-2" />}
            {videoPreview && !videoPreview.startsWith('blob:') && videoPreview.startsWith('http') && <p className="text-sm text-green-600 mb-2">Video uploaded</p>}
            <div className="flex gap-2 flex-wrap">
              <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700">
                <Video className="h-4 w-4 mr-2" /> Upload Video
                <input type="file" accept="video/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setVideoFile(file); setVideoPreview(URL.createObjectURL(file)); } }} />
              </label>
              <Input value={formData.video_url} onChange={(e) => { setFormData(prev => ({ ...prev, video_url: e.target.value })); setVideoPreview(e.target.value); }} placeholder="Or YouTube URL" className="flex-1 min-w-48" />
            </div>
          </div>

          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>{editEvent ? 'Save Changes' : 'Create Event'}</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/GalleryPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, GalleryItem } from '../../lib/supabase';
import { Card, Button, Input, Select, Modal, LoadingSpinner } from '../../components/ui';
import { Image, Plus, Edit, Trash2, Video, Upload } from 'lucide-react';

export function GalleryAdminPage() {
  const [gallery, setGallery] = useState<GalleryItem[]>([]);
  const [loading, setLoading] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [editItem, setEditItem] = useState<GalleryItem | null>(null);
  const [saving, setSaving] = useState(false);
  const [mediaFile, setMediaFile] = useState<File | null>(null);
  const [preview, setPreview] = useState('');
  const [videoPreview, setVideoPreview] = useState('');

  const [formData, setFormData] = useState({
    title: '',
    type: 'image' as 'image' | 'video',
    end_date: ''
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('gallery').select('*').order('created_at', { ascending: false });
    setGallery(data || []);
    setLoading(false);
  };

  const openModal = (item: GalleryItem | null) => {
    if (item) {
      setFormData({ title: item.title || '', type: item.type, end_date: item.end_date || '' });
      setPreview(item.type === 'image' ? item.url : '');
      setVideoPreview(item.type === 'video' ? item.url : '');
      setEditItem(item);
    } else {
      setFormData({ title: '', type: 'image', end_date: '' });
      setPreview('');
      setVideoPreview('');
      setEditItem(null);
    }
    setMediaFile(null);
    setShowModal(true);
  };

  const handleSave = async () => {
    if (!mediaFile && !editItem?.url) { alert('Please select a file'); return; }
    setSaving(true);

    try {
      let url = editItem?.url || '';

      if (mediaFile) {
        const folder = formData.type === 'video' ? 'videos/gallery' : 'gallery';
        const fileName = `${folder}/${Date.now()}_${mediaFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, mediaFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          url = urlData.publicUrl;
        }
      }

      const dataToSave = {
        title: formData.title || null,
        type: formData.type,
        url,
        end_date: formData.end_date || null
      };

      let error;
      if (editItem) {
        ({ error } = await supabase.from('gallery').update(dataToSave).eq('id', editItem.id));
      } else {
        ({ error } = await supabase.from('gallery').insert(dataToSave));
      }

      if (error) throw error;

      setShowModal(false);
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const handleDelete = async (item: GalleryItem) => {
    if (!confirm('Delete this item?')) return;
    await supabase.from('deleted_records').insert({ table_name: 'gallery', record_id: item.id, record_data: item as unknown as Record<string, unknown> });
    await supabase.from('gallery').delete().eq('id', item.id);
    fetchData();
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Gallery</h1>
          <p className="text-gray-500 text-sm mt-1">Manage photos and videos</p>
        </div>
        <Button onClick={() => openModal(null)}><Plus className="h-4 w-4 mr-2" /> Add Media</Button>
      </div>

      <div className="grid grid-cols-2 md:grid-cols-4 lg:grid-cols-6 gap-4">
        {gallery.map((item) => (
          <Card key={item.id} className="relative group overflow-hidden">
            <div className="aspect-square">
              {item.type === 'video' ? (
                <div className="w-full h-full bg-gray-200 dark:bg-gray-700 flex items-center justify-center relative">
                  {item.thumbnail_url ? (
                    <img src={item.thumbnail_url} alt="" className="w-full h-full object-cover" />
                  ) : item.url ? (
                    <video src={item.url} className="w-full h-full object-cover" />
                  ) : (
                    <Video className="h-12 w-12 text-gray-400" />
                  )}
                  <div className="absolute inset-0 flex items-center justify-center">
                    <div className="w-10 h-10 rounded-full bg-white/80 flex items-center justify-center">
                      <Video className="h-5 w-5 text-blue-600" />
                    </div>
                  </div>
                </div>
              ) : (
                <img src={item.url} alt={item.title || ''} className="w-full h-full object-cover" />
              )}
            </div>
            {item.end_date && <p className="text-xs text-orange-500 p-1 text-center">Expires: {new Date(item.end_date).toLocaleDateString()}</p>}
            <div className="absolute inset-0 bg-black/50 opacity-0 group-hover:opacity-100 transition-opacity flex items-center justify-center space-x-2">
              <button onClick={() => openModal(item)} className="p-2 rounded-full bg-white hover:bg-gray-100"><Edit className="h-4 w-4 text-gray-700" /></button>
              <button onClick={() => handleDelete(item)} className="p-2 rounded-full bg-white hover:bg-red-50"><Trash2 className="h-4 w-4 text-red-600" /></button>
            </div>
          </Card>
        ))}
      </div>

      {gallery.length === 0 && <Card className="p-8 text-center text-gray-500">No media items yet</Card>}

      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title={editItem ? 'Edit Media' : 'Add Media'} size="lg">
        <div className="space-y-5">
          <Input label="Title" value={formData.title} onChange={(e) => setFormData(prev => ({ ...prev, title: e.target.value }))} placeholder="Optional title" />
          <Select label="Type" value={formData.type} onChange={(e) => setFormData(prev => ({ ...prev, type: e.target.value as 'image' | 'video' }))} options={[{ value: 'image', label: 'Image' }, { value: 'video', label: 'Video' }]} />
          <Input label="End Date (Auto-remove)" type="date" value={formData.end_date} onChange={(e) => setFormData(prev => ({ ...prev, end_date: e.target.value }))} />

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">{formData.type === 'video' ? 'Video' : 'Image'}</p>
            {formData.type === 'video' ? (
              <>
                {videoPreview && <video src={videoPreview} controls className="max-w-full rounded-lg mb-2 max-h-48" />}
                <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700">
                  <Video className="h-4 w-4 mr-2" /> Upload Video
                  <input type="file" accept="video/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setMediaFile(file); setVideoPreview(URL.createObjectURL(file)); } }} />
                </label>
              </>
            ) : (
              <>
                {preview && <img src={preview} alt="" className="max-w-full rounded-lg mb-2 max-h-48" />}
                <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
                  <Upload className="h-4 w-4 mr-2" /> Upload Image
                  <input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setMediaFile(file); const reader = new FileReader(); reader.onload = (ev) => setPreview(ev.target?.result as string); reader.readAsDataURL(file); } }} />
                </label>
              </>
            )}
            {mediaFile && <p className="text-sm text-green-600 mt-2">Selected: {mediaFile.name}</p>}
          </div>

          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>{editItem ? 'Save Changes' : 'Add Media'}</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/LoginPage.tsx`

```tsx
import React, { useState } from 'react';
import { useNavigate, Navigate } from 'react-router-dom';
import { useAuth } from '../../contexts/AuthContext';
import { useTheme } from '../../contexts/ThemeContext';
import { Button, Input, Card } from '../../components/ui';
import { Lock, Eye, EyeOff, KeyRound, ArrowLeft } from 'lucide-react';

export function AdminLoginPage() {
  const { user, login } = useAuth();
  const { settings } = useTheme();
  const navigate = useNavigate();

  const [password, setPassword] = useState('');
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState('');
  const [showPassword, setShowPassword] = useState(false);
  const [showRecovery, setShowRecovery] = useState(false);
  const [recoveryEmail, setRecoveryEmail] = useState('');
  const [recoverySent, setRecoverySent] = useState(false);

  if (user) {
    return <Navigate to="/admin" replace />;
  }

  const handleLogin = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    setError('');

    const result = await login(password);

    if (result.success) {
      navigate('/admin');
    } else {
      setError(result.error || 'Invalid password');
    }

    setLoading(false);
  };

  const handleRecovery = async (e: React.FormEvent) => {
    e.preventDefault();
    setLoading(true);
    // Simulate sending recovery email
    await new Promise(resolve => setTimeout(resolve, 1500));
    setRecoverySent(true);
    setLoading(false);
  };

  if (showRecovery) {
    return (
      <div className="min-h-screen bg-gradient-to-br from-blue-900 via-blue-800 to-green-700 flex items-center justify-center p-4">
        <div className="w-full max-w-md">
          <Card className="p-8">
            <button onClick={() => { setShowRecovery(false); setRecoverySent(false); }} className="flex items-center text-gray-500 hover:text-gray-700 mb-6 text-sm">
              <ArrowLeft className="h-4 w-4 mr-1" /> Back to login
            </button>

            <div className="text-center mb-8">
              <div className="h-16 w-16 mx-auto rounded-full bg-gradient-to-br from-blue-600 to-green-500 flex items-center justify-center text-white font-bold text-2xl mb-4">
                <KeyRound className="h-8 w-8" />
              </div>
              <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Recover Password</h1>
              <p className="text-gray-500 text-sm mt-1">Enter your email to receive recovery instructions</p>
            </div>

            {recoverySent ? (
              <div className="text-center">
                <div className="bg-green-50 dark:bg-green-900/20 border border-green-200 dark:border-green-800 text-green-600 dark:text-green-400 px-4 py-3 rounded-lg text-sm mb-4">
                  If an admin account exists with this email, you will receive recovery instructions.
                </div>
                <Button onClick={() => { setShowRecovery(false); setRecoverySent(false); }} className="w-full">Back to Login</Button>
              </div>
            ) : (
              <form onSubmit={handleRecovery} className="space-y-5">
                <Input
                  label="Admin Email"
                  type="email"
                  value={recoveryEmail}
                  onChange={(e) => setRecoveryEmail(e.target.value)}
                  placeholder="admin@church.org"
                  required
                />

                <Button type="submit" className="w-full" loading={loading}>
                  Send Recovery Link
                </Button>
              </form>
            )}
          </Card>
        </div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-gradient-to-br from-blue-900 via-blue-800 to-green-700 flex items-center justify-center p-4">
      <div className="w-full max-w-md">
        <Card className="p-8">
          {/* Logo */}
          <div className="text-center mb-8">
            {settings.logo_url ? (
              <img src={settings.logo_url} alt="" className="h-16 w-16 mx-auto rounded-full mb-4" />
            ) : (
              <div className="h-16 w-16 mx-auto rounded-full bg-gradient-to-br from-blue-600 to-green-500 flex items-center justify-center text-white font-bold text-2xl mb-4">
                D
              </div>
            )}
            <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Admin Portal</h1>
            <p className="text-gray-500 text-sm mt-1">{settings.church_name}</p>
          </div>

          <form onSubmit={handleLogin} className="space-y-5">
            {error && (
              <div className="bg-red-50 dark:bg-red-900/20 border border-red-200 dark:border-red-800 text-red-600 dark:text-red-400 px-4 py-3 rounded-lg text-sm">
                {error}
              </div>
            )}

            <div>
              <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-1">
                Admin Password
              </label>
              <div className="relative">
                <Lock className="absolute left-3 top-1/2 transform -translate-y-1/2 h-5 w-5 text-gray-400" />
                <Input
                  type={showPassword ? 'text' : 'password'}
                  value={password}
                  onChange={(e) => setPassword(e.target.value)}
                  placeholder="Enter admin password"
                  className="pl-10 pr-10"
                  required
                />
                <button
                  type="button"
                  onClick={() => setShowPassword(!showPassword)}
                  className="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 hover:text-gray-600"
                >
                  {showPassword ? <EyeOff className="h-5 w-5" /> : <Eye className="h-5 w-5" />}
                </button>
              </div>
            </div>

            <Button type="submit" className="w-full" loading={loading}>
              Sign In
            </Button>

            <div className="text-center">
              <button
                type="button"
                onClick={() => setShowRecovery(true)}
                className="text-sm text-blue-600 hover:underline"
              >
                Forgot password?
              </button>
            </div>
          </form>
        </Card>
      </div>
    </div>
  );
}

```

## `./src/pages/admin/MembersPage.tsx`

```tsx
import React, { useState, useEffect, useMemo, useCallback } from 'react';
import { supabase, Member, Department, calculateAge, calculateFellowship } from '../../lib/supabase';
import { Card, Button, Input, Select, Badge, Modal, Tabs, LoadingSpinner } from '../../components/ui';
import { Users, Search, Download, Eye, Check, X, Edit, Trash2, UserPlus, Upload } from 'lucide-react';

export function MembersPage() {
  const [members, setMembers] = useState<Member[]>([]);
  const [departments, setDepartments] = useState<Department[]>([]);
  const [loading, setLoading] = useState(true);
  const [searchTerm, setSearchTerm] = useState('');
  const [statusFilter, setStatusFilter] = useState<'all' | 'pending' | 'approved' | 'rejected'>('all');
  const [selectedMember, setSelectedMember] = useState<Member | null>(null);
  const [showModal, setShowModal] = useState(false);
  const [showViewModal, setShowViewModal] = useState(false);
  const [editMode, setEditMode] = useState(false);
  const [photoFile, setPhotoFile] = useState<File | null>(null);
  const [photoPreview, setPhotoPreview] = useState<string>('');
  const [saving, setSaving] = useState(false);

  const [formData, setFormData] = useState({
    full_name: '',
    date_of_birth: '',
    gender: 'Male' as 'Male' | 'Female',
    phone: '',
    email: '',
    address: '',
    occupation: '',
    department_id: '',
    status: 'pending' as 'pending' | 'approved' | 'rejected'
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const [{ data: membersData }, { data: deptsData }] = await Promise.all([
      supabase.from('members').select('*, departments(*)').order('created_at', { ascending: false }),
      supabase.from('departments').select('*').order('name')
    ]);
    setMembers(membersData || []);
    setDepartments(deptsData || []);
    setLoading(false);
  };

  const filteredMembers = useMemo(() => {
    return members.filter(member => {
      const matchesSearch = member.full_name.toLowerCase().includes(searchTerm.toLowerCase()) ||
        member.member_id.toLowerCase().includes(searchTerm.toLowerCase()) ||
        (member.email?.toLowerCase() || '').includes(searchTerm.toLowerCase());
      const matchesStatus = statusFilter === 'all' || member.status === statusFilter;
      return matchesSearch && matchesStatus;
    });
  }, [members, searchTerm, statusFilter]);

  const handleStatusUpdate = async (member: Member, newStatus: 'approved' | 'rejected') => {
    const fellowship = calculateFellowship(member.date_of_birth, member.gender);
    await supabase.from('members').update({ status: newStatus, fellowship, updated_at: new Date().toISOString() }).eq('id', member.id);
    fetchData();
  };

  const handleDelete = async (member: Member) => {
    if (!confirm('Delete this member? It will be moved to trash.')) return;
    await supabase.from('deleted_records').insert({ table_name: 'members', record_id: member.id, record_data: member as unknown as Record<string, unknown> });
    await supabase.from('members').delete().eq('id', member.id);
    setShowViewModal(false);
    fetchData();
  };

  const openMemberModal = (member: Member | null) => {
    if (member) {
      setFormData({
        full_name: member.full_name,
        date_of_birth: member.date_of_birth,
        gender: member.gender,
        phone: member.phone || '',
        email: member.email || '',
        address: member.address || '',
        occupation: member.occupation || '',
        department_id: member.department_id || '',
        status: member.status
      });
      setPhotoPreview(member.photo_url || '');
      setEditMode(true);
    } else {
      setFormData({ full_name: '', date_of_birth: '', gender: 'Male', phone: '', email: '', address: '', occupation: '', department_id: '', status: 'pending' });
      setPhotoPreview('');
      setEditMode(false);
    }
    setPhotoFile(null);
    setSelectedMember(member);
    setShowModal(true);
  };

  const openViewModal = (member: Member) => {
    setSelectedMember(member);
    setShowViewModal(true);
  };

  const handlePhotoSelect = (file: File) => {
    setPhotoFile(file);
    const reader = new FileReader();
    reader.onload = (e) => setPhotoPreview(e.target?.result as string);
    reader.readAsDataURL(file);
  };

  const handleSave = async () => {
    setSaving(true);
    try {
      let photoUrl = selectedMember?.photo_url || '';

      if (photoFile) {
        const fileName = `members/${Date.now()}_${photoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, photoFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          photoUrl = urlData.publicUrl;
        }
      }

      const fellowship = calculateFellowship(formData.date_of_birth, formData.gender);

      const memberDataToSave = {
        full_name: formData.full_name,
        date_of_birth: formData.date_of_birth,
        gender: formData.gender,
        phone: formData.phone || null,
        email: formData.email || null,
        address: formData.address || null,
        occupation: formData.occupation || null,
        department_id: formData.department_id || null,
        photo_url: photoUrl || null,
        fellowship,
        status: formData.status
      };

      let error;
      if (editMode && selectedMember) {
        ({ error } = await supabase.from('members').update({ ...memberDataToSave, updated_at: new Date().toISOString() }).eq('id', selectedMember.id));
      } else {
        ({ error } = await supabase.from('members').insert({ ...memberDataToSave, status: 'pending' }));
      }

      if (error) throw error;

      setShowModal(false);
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving member: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const exportToCSV = useCallback(() => {
    if (filteredMembers.length === 0) return;
    const headers = ['ID', 'Name', 'DOB', 'Gender', 'Phone', 'Email', 'Occupation', 'Fellowship', 'Status'];
    const rows = filteredMembers.map(m => [m.member_id, m.full_name, m.date_of_birth, m.gender, m.phone || '', m.email || '', m.occupation || '', m.fellowship || '', m.status]);
    const csv = [headers.join(','), ...rows.map(r => r.join(','))].join('\n');
    const blob = new Blob([csv], { type: 'text/csv' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = `members_${new Date().toISOString().split('T')[0]}.csv`;
    a.click();
    URL.revokeObjectURL(url);
  }, [filteredMembers]);

  const tabs = [
    { id: 'all', label: 'All', count: members.length },
    { id: 'pending', label: 'Pending', count: members.filter(m => m.status === 'pending').length },
    { id: 'approved', label: 'Approved', count: members.filter(m => m.status === 'approved').length },
    { id: 'rejected', label: 'Rejected', count: members.filter(m => m.status === 'rejected').length }
  ];

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Members Management</h1>
          <p className="text-gray-500 text-sm mt-1">Manage church members and registrations</p>
        </div>
        <div className="flex space-x-2">
          <Button variant="outline" size="sm" onClick={exportToCSV}><Download className="h-4 w-4 mr-2" /> Export CSV</Button>
          <Button onClick={() => openMemberModal(null)}><UserPlus className="h-4 w-4 mr-2" /> Add Member</Button>
        </div>
      </div>

      <Card className="p-4">
        <div className="flex flex-col md:flex-row md:items-center gap-4">
          <div className="relative flex-1">
            <Search className="absolute left-3 top-1/2 transform -translate-y-1/2 h-5 w-5 text-gray-400" />
            <Input placeholder="Search by name, ID, or email..." value={searchTerm} onChange={(e) => setSearchTerm(e.target.value)} className="pl-10" />
          </div>
          <Tabs tabs={tabs} activeTab={statusFilter} onChange={(id) => setStatusFilter(id as typeof statusFilter)} />
        </div>
      </Card>

      <Card>
        <div className="overflow-x-auto">
          <table className="min-w-full divide-y divide-gray-200 dark:divide-gray-700">
            <thead className="bg-gray-50 dark:bg-gray-800">
              <tr>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Member</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">ID / Fellowship</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Contact</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Status</th>
                <th className="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase">Actions</th>
              </tr>
            </thead>
            <tbody className="bg-white dark:bg-gray-900 divide-y divide-gray-200 dark:divide-gray-700">
              {filteredMembers.map((member) => (
                <tr key={member.id} className="hover:bg-gray-50 dark:hover:bg-gray-800">
                  <td className="px-6 py-4 whitespace-nowrap">
                    <div className="flex items-center space-x-3">
                      <div className="w-10 h-10 rounded-full bg-gray-200 dark:bg-gray-700 overflow-hidden flex items-center justify-center">
                        {member.photo_url ? <img src={member.photo_url} alt="" className="w-full h-full object-cover" /> : <span className="text-gray-600 dark:text-gray-300 font-medium">{member.full_name.charAt(0)}</span>}
                      </div>
                      <div>
                        <p className="font-medium text-gray-900 dark:text-white">{member.full_name}</p>
                        <p className="text-sm text-gray-500">{calculateAge(member.date_of_birth)} years, {member.gender}</p>
                      </div>
                    </div>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <p className="text-sm text-gray-900 dark:text-white">{member.member_id}</p>
                    <p className="text-sm text-gray-500">{member.fellowship}</p>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <p className="text-sm text-gray-900 dark:text-white">{member.phone || '-'}</p>
                    <p className="text-sm text-gray-500">{member.email || '-'}</p>
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    {member.status === 'pending' && <Badge variant="warning">Pending</Badge>}
                    {member.status === 'approved' && <Badge variant="success">Approved</Badge>}
                    {member.status === 'rejected' && <Badge variant="error">Rejected</Badge>}
                  </td>
                  <td className="px-6 py-4 whitespace-nowrap">
                    <div className="flex space-x-1">
                      <button onClick={() => openViewModal(member)} className="p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700" title="View"><Eye className="h-4 w-4" /></button>
                      <button onClick={() => openMemberModal(member)} className="p-2 rounded-lg text-gray-500 hover:bg-gray-100 dark:hover:bg-gray-700" title="Edit"><Edit className="h-4 w-4" /></button>
                      {member.status === 'pending' && (
                        <>
                          <button onClick={() => handleStatusUpdate(member, 'approved')} className="p-2 rounded-lg text-green-600 hover:bg-green-50 dark:hover:bg-green-900/20" title="Approve"><Check className="h-4 w-4" /></button>
                          <button onClick={() => handleStatusUpdate(member, 'rejected')} className="p-2 rounded-lg text-red-600 hover:bg-red-50 dark:hover:bg-red-900/20" title="Reject"><X className="h-4 w-4" /></button>
                        </>
                      )}
                      <button onClick={() => handleDelete(member)} className="p-2 rounded-lg text-red-600 hover:bg-red-50 dark:hover:bg-red-900/20" title="Delete"><Trash2 className="h-4 w-4" /></button>
                    </div>
                  </td>
                </tr>
              ))}
            </tbody>
          </table>
        </div>
      </Card>

      {/* View Modal */}
      <Modal isOpen={showViewModal} onClose={() => setShowViewModal(false)} title="Member Details" size="md">
        {selectedMember && (
          <div className="space-y-4">
            <div className="flex items-center space-x-4">
              <div className="w-20 h-20 rounded-full bg-gray-200 dark:bg-gray-700 overflow-hidden">
                {selectedMember.photo_url ? <img src={selectedMember.photo_url} alt="" className="w-full h-full object-cover" /> : <span className="w-full h-full flex items-center justify-center text-2xl text-gray-500">{selectedMember.full_name.charAt(0)}</span>}
              </div>
              <div>
                <h3 className="text-xl font-bold text-gray-900 dark:text-white">{selectedMember.full_name}</h3>
                <p className="text-gray-500">{selectedMember.member_id}</p>
                <Badge variant={selectedMember.status === 'approved' ? 'success' : selectedMember.status === 'pending' ? 'warning' : 'error'} className="mt-1">{selectedMember.status}</Badge>
              </div>
            </div>
            <div className="grid grid-cols-2 gap-3 text-sm">
              <div><span className="text-gray-500">DOB:</span> <span className="text-gray-900 dark:text-white">{selectedMember.date_of_birth}</span></div>
              <div><span className="text-gray-500">Gender:</span> <span className="text-gray-900 dark:text-white">{selectedMember.gender}</span></div>
              <div><span className="text-gray-500">Phone:</span> <span className="text-gray-900 dark:text-white">{selectedMember.phone || 'N/A'}</span></div>
              <div><span className="text-gray-500">Email:</span> <span className="text-gray-900 dark:text-white">{selectedMember.email || 'N/A'}</span></div>
              <div><span className="text-gray-500">Occupation:</span> <span className="text-gray-900 dark:text-white">{selectedMember.occupation || 'N/A'}</span></div>
              <div><span className="text-gray-500">Fellowship:</span> <span className="text-gray-900 dark:text-white">{selectedMember.fellowship || 'N/A'}</span></div>
              <div className="col-span-2"><span className="text-gray-500">Address:</span> <span className="text-gray-900 dark:text-white">{selectedMember.address || 'N/A'}</span></div>
            </div>
            <div className="flex gap-2 pt-4 border-t dark:border-gray-700">
              {selectedMember.status === 'pending' && (
                <>
                  <Button onClick={() => { handleStatusUpdate(selectedMember, 'approved'); setShowViewModal(false); }} className="flex-1"><Check className="h-4 w-4 mr-2" /> Approve</Button>
                  <Button variant="outline" onClick={() => { handleStatusUpdate(selectedMember, 'rejected'); setShowViewModal(false); }} className="flex-1"><X className="h-4 w-4 mr-2" /> Reject</Button>
                </>
              )}
              <Button variant="outline" onClick={() => { setShowViewModal(false); openMemberModal(selectedMember); }}><Edit className="h-4 w-4 mr-2" /> Edit</Button>
              <Button variant="danger" onClick={() => handleDelete(selectedMember)}><Trash2 className="h-4 w-4" /></Button>
            </div>
          </div>
        )}
      </Modal>

      {/* Edit/Add Modal */}
      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title={editMode ? 'Edit Member' : 'Add New Member'} size="lg">
        <div className="space-y-5">
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Full Name *" value={formData.full_name} onChange={(e) => setFormData(prev => ({ ...prev, full_name: e.target.value }))} />
            <Input label="Date of Birth *" type="date" value={formData.date_of_birth} onChange={(e) => setFormData(prev => ({ ...prev, date_of_birth: e.target.value }))} />
          </div>
          <div className="grid md:grid-cols-2 gap-4">
            <Select label="Gender *" value={formData.gender} onChange={(e) => setFormData(prev => ({ ...prev, gender: e.target.value as 'Male' | 'Female' }))} options={[{ value: 'Male', label: 'Male' }, { value: 'Female', label: 'Female' }]} />
            <Input label="Occupation" value={formData.occupation} onChange={(e) => setFormData(prev => ({ ...prev, occupation: e.target.value }))} placeholder="e.g., Teacher, Engineer" />
          </div>
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Phone" value={formData.phone} onChange={(e) => setFormData(prev => ({ ...prev, phone: e.target.value }))} />
            <Input label="Email" type="email" value={formData.email} onChange={(e) => setFormData(prev => ({ ...prev, email: e.target.value }))} />
          </div>
          <Input label="Address" value={formData.address} onChange={(e) => setFormData(prev => ({ ...prev, address: e.target.value }))} />
          <Select label="Department" value={formData.department_id} onChange={(e) => setFormData(prev => ({ ...prev, department_id: e.target.value }))} options={[{ value: '', label: 'None' }, ...departments.map(d => ({ value: d.id, label: d.name }))]} />

          <div className="flex items-center justify-center">
            <div className="relative">
              <div className="w-24 h-24 rounded-full overflow-hidden bg-gray-200 dark:bg-gray-700 flex items-center justify-center">
                {photoPreview ? <img src={photoPreview} alt="" className="w-full h-full object-cover" /> : <Upload className="h-8 w-8 text-gray-400" />}
              </div>
              <label className="absolute inset-0 cursor-pointer"><input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) handlePhotoSelect(file); }} /></label>
            </div>
          </div>

          {editMode && (
            <Select label="Status" value={formData.status} onChange={(e) => setFormData(prev => ({ ...prev, status: e.target.value as typeof formData.status }))} options={[{ value: 'pending', label: 'Pending' }, { value: 'approved', label: 'Approved' }, { value: 'rejected', label: 'Rejected' }]} />
          )}

          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>{editMode ? 'Save Changes' : 'Add Member'}</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/MessagesPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, ContactMessage } from '../../lib/supabase';
import { Card, Badge, LoadingSpinner } from '../../components/ui';
import { Mail, MailOpen, CheckCircle } from 'lucide-react';

export function MessagesPage() {
  const [messages, setMessages] = useState<ContactMessage[]>([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('contact_messages').select('*').order('created_at', { ascending: false });
    setMessages(data || []);
    setLoading(false);
  };

  const markAsRead = async (id: string) => {
    await supabase.from('contact_messages').update({ status: 'read' }).eq('id', id);
    fetchData();
  };

  const markAsReplied = async (id: string) => {
    await supabase.from('contact_messages').update({ status: 'replied' }).eq('id', id);
    fetchData();
  };

  const unreadCount = messages.filter(m => m.status === 'unread').length;

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Messages</h1>
          <p className="text-gray-500 text-sm mt-1">{unreadCount} unread message{unreadCount !== 1 ? 's' : ''}</p>
        </div>
      </div>

      <Card>
        <div className="divide-y divide-gray-200 dark:divide-gray-700">
          {messages.length > 0 ? messages.map((msg) => (
            <div key={msg.id} className={`p-4 ${msg.status === 'unread' ? 'bg-blue-50 dark:bg-blue-900/10' : ''}`}>
              <div className="flex items-start justify-between">
                <div className="flex-1">
                  <div className="flex items-center space-x-2 mb-1">
                    {msg.status === 'unread' && <Mail className="h-4 w-4 text-blue-600" />}
                    {msg.status === 'read' && <MailOpen className="h-4 w-4 text-gray-400" />}
                    {msg.status === 'replied' && <CheckCircle className="h-4 w-4 text-green-600" />}
                    <h3 className="font-medium text-gray-900 dark:text-white">{msg.name}</h3>
                    <Badge variant={msg.status === 'unread' ? 'info' : msg.status === 'read' ? 'default' : 'success'}>{msg.status}</Badge>
                  </div>
                  <p className="text-sm text-gray-500">{msg.email} {msg.phone && `| ${msg.phone}`}</p>
                  {msg.subject && <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mt-1">{msg.subject}</p>}
                  <p className="text-sm text-gray-600 dark:text-gray-400 mt-2">{msg.message}</p>
                  <p className="text-xs text-gray-400 mt-2">{new Date(msg.created_at).toLocaleString()}</p>
                </div>
                <div className="flex space-x-2 ml-4">
                  {msg.status === 'unread' && <button onClick={() => markAsRead(msg.id)} className="text-xs text-blue-600 hover:underline">Mark as read</button>}
                  {(msg.status === 'unread' || msg.status === 'read') && <button onClick={() => markAsReplied(msg.id)} className="text-xs text-green-600 hover:underline">Mark as replied</button>}
                </div>
              </div>
            </div>
          )) : <div className="p-8 text-center text-gray-500">No messages yet.</div>}
        </div>
      </Card>
    </div>
  );
}

```

## `./src/pages/admin/PastorsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, PastorHistory } from '../../lib/supabase';
import { Card, Button, Input, TextArea, Modal, LoadingSpinner } from '../../components/ui';
import { User, Plus, Edit, Trash2, Upload } from 'lucide-react';

export function PastorsPage() {
  const [pastors, setPastors] = useState<PastorHistory[]>([]);
  const [loading, setLoading] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [editItem, setEditItem] = useState<PastorHistory | null>(null);
  const [saving, setSaving] = useState(false);
  const [photoFile, setPhotoFile] = useState<File | null>(null);
  const [photoPreview, setPhotoPreview] = useState('');

  const [formData, setFormData] = useState({
    full_name: '',
    title: '',
    role: '',
    bio: '',
    quote: '',
    start_date: '',
    end_date: '',
    current: false
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('pastor_history').select('*').order('current', { ascending: false }).order('start_date', { ascending: false });
    setPastors(data || []);
    setLoading(false);
  };

  const openModal = (item: PastorHistory | null) => {
    if (item) {
      setFormData({
        full_name: item.full_name,
        title: item.title || '',
        role: item.role,
        bio: item.bio || '',
        quote: item.quote || '',
        start_date: item.start_date || '',
        end_date: item.end_date || '',
        current: item.current
      });
      setPhotoPreview(item.photo_url || '');
      setEditItem(item);
    } else {
      setFormData({ full_name: '', title: '', role: '', bio: '', quote: '', start_date: '', end_date: '', current: false });
      setPhotoPreview('');
      setEditItem(null);
    }
    setPhotoFile(null);
    setShowModal(true);
  };

  const handleSave = async () => {
    if (!formData.full_name || !formData.role) { alert('Name and role are required'); return; }
    setSaving(true);

    try {
      let photoUrl = editItem?.photo_url || '';

      if (photoFile) {
        const fileName = `pastors/${Date.now()}_${photoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, photoFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          photoUrl = urlData.publicUrl;
        }
      }

      // If marking as current, unmark others
      if (formData.current) {
        await supabase.from('pastor_history').update({ current: false }).eq('current', true);
      }

      const dataToSave = {
        full_name: formData.full_name,
        title: formData.title || null,
        role: formData.role,
        bio: formData.bio || null,
        quote: formData.quote || null,
        photo_url: photoUrl || null,
        start_date: formData.start_date || null,
        end_date: formData.current ? null : formData.end_date || null,
        current: formData.current
      };

      let error;
      if (editItem) {
        ({ error } = await supabase.from('pastor_history').update({ ...dataToSave, updated_at: new Date().toISOString() }).eq('id', editItem.id));
      } else {
        ({ error } = await supabase.from('pastor_history').insert(dataToSave));
      }

      if (error) throw error;

      setShowModal(false);
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const handleDelete = async (item: PastorHistory) => {
    if (!confirm('Delete this pastor record?')) return;
    await supabase.from('deleted_records').insert({ table_name: 'pastor_history', record_id: item.id, record_data: item as unknown as Record<string, unknown> });
    await supabase.from('pastor_history').delete().eq('id', item.id);
    fetchData();
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Pastors & Leadership</h1>
          <p className="text-gray-500 text-sm mt-1">Manage church leadership history</p>
        </div>
        <Button onClick={() => openModal(null)}><Plus className="h-4 w-4 mr-2" /> Add Pastor</Button>
      </div>

      <div className="grid md:grid-cols-2 lg:grid-cols-3 gap-6">
        {pastors.map((pastor) => (
          <Card key={pastor.id} className="overflow-hidden">
            <div className="h-48 relative">
              {pastor.photo_url ? (
                <img src={pastor.photo_url} alt={pastor.full_name} className="w-full h-full object-cover" />
              ) : (
                <div className="w-full h-full bg-gradient-to-br from-blue-600 to-green-600 flex items-center justify-center text-white text-4xl font-bold">{pastor.full_name.charAt(0)}</div>
              )}
              {pastor.current && <div className="absolute top-4 right-4 bg-green-500 text-white px-3 py-1 rounded-full text-xs font-medium">Current</div>}
            </div>
            <div className="p-4">
              <h3 className="font-bold text-gray-900 dark:text-white">{pastor.full_name}</h3>
              <p className="text-blue-600 dark:text-blue-400 text-sm">{pastor.title} {pastor.role}</p>
              {pastor.bio && <p className="text-gray-500 text-sm mt-2 line-clamp-2">{pastor.bio}</p>}
              {pastor.quote && <p className="text-gray-400 text-xs mt-2 italic">"{pastor.quote}"</p>}
              <div className="flex justify-between items-center mt-4">
                <span className="text-xs text-gray-500">{pastor.start_date ? new Date(pastor.start_date).getFullYear() : ''} - {pastor.current ? 'Present' : pastor.end_date ? new Date(pastor.end_date).getFullYear() : ''}</span>
                <div className="flex space-x-2">
                  <button onClick={() => openModal(pastor)} className="p-1.5 rounded hover:bg-gray-100 dark:hover:bg-gray-700"><Edit className="h-4 w-4 text-gray-500" /></button>
                  <button onClick={() => handleDelete(pastor)} className="p-1.5 rounded hover:bg-red-50 dark:hover:bg-red-900/20"><Trash2 className="h-4 w-4 text-red-500" /></button>
                </div>
              </div>
            </div>
          </Card>
        ))}
      </div>

      {pastors.length === 0 && <Card className="p-8 text-center text-gray-500">No pastor records yet. Click "Add Pastor" to add one.</Card>}

      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title={editItem ? 'Edit Pastor' : 'Add Pastor'} size="lg">
        <div className="space-y-5">
          <div className="flex justify-center">
            <div className="relative">
              <div className="w-32 h-32 rounded-full overflow-hidden bg-gray-200 dark:bg-gray-700 flex items-center justify-center">
                {photoPreview ? <img src={photoPreview} alt="" className="w-full h-full object-cover" /> : <User className="h-12 w-12 text-gray-400" />}
              </div>
              <label className="absolute inset-0 cursor-pointer rounded-full">
                <input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setPhotoFile(file); const reader = new FileReader(); reader.onload = (ev) => setPhotoPreview(ev.target?.result as string); reader.readAsDataURL(file); } }} />
              </label>
            </div>
          </div>
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Full Name *" value={formData.full_name} onChange={(e) => setFormData(prev => ({ ...prev, full_name: e.target.value }))} />
            <Input label="Title" value={formData.title} onChange={(e) => setFormData(prev => ({ ...prev, title: e.target.value }))} placeholder="e.g., Pastor, Rev., Bishop" />
          </div>
          <Input label="Role *" value={formData.role} onChange={(e) => setFormData(prev => ({ ...prev, role: e.target.value }))} placeholder="e.g., Senior Pastor, Youth Pastor" />
          <TextArea label="Bio" value={formData.bio} onChange={(e) => setFormData(prev => ({ ...prev, bio: e.target.value }))} rows={3} />
          <Input label="Quote" value={formData.quote} onChange={(e) => setFormData(prev => ({ ...prev, quote: e.target.value }))} />
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Start Date" type="date" value={formData.start_date} onChange={(e) => setFormData(prev => ({ ...prev, start_date: e.target.value }))} />
            <Input label="End Date" type="date" value={formData.end_date} onChange={(e) => setFormData(prev => ({ ...prev, end_date: e.target.value }))} disabled={formData.current} />
          </div>
          <div className="flex items-center space-x-2">
            <input type="checkbox" id="current" checked={formData.current} onChange={(e) => setFormData(prev => ({ ...prev, current: e.target.checked, end_date: e.target.checked ? '' : prev.end_date }))} className="rounded" />
            <label htmlFor="current" className="text-sm text-gray-700 dark:text-gray-300">Currently serving</label>
          </div>
          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>{editItem ? 'Save Changes' : 'Add Pastor'}</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/SermonsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, Sermon } from '../../lib/supabase';
import { Card, Button, Input, TextArea, Modal, Badge, LoadingSpinner } from '../../components/ui';
import { Mic, Plus, Edit, Trash2, Video, Upload } from 'lucide-react';

export function SermonsAdminPage() {
  const [sermons, setSermons] = useState<Sermon[]>([]);
  const [loading, setLoading] = useState(true);
  const [showModal, setShowModal] = useState(false);
  const [editSermon, setEditSermon] = useState<Sermon | null>(null);
  const [saving, setSaving] = useState(false);
  const [videoFile, setVideoFile] = useState<File | null>(null);
  const [audioFile, setAudioFile] = useState<File | null>(null);
  const [thumbnailFile, setThumbnailFile] = useState<File | null>(null);
  const [videoPreview, setVideoPreview] = useState('');

  const [formData, setFormData] = useState({
    title: '',
    speaker: '',
    scripture_reference: '',
    sermon_date: '',
    end_date: '',
    description: '',
    video_url: '',
    audio_url: ''
  });

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('sermons').select('*').order('sermon_date', { ascending: false });
    setSermons(data || []);
    setLoading(false);
  };

  const openModal = (sermon: Sermon | null) => {
    if (sermon) {
      setFormData({
        title: sermon.title,
        speaker: sermon.speaker,
        scripture_reference: sermon.scripture_reference || '',
        sermon_date: sermon.sermon_date,
        end_date: sermon.end_date || '',
        description: sermon.description || '',
        video_url: sermon.video_url || '',
        audio_url: sermon.audio_url || ''
      });
      setVideoPreview(sermon.video_url || '');
      setEditSermon(sermon);
    } else {
      setFormData({ title: '', speaker: '', scripture_reference: '', sermon_date: '', end_date: '', description: '', video_url: '', audio_url: '' });
      setVideoPreview('');
      setEditSermon(null);
    }
    setVideoFile(null);
    setAudioFile(null);
    setThumbnailFile(null);
    setShowModal(true);
  };

  const handleSave = async () => {
    if (!formData.title || !formData.speaker || !formData.sermon_date) { alert('Title, speaker and date are required'); return; }
    setSaving(true);

    try {
      let videoUrl = formData.video_url || '';
      let audioUrl = formData.audio_url || '';
      let thumbnailUrl = editSermon?.thumbnail_url || '';

      if (videoFile) {
        const fileName = `videos/sermons/${Date.now()}_${videoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, videoFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          videoUrl = urlData.publicUrl;
        }
      }

      if (audioFile) {
        const fileName = `audio/sermons/${Date.now()}_${audioFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, audioFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          audioUrl = urlData.publicUrl;
        }
      }

      if (thumbnailFile) {
        const fileName = `sermons/${Date.now()}_${thumbnailFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, thumbnailFile);
        if (!uploadError && uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          thumbnailUrl = urlData.publicUrl;
        }
      }

      const dataToSave = {
        title: formData.title,
        speaker: formData.speaker,
        scripture_reference: formData.scripture_reference || null,
        sermon_date: formData.sermon_date,
        end_date: formData.end_date || null,
        description: formData.description || null,
        video_url: videoUrl || null,
        audio_url: audioUrl || null,
        thumbnail_url: thumbnailUrl || null
      };

      let error;
      if (editSermon) {
        ({ error } = await supabase.from('sermons').update({ ...dataToSave, updated_at: new Date().toISOString() }).eq('id', editSermon.id));
      } else {
        ({ error } = await supabase.from('sermons').insert(dataToSave));
      }

      if (error) throw error;

      setShowModal(false);
      fetchData();
    } catch (err: unknown) {
      console.error('Save error:', err);
      alert('Error saving sermon: ' + (err instanceof Error ? err.message : 'Unknown error'));
    } finally {
      setSaving(false);
    }
  };

  const handleDelete = async (sermon: Sermon) => {
    if (!confirm('Delete this sermon? It will be moved to trash.')) return;
    await supabase.from('deleted_records').insert({ table_name: 'sermons', record_id: sermon.id, record_data: sermon as unknown as Record<string, unknown> });
    await supabase.from('sermons').delete().eq('id', sermon.id);
    fetchData();
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Sermons</h1>
          <p className="text-gray-500 text-sm mt-1">Manage sermon recordings</p>
        </div>
        <Button onClick={() => openModal(null)}><Plus className="h-4 w-4 mr-2" /> Add Sermon</Button>
      </div>

      <Card>
        <div className="divide-y divide-gray-200 dark:divide-gray-700">
          {sermons.length > 0 ? sermons.map((sermon) => (
            <div key={sermon.id} className="p-4 flex items-center justify-between hover:bg-gray-50 dark:hover:bg-gray-800">
              <div className="flex items-center space-x-4">
                {sermon.thumbnail_url ? (
                  <img src={sermon.thumbnail_url} alt="" className="w-20 h-16 rounded-lg object-cover" />
                ) : (
                  <div className="w-20 h-16 rounded-lg bg-gradient-to-br from-purple-600 to-blue-600 flex items-center justify-center text-white"><Mic className="h-8 w-8" /></div>
                )}
                <div>
                  <h3 className="font-medium text-gray-900 dark:text-white">{sermon.title}</h3>
                  <p className="text-sm text-gray-500">{sermon.speaker} | {new Date(sermon.sermon_date).toLocaleDateString()}</p>
                  {sermon.end_date && <p className="text-xs text-orange-500">Expires: {new Date(sermon.end_date).toLocaleDateString()}</p>}
                  <div className="flex gap-2 mt-1">
                    {sermon.video_url && <Badge variant="info">Video</Badge>}
                    {sermon.audio_url && <Badge variant="success">Audio</Badge>}
                  </div>
                </div>
              </div>
              <div className="flex space-x-2">
                <button onClick={() => openModal(sermon)} className="p-2 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700"><Edit className="h-4 w-4 text-gray-600" /></button>
                <button onClick={() => handleDelete(sermon)} className="p-2 rounded-lg hover:bg-red-50 dark:hover:bg-red-900/20"><Trash2 className="h-4 w-4 text-red-600" /></button>
              </div>
            </div>
          )) : <div className="p-8 text-center text-gray-500">No sermons yet</div>}
        </div>
      </Card>

      <Modal isOpen={showModal} onClose={() => setShowModal(false)} title={editSermon ? 'Edit Sermon' : 'New Sermon'} size="lg">
        <div className="space-y-5">
          <Input label="Title *" value={formData.title} onChange={(e) => setFormData(prev => ({ ...prev, title: e.target.value }))} />
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Speaker *" value={formData.speaker} onChange={(e) => setFormData(prev => ({ ...prev, speaker: e.target.value }))} />
            <Input label="Scripture Reference" value={formData.scripture_reference} onChange={(e) => setFormData(prev => ({ ...prev, scripture_reference: e.target.value }))} placeholder="e.g., John 3:16" />
          </div>
          <div className="grid md:grid-cols-2 gap-4">
            <Input label="Sermon Date *" type="date" value={formData.sermon_date} onChange={(e) => setFormData(prev => ({ ...prev, sermon_date: e.target.value }))} />
            <Input label="End Date (Auto-remove)" type="date" value={formData.end_date} onChange={(e) => setFormData(prev => ({ ...prev, end_date: e.target.value }))} />
          </div>
          <TextArea label="Description" value={formData.description} onChange={(e) => setFormData(prev => ({ ...prev, description: e.target.value }))} rows={3} />

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Video</p>
            {videoPreview && videoPreview.startsWith('blob:') && <video src={videoPreview} controls className="max-w-full rounded-lg mb-2 max-h-48" />}
            <div className="flex gap-2 flex-wrap">
              <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-purple-600 text-white rounded-lg hover:bg-purple-700">
                <Video className="h-4 w-4 mr-2" /> Upload Video
                <input type="file" accept="video/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) { setVideoFile(file); setVideoPreview(URL.createObjectURL(file)); } }} />
              </label>
              <Input value={formData.video_url} onChange={(e) => { setFormData(prev => ({ ...prev, video_url: e.target.value })); setVideoPreview(e.target.value); }} placeholder="Or YouTube URL" className="flex-1 min-w-48" />
            </div>
          </div>

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Audio</p>
            <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-blue-600 text-white rounded-lg hover:bg-blue-700">
              <Upload className="h-4 w-4 mr-2" /> Upload Audio
              <input type="file" accept="audio/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) setAudioFile(file); }} />
            </label>
            {audioFile && <p className="text-sm text-green-600 mt-2">Selected: {audioFile.name}</p>}
          </div>

          <div className="border-2 border-dashed border-gray-300 dark:border-gray-600 rounded-lg p-4">
            <p className="text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Thumbnail Image</p>
            <label className="cursor-pointer inline-flex items-center px-4 py-2 bg-gray-600 text-white rounded-lg hover:bg-gray-700">
              <Upload className="h-4 w-4 mr-2" /> Upload Thumbnail
              <input type="file" accept="image/*" className="hidden" onChange={(e) => { const file = e.target.files?.[0]; if (file) setThumbnailFile(file); }} />
            </label>
            {thumbnailFile && <p className="text-sm text-green-600 mt-2">Selected: {thumbnailFile.name}</p>}
          </div>

          <div className="flex justify-end space-x-3 pt-4">
            <Button variant="outline" onClick={() => setShowModal(false)}>Cancel</Button>
            <Button onClick={handleSave} loading={saving}>{editSermon ? 'Save Changes' : 'Create Sermon'}</Button>
          </div>
        </div>
      </Modal>
    </div>
  );
}

```

## `./src/pages/admin/SettingsPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { useTheme } from '../../contexts/ThemeContext';
import { useAuth } from '../../contexts/AuthContext';
import { supabase } from '../../lib/supabase';
import { Card, Button, Input, TextArea, LoadingSpinner } from '../../components/ui';
import { Settings, Save, Upload, Palette, Globe, Shield, Bell, Database } from 'lucide-react';

export function SettingsPage() {
  const { settings, refreshSettings, darkMode, toggleDarkMode } = useTheme();
  const { user } = useAuth();
  const [loading, setLoading] = useState(false);
  const [activeTab, setActiveTab] = useState('general');
  const [logoFile, setLogoFile] = useState<File | null>(null);
  const [logoPreview, setLogoPreview] = useState<string>('');

  const [formData, setFormData] = useState({
    church_name: '',
    motto: '',
    slogan: '',
    address: '',
    phone: '',
    email: '',
    website: '',
    theme_of_year: '',
    verse_of_year: '',
    welcome_message: '',
    service_times: [] as { day: string; time: string; name: string }[],
    primary_color: '#1e40af',
    secondary_color: '#ffffff',
    accent_color: '#16a34a',
    social_facebook: '',
    social_youtube: '',
    social_instagram: '',
    social_whatsapp: ''
  });

  useEffect(() => {
    if (settings) {
      setFormData({
        church_name: settings.church_name || '',
        motto: settings.motto || '',
        slogan: settings.slogan || '',
        address: settings.address || '',
        phone: settings.phone || '',
        email: settings.email || '',
        website: settings.website || '',
        theme_of_year: settings.theme_of_year || '',
        verse_of_year: settings.verse_of_year || '',
        welcome_message: settings.welcome_message || '',
        service_times: settings.service_times || [],
        primary_color: settings.primary_color || '#1e40af',
        secondary_color: settings.secondary_color || '#ffffff',
        accent_color: settings.accent_color || '#16a34a',
        social_facebook: settings.social_links?.facebook || '',
        social_youtube: settings.social_links?.youtube || '',
        social_instagram: settings.social_links?.instagram || '',
        social_whatsapp: settings.social_links?.whatsapp || ''
      });
    }
  }, [settings]);

  const handleSave = async () => {
    setLoading(true);

    try {
      let logoUrl = settings?.logo_url || '';
      if (logoFile) {
        const fileName = `logo/${Date.now()}_${logoFile.name}`;
        const { data: uploadData, error: uploadError } = await supabase.storage.from('images').upload(fileName, logoFile, { upsert: true });
        if (uploadError) {
          console.error('Logo upload error:', uploadError);
        } else if (uploadData) {
          const { data: urlData } = supabase.storage.from('images').getPublicUrl(uploadData.path);
          logoUrl = urlData.publicUrl;
        }
      }

      const social_links = {
        facebook: formData.social_facebook,
        youtube: formData.social_youtube,
        instagram: formData.social_instagram,
        whatsapp: formData.social_whatsapp
      };

      const { error: updateError } = await supabase
        .from('church_settings')
        .update({
          church_name: formData.church_name,
          motto: formData.motto,
          slogan: formData.slogan,
          address: formData.address,
          phone: formData.phone,
          email: formData.email,
          website: formData.website,
          theme_of_year: formData.theme_of_year,
          verse_of_year: formData.verse_of_year,
          welcome_message: formData.welcome_message,
          service_times: formData.service_times,
          primary_color: formData.primary_color,
          secondary_color: formData.secondary_color,
          accent_color: formData.accent_color,
          social_links,
          logo_url: logoUrl,
          updated_at: new Date().toISOString()
        })
        .neq('id', '00000000-0000-0000-0000-000000000000');

      if (updateError) {
        console.error('Settings update error:', updateError);
        alert('Error saving settings: ' + updateError.message);
      } else {
        await refreshSettings();
        setLogoFile(null);
        setLogoPreview('');
        alert('Settings saved successfully!');
      }
    } catch (err) {
      console.error('Save error:', err);
      alert('Error saving settings. Please try again.');
    } finally {
      setLoading(false);
    }
  };

  const addServiceTime = () => {
    setFormData(prev => ({
      ...prev,
      service_times: [...prev.service_times, { day: 'Sunday', time: '10:00 AM', name: 'Service' }]
    }));
  };

  const removeServiceTime = (index: number) => {
    setFormData(prev => ({
      ...prev,
      service_times: prev.service_times.filter((_, i) => i !== index)
    }));
  };

  const updateServiceTime = (index: number, field: string, value: string) => {
    setFormData(prev => ({
      ...prev,
      service_times: prev.service_times.map((s, i) =>
        i === index ? { ...s, [field]: value } : s
      )
    }));
  };

  const tabs = [
    { id: 'general', label: 'General', icon: Settings },
    { id: 'appearance', label: 'Appearance', icon: Palette },
    { id: 'services', label: 'Service Times', icon: Globe },
    { id: 'social', label: 'Social Links', icon: Globe },
    { id: 'security', label: 'Security', icon: Shield }
  ];

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Settings</h1>
          <p className="text-gray-500 text-sm mt-1">Configure your church website settings</p>
        </div>
        <Button onClick={handleSave} loading={loading}>
          <Save className="h-4 w-4 mr-2" /> Save Changes
        </Button>
      </div>

      <div className="grid lg:grid-cols-4 gap-6">
        {/* Sidebar */}
        <Card className="p-4">
          <nav className="space-y-1">
            {tabs.map((tab) => (
              <button
                key={tab.id}
                onClick={() => setActiveTab(tab.id)}
                className={`w-full flex items-center space-x-3 px-4 py-3 rounded-lg transition-colors ${
                  activeTab === tab.id
                    ? 'bg-blue-50 dark:bg-blue-900/20 text-blue-600'
                    : 'text-gray-600 hover:bg-gray-100 dark:hover:bg-gray-700'
                }`}
              >
                <tab.icon className="h-5 w-5" />
                <span className="font-medium">{tab.label}</span>
              </button>
            ))}
          </nav>
        </Card>

        {/* Content */}
        <div className="lg:col-span-3">
          <Card className="p-6">
            {activeTab === 'general' && (
              <div className="space-y-5">
                <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">General Settings</h2>

                {/* Logo Upload */}
                <div className="flex items-center space-x-6">
                  <div className="w-20 h-20 rounded-full bg-gray-200 dark:bg-gray-700 overflow-hidden flex items-center justify-center">
                    {logoPreview || settings?.logo_url ? (
                      <img src={logoPreview || settings?.logo_url} alt="Logo" className="w-full h-full object-cover" />
                    ) : (
                      <span className="text-2xl font-bold text-gray-400">D</span>
                    )}
                  </div>
                  <div>
                    <label className="cursor-pointer">
                      <Button variant="outline" size="sm">
                        <Upload className="h-4 w-4 mr-2" /> Upload Logo
                      </Button>
                      <input
                        type="file"
                        accept="image/*"
                        className="hidden"
                        onChange={(e) => {
                          const file = e.target.files?.[0];
                          if (file) {
                            setLogoFile(file);
                            const reader = new FileReader();
                            reader.onload = (ev) => setLogoPreview(ev.target?.result as string);
                            reader.readAsDataURL(file);
                          }
                        }}
                      />
                    </label>
                    <p className="text-xs text-gray-500 mt-1">Recommended: Square image, 200x200px</p>
                    {logoFile && <p className="text-xs text-green-600 mt-1">Selected: {logoFile.name}</p>}
                  </div>
                </div>

                <Input
                  label="Church Name"
                  value={formData.church_name}
                  onChange={(e) => setFormData(prev => ({ ...prev, church_name: e.target.value }))}
                />

                <div className="grid md:grid-cols-2 gap-4">
                  <Input
                    label="Motto"
                    value={formData.motto}
                    onChange={(e) => setFormData(prev => ({ ...prev, motto: e.target.value }))}
                  />
                  <Input
                    label="Slogan"
                    value={formData.slogan}
                    onChange={(e) => setFormData(prev => ({ ...prev, slogan: e.target.value }))}
                  />
                </div>

                <Input
                  label="Address"
                  value={formData.address}
                  onChange={(e) => setFormData(prev => ({ ...prev, address: e.target.value }))}
                />

                <div className="grid md:grid-cols-2 gap-4">
                  <Input
                    label="Phone"
                    value={formData.phone}
                    onChange={(e) => setFormData(prev => ({ ...prev, phone: e.target.value }))}
                  />
                  <Input
                    label="Email"
                    type="email"
                    value={formData.email}
                    onChange={(e) => setFormData(prev => ({ ...prev, email: e.target.value }))}
                  />
                </div>

                <Input
                  label="Website"
                  value={formData.website}
                  onChange={(e) => setFormData(prev => ({ ...prev, website: e.target.value }))}
                />

                <Input
                  label="Theme of the Year"
                  value={formData.theme_of_year}
                  onChange={(e) => setFormData(prev => ({ ...prev, theme_of_year: e.target.value }))}
                />

                <Input
                  label="Verse of the Year"
                  value={formData.verse_of_year}
                  onChange={(e) => setFormData(prev => ({ ...prev, verse_of_year: e.target.value }))}
                />

                <TextArea
                  label="Welcome Message"
                  value={formData.welcome_message}
                  onChange={(e) => setFormData(prev => ({ ...prev, welcome_message: e.target.value }))}
                  rows={2}
                />
              </div>
            )}

            {activeTab === 'appearance' && (
              <div className="space-y-5">
                <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Appearance Settings</h2>

                <div className="flex items-center justify-between p-4 bg-gray-50 dark:bg-gray-800 rounded-lg">
                  <div>
                    <p className="font-medium text-gray-900 dark:text-white">Dark Mode</p>
                    <p className="text-sm text-gray-500">Toggle dark theme for admin panel</p>
                  </div>
                  <button
                    onClick={toggleDarkMode}
                    className={`relative inline-flex h-6 w-11 items-center rounded-full transition-colors ${
                      darkMode ? 'bg-blue-600' : 'bg-gray-300'
                    }`}
                  >
                    <span className={`inline-block h-4 w-4 transform rounded-full bg-white transition-transform ${
                      darkMode ? 'translate-x-6' : 'translate-x-1'
                    }`} />
                  </button>
                </div>

                <div className="grid md:grid-cols-3 gap-4">
                  <div>
                    <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Primary Color</label>
                    <div className="flex items-center space-x-2">
                      <input
                        type="color"
                        value={formData.primary_color}
                        onChange={(e) => setFormData(prev => ({ ...prev, primary_color: e.target.value }))}
                        className="w-10 h-10 rounded cursor-pointer"
                      />
                      <Input
                        value={formData.primary_color}
                        onChange={(e) => setFormData(prev => ({ ...prev, primary_color: e.target.value }))}
                        className="flex-1"
                      />
                    </div>
                  </div>

                  <div>
                    <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Secondary Color</label>
                    <div className="flex items-center space-x-2">
                      <input
                        type="color"
                        value={formData.secondary_color}
                        onChange={(e) => setFormData(prev => ({ ...prev, secondary_color: e.target.value }))}
                        className="w-10 h-10 rounded cursor-pointer"
                      />
                      <Input
                        value={formData.secondary_color}
                        onChange={(e) => setFormData(prev => ({ ...prev, secondary_color: e.target.value }))}
                        className="flex-1"
                      />
                    </div>
                  </div>

                  <div>
                    <label className="block text-sm font-medium text-gray-700 dark:text-gray-300 mb-2">Accent Color</label>
                    <div className="flex items-center space-x-2">
                      <input
                        type="color"
                        value={formData.accent_color}
                        onChange={(e) => setFormData(prev => ({ ...prev, accent_color: e.target.value }))}
                        className="w-10 h-10 rounded cursor-pointer"
                      />
                      <Input
                        value={formData.accent_color}
                        onChange={(e) => setFormData(prev => ({ ...prev, accent_color: e.target.value }))}
                        className="flex-1"
                      />
                    </div>
                  </div>
                </div>

                <div className="p-4 bg-gray-50 dark:bg-gray-800 rounded-lg">
                  <p className="font-medium text-gray-900 dark:text-white mb-4">Color Preview</p>
                  <div className="flex space-x-4">
                    <div className="h-12 w-20 rounded" style={{ backgroundColor: formData.primary_color }} />
                    <div className="h-12 w-20 rounded border" style={{ backgroundColor: formData.secondary_color }} />
                    <div className="h-12 w-20 rounded" style={{ backgroundColor: formData.accent_color }} />
                  </div>
                </div>
              </div>
            )}

            {activeTab === 'services' && (
              <div className="space-y-5">
                <div className="flex items-center justify-between mb-4">
                  <h2 className="text-lg font-semibold text-gray-900 dark:text-white">Service Times</h2>
                  <Button size="sm" onClick={addServiceTime}>Add Service</Button>
                </div>

                {formData.service_times.map((service, index) => (
                  <div key={index} className="p-4 bg-gray-50 dark:bg-gray-800 rounded-lg">
                    <div className="flex items-center justify-between mb-3">
                      <span className="text-sm font-medium text-gray-500">Service {index + 1}</span>
                      <button
                        onClick={() => removeServiceTime(index)}
                        className="text-red-500 text-sm hover:underline"
                      >
                        Remove
                      </button>
                    </div>
                    <div className="grid md:grid-cols-3 gap-3">
                      <Input
                        placeholder="Service name"
                        value={service.name}
                        onChange={(e) => updateServiceTime(index, 'name', e.target.value)}
                      />
                      <Input
                        placeholder="Day"
                        value={service.day}
                        onChange={(e) => updateServiceTime(index, 'day', e.target.value)}
                      />
                      <Input
                        placeholder="Time"
                        value={service.time}
                        onChange={(e) => updateServiceTime(index, 'time', e.target.value)}
                      />
                    </div>
                  </div>
                ))}

                {formData.service_times.length === 0 && (
                  <div className="text-center py-8 text-gray-500">
                    No service times configured. Click "Add Service" to add one.
                  </div>
                )}
              </div>
            )}

            {activeTab === 'social' && (
              <div className="space-y-5">
                <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Social Media Links</h2>

                <Input
                  label="Facebook"
                  placeholder="https://facebook.com/yourpage"
                  value={formData.social_facebook}
                  onChange={(e) => setFormData(prev => ({ ...prev, social_facebook: e.target.value }))}
                />

                <Input
                  label="YouTube"
                  placeholder="https://youtube.com/yourchannel"
                  value={formData.social_youtube}
                  onChange={(e) => setFormData(prev => ({ ...prev, social_youtube: e.target.value }))}
                />

                <Input
                  label="Instagram"
                  placeholder="https://instagram.com/yourhandle"
                  value={formData.social_instagram}
                  onChange={(e) => setFormData(prev => ({ ...prev, social_instagram: e.target.value }))}
                />

                <Input
                  label="WhatsApp"
                  placeholder="https://wa.me/yourphonenumber"
                  value={formData.social_whatsapp}
                  onChange={(e) => setFormData(prev => ({ ...prev, social_whatsapp: e.target.value }))}
                />
              </div>
            )}

            {activeTab === 'security' && (
              <div className="space-y-5">
                <h2 className="text-lg font-semibold text-gray-900 dark:text-white mb-4">Security Settings</h2>

                <div className="p-4 bg-gray-50 dark:bg-gray-800 rounded-lg">
                  <p className="font-medium text-gray-900 dark:text-white">Admin Access</p>
                  <p className="text-sm text-gray-500 mt-1">Logged in as: {user?.email}</p>
                  <p className="text-sm text-gray-500">Role: {user?.role}</p>
                </div>

                <div className="p-4 bg-yellow-50 dark:bg-yellow-900/20 rounded-lg">
                  <p className="font-medium text-yellow-800 dark:text-yellow-200">Two-Factor Authentication</p>
                  <p className="text-sm text-yellow-700 dark:text-yellow-300 mt-1">
                    Add an extra layer of security to your admin account.
                  </p>
                  <Button variant="outline" size="sm" className="mt-3">
                    Enable 2FA
                  </Button>
                </div>

                <div className="p-4 bg-blue-50 dark:bg-blue-900/20 rounded-lg">
                  <p className="font-medium text-blue-800 dark:text-blue-200">Change Password</p>
                  <p className="text-sm text-blue-700 dark:text-blue-300 mt-1 mb-4">
                    Update your admin password regularly for better security.
                  </p>
                  <Button variant="outline" size="sm">
                    Change Password
                  </Button>
                </div>
              </div>
            )}
          </Card>
        </div>
      </div>
    </div>
  );
}

```

## `./src/pages/admin/TrashPage.tsx`

```tsx
import React, { useState, useEffect } from 'react';
import { supabase, DeletedRecord } from '../../lib/supabase';
import { Card, Button, LoadingSpinner, Badge } from '../../components/ui';
import { Trash2, RotateCcw, Database } from 'lucide-react';

export function TrashPage() {
  const [records, setRecords] = useState<DeletedRecord[]>([]);
  const [loading, setLoading] = useState(true);
  const [restoring, setRestoring] = useState<string | null>(null);

  useEffect(() => { fetchData(); }, []);

  const fetchData = async () => {
    const { data } = await supabase.from('deleted_records').select('*').order('deleted_at', { ascending: false });
    setRecords(data || []);
    setLoading(false);
  };

  const handleRestore = async (record: DeletedRecord) => {
    setRestoring(record.id);
    try {
      const { id: _, ...data } = record.record_data;
      await supabase.from(record.table_name).insert(data);
      await supabase.from('deleted_records').delete().eq('id', record.id);
      fetchData();
    } catch (error) {
      alert('Failed to restore record');
    }
    setRestoring(null);
  };

  const handlePermanentDelete = async (id: string) => {
    if (!confirm('Permanently delete this record? This cannot be undone.')) return;
    await supabase.from('deleted_records').delete().eq('id', id);
    fetchData();
  };

  const handleEmptyTrash = async () => {
    if (!confirm('Permanently delete ALL records in trash? This cannot be undone.')) return;
    await supabase.from('deleted_records').delete().neq('id', '00000000-0000-0000-0000-000000000000');
    fetchData();
  };

  const getTableName = (table: string) => {
    const names: Record<string, string> = { members: 'Member', events: 'Event', sermons: 'Sermon', gallery: 'Gallery', announcements: 'Announcement', adverts: 'Advert', pastor_history: 'Pastor' };
    return names[table] || table;
  };

  if (loading) return <div className="flex justify-center py-20"><LoadingSpinner size="lg" /></div>;

  return (
    <div className="space-y-6">
      <div className="flex items-center justify-between">
        <div>
          <h1 className="text-2xl font-bold text-gray-900 dark:text-white">Trash</h1>
          <p className="text-gray-500 text-sm mt-1">{records.length} deleted record{records.length !== 1 ? 's' : ''}</p>
        </div>
        {records.length > 0 && <Button variant="danger" onClick={handleEmptyTrash}><Trash2 className="h-4 w-4 mr-2" /> Empty Trash</Button>}
      </div>

      <Card>
        {records.length > 0 ? (
          <div className="divide-y divide-gray-200 dark:divide-gray-700">
            {records.map((record) => (
              <div key={record.id} className="p-4 flex items-center justify-between hover:bg-gray-50 dark:hover:bg-gray-800">
                <div className="flex items-center space-x-4">
                  <div className="w-10 h-10 rounded-full bg-gray-100 dark:bg-gray-700 flex items-center justify-center">
                    <Database className="h-5 w-5 text-gray-400" />
                  </div>
                  <div>
                    <Badge variant="default" className="mb-1">{getTableName(record.table_name)}</Badge>
                    <p className="text-sm text-gray-600 dark:text-gray-400">{(record.record_data as Record<string, unknown>).full_name || (record.record_data as Record<string, unknown>).title || 'Untitled'}</p>
                    <p className="text-xs text-gray-500">Deleted: {new Date(record.deleted_at).toLocaleString()}</p>
                  </div>
                </div>
                <div className="flex space-x-2">
                  <Button variant="outline" size="sm" onClick={() => handleRestore(record)} loading={restoring === record.id}><RotateCcw className="h-4 w-4 mr-1" /> Restore</Button>
                  <Button variant="ghost" size="sm" onClick={() => handlePermanentDelete(record.id)} className="text-red-600 hover:bg-red-50"><Trash2 className="h-4 w-4" /></Button>
                </div>
              </div>
            ))}
          </div>
        ) : (
          <div className="p-12 text-center">
            <Trash2 className="h-12 w-12 mx-auto text-gray-300 dark:text-gray-600 mb-4" />
            <p className="text-gray-500 dark:text-gray-400">Trash is empty</p>
          </div>
        )}
      </Card>
    </div>
  );
}

```

## `./src/vite-env.d.ts`

```ts
/// <reference types="vite/client" />

```

## `./supabase/migrations/20260614001644_001_initial_schema.sql`

```sql
-- Create sequence first
CREATE SEQUENCE member_seq START 1;

-- Church Settings
CREATE TABLE church_settings (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  church_name TEXT DEFAULT 'Defence Word Ministry',
  motto TEXT DEFAULT 'God of Possibilities',
  slogan TEXT DEFAULT 'God of Possibilities',
  primary_color TEXT DEFAULT '#1e40af',
  secondary_color TEXT DEFAULT '#ffffff',
  accent_color TEXT DEFAULT '#16a34a',
  logo_url TEXT,
  address TEXT DEFAULT '',
  phone TEXT DEFAULT '',
  email TEXT DEFAULT '',
  website TEXT DEFAULT '',
  service_times JSONB DEFAULT '[]'::jsonb,
  theme_of_year TEXT DEFAULT '',
  verse_of_year TEXT DEFAULT '',
  welcome_message TEXT DEFAULT 'Welcome to Defence Word Ministries International Ghana',
  social_links JSONB DEFAULT '{}'::jsonb,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

INSERT INTO church_settings (id) VALUES (gen_random_uuid());

-- Departments
CREATE TABLE departments (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL UNIQUE,
  description TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

INSERT INTO departments (name, description) VALUES
('Ushering', 'Ushers and protocol team'),
('Choir', 'Music ministry'),
('Media', 'Media and technical team'),
('Children', 'Children''s ministry'),
('Youth', 'Youth ministry'),
('Women', 'Women''s ministry'),
('Men', 'Men''s ministry'),
('Prayer', 'Prayer team'),
('Evangelism', 'Evangelism team');

-- Admin Users
CREATE TABLE admin_users (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  email TEXT NOT NULL UNIQUE,
  password_hash TEXT NOT NULL,
  full_name TEXT NOT NULL,
  role TEXT DEFAULT 'secretary' CHECK (role IN ('pastor', 'secretary', 'media', 'admin')),
  two_factor_secret TEXT,
  two_factor_enabled BOOLEAN DEFAULT FALSE,
  active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Members
CREATE TABLE members (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  member_id TEXT UNIQUE DEFAULT 'DWM-' || to_char(NOW(), 'YYYY') || '-' || LPAD(nextval('member_seq')::TEXT, 4, '0'),
  full_name TEXT NOT NULL,
  date_of_birth DATE NOT NULL,
  gender TEXT NOT NULL CHECK (gender IN ('Male', 'Female')),
  phone TEXT,
  email TEXT,
  address TEXT,
  department_id UUID REFERENCES departments(id),
  photo_url TEXT,
  fellowship TEXT,
  referral_source TEXT,
  referral_name TEXT,
  prayer_request TEXT,
  status TEXT DEFAULT 'pending' CHECK (status IN ('pending', 'approved', 'rejected')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Events
CREATE TABLE events (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  description TEXT,
  event_date DATE NOT NULL,
  start_time TEXT,
  end_time TEXT,
  location TEXT,
  image_url TEXT,
  video_url TEXT,
  status TEXT DEFAULT 'upcoming' CHECK (status IN ('upcoming', 'completed', 'cancelled')),
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Sermons
CREATE TABLE sermons (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  speaker TEXT NOT NULL,
  scripture_reference TEXT,
  sermon_date DATE NOT NULL,
  description TEXT,
  audio_url TEXT,
  video_url TEXT,
  thumbnail_url TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Gallery
CREATE TABLE gallery (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT,
  type TEXT NOT NULL CHECK (type IN ('image', 'video')),
  url TEXT NOT NULL,
  thumbnail_url TEXT,
  event_id UUID REFERENCES events(id),
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Announcements
CREATE TABLE announcements (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  content TEXT NOT NULL,
  image_url TEXT,
  video_url TEXT,
  link_url TEXT,
  priority INTEGER DEFAULT 0,
  active BOOLEAN DEFAULT TRUE,
  start_date DATE,
  end_date DATE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Adverts/Promotional Content
CREATE TABLE adverts (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  title TEXT NOT NULL,
  description TEXT,
  company_name TEXT,
  contact_info TEXT,
  website_url TEXT,
  social_url TEXT,
  image_url TEXT,
  video_url TEXT,
  image_size TEXT DEFAULT 'medium' CHECK (image_size IN ('small', 'medium', 'large')),
  active BOOLEAN DEFAULT TRUE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Attendance
CREATE TABLE attendance (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  member_id UUID REFERENCES members(id) ON DELETE CASCADE,
  service_date DATE NOT NULL,
  service_type TEXT DEFAULT 'Sunday Service',
  status TEXT NOT NULL CHECK (status IN ('present', 'absent', 'late')),
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE(member_id, service_date, service_type)
);

-- Donations
CREATE TABLE donations (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  member_id UUID REFERENCES members(id),
  amount DECIMAL(10,2) NOT NULL,
  currency TEXT DEFAULT 'GHS',
  donation_type TEXT,
  payment_method TEXT,
  reference_number TEXT,
  donation_date DATE DEFAULT CURRENT_DATE,
  notes TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Pastor/Leadership History
CREATE TABLE pastor_history (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  full_name TEXT NOT NULL,
  title TEXT,
  role TEXT NOT NULL,
  bio TEXT,
  quote TEXT,
  photo_url TEXT,
  start_date DATE,
  end_date DATE,
  current BOOLEAN DEFAULT FALSE,
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);

-- Deleted Records (for recovery)
CREATE TABLE deleted_records (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  table_name TEXT NOT NULL,
  record_id TEXT NOT NULL,
  record_data JSONB NOT NULL,
  deleted_at TIMESTAMPTZ DEFAULT NOW()
);

-- SMS/Notification Logs
CREATE TABLE notification_logs (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  recipient_phone TEXT NOT NULL,
  message TEXT NOT NULL,
  notification_type TEXT NOT NULL,
  status TEXT DEFAULT 'pending',
  sent_at TIMESTAMPTZ,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Contact Messages
CREATE TABLE contact_messages (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  name TEXT NOT NULL,
  email TEXT NOT NULL,
  phone TEXT,
  subject TEXT,
  message TEXT NOT NULL,
  status TEXT DEFAULT 'unread',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Birthday/Anniversary Reminders
CREATE TABLE special_dates (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  member_id UUID REFERENCES members(id) ON DELETE CASCADE,
  date_type TEXT NOT NULL CHECK (date_type IN ('birthday', 'anniversary', 'other')),
  date_value DATE NOT NULL,
  reminder_days INTEGER DEFAULT 7,
  last_notified DATE,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Create indexes for performance
CREATE INDEX idx_members_status ON members(status);
CREATE INDEX idx_members_department ON members(department_id);
CREATE INDEX idx_members_fellowship ON members(fellowship);
CREATE INDEX idx_attendance_date ON attendance(service_date);
CREATE INDEX idx_events_date ON events(event_date);
CREATE INDEX idx_sermons_date ON sermons(sermon_date);
CREATE INDEX idx_donations_date ON donations(donation_date);
CREATE INDEX idx_special_dates ON special_dates(date_value);

-- Enable RLS on all tables
ALTER TABLE church_settings ENABLE ROW LEVEL SECURITY;
ALTER TABLE departments ENABLE ROW LEVEL SECURITY;
ALTER TABLE admin_users ENABLE ROW LEVEL SECURITY;
ALTER TABLE members ENABLE ROW LEVEL SECURITY;
ALTER TABLE events ENABLE ROW LEVEL SECURITY;
ALTER TABLE sermons ENABLE ROW LEVEL SECURITY;
ALTER TABLE gallery ENABLE ROW LEVEL SECURITY;
ALTER TABLE announcements ENABLE ROW LEVEL SECURITY;
ALTER TABLE adverts ENABLE ROW LEVEL SECURITY;
ALTER TABLE attendance ENABLE ROW LEVEL SECURITY;
ALTER TABLE donations ENABLE ROW LEVEL SECURITY;
ALTER TABLE pastor_history ENABLE ROW LEVEL SECURITY;
ALTER TABLE deleted_records ENABLE ROW LEVEL SECURITY;
ALTER TABLE notification_logs ENABLE ROW LEVEL SECURITY;
ALTER TABLE contact_messages ENABLE ROW LEVEL SECURITY;
ALTER TABLE special_dates ENABLE ROW LEVEL SECURITY;

-- RLS Policies: Public read for most tables
CREATE POLICY "public_read_church_settings" ON church_settings FOR SELECT TO public USING (true);
CREATE POLICY "public_read_departments" ON departments FOR SELECT TO public USING (true);
CREATE POLICY "public_read_events" ON events FOR SELECT TO public USING (true);
CREATE POLICY "public_read_sermons" ON sermons FOR SELECT TO public USING (true);
CREATE POLICY "public_read_gallery" ON gallery FOR SELECT TO public USING (true);
CREATE POLICY "public_read_announcements" ON announcements FOR SELECT TO public USING (active = true);
CREATE POLICY "public_read_adverts" ON adverts FOR SELECT TO public USING (active = true);
CREATE POLICY "public_read_pastor" ON pastor_history FOR SELECT TO public USING (true);

-- RLS Policies: Authenticated users can manage most tables
CREATE POLICY "auth_insert_members" ON members FOR INSERT TO authenticated WITH CHECK (true);
CREATE POLICY "auth_select_members" ON members FOR SELECT TO authenticated USING (true);
CREATE POLICY "auth_update_members" ON members FOR UPDATE TO authenticated USING (true) WITH CHECK (true);
CREATE POLICY "auth_delete_members" ON members FOR DELETE TO authenticated USING (true);

-- Contact messages - anyone can insert
CREATE POLICY "public_insert_contact" ON contact_messages FOR INSERT TO public WITH CHECK (true);

-- Announcements CRUD for authenticated
CREATE POLICY "auth_manage_announcements" ON announcements FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Events CRUD for authenticated
CREATE POLICY "auth_manage_events" ON events FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Sermons CRUD for authenticated
CREATE POLICY "auth_manage_sermons" ON sermons FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Gallery CRUD for authenticated
CREATE POLICY "auth_manage_gallery" ON gallery FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Adverts CRUD for authenticated
CREATE POLICY "auth_manage_adverts" ON adverts FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Attendance CRUD for authenticated
CREATE POLICY "auth_manage_attendance" ON attendance FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Donations CRUD for authenticated
CREATE POLICY "auth_manage_donations" ON donations FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Pastor history CRUD for authenticated
CREATE POLICY "auth_manage_pastor" ON pastor_history FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Departments CRUD for authenticated
CREATE POLICY "auth_manage_departments" ON departments FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Church settings CRUD for authenticated
CREATE POLICY "auth_manage_settings" ON church_settings FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Admin users CRUD for authenticated
CREATE POLICY "auth_manage_admin" ON admin_users FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Deleted records for authenticated
CREATE POLICY "auth_manage_deleted" ON deleted_records FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Notification logs for authenticated
CREATE POLICY "auth_manage_notifications" ON notification_logs FOR ALL TO authenticated USING (true) WITH CHECK (true);

-- Special dates for authenticated
CREATE POLICY "auth_manage_special_dates" ON special_dates FOR ALL TO authenticated USING (true) WITH CHECK (true);

```

## `./supabase/migrations/20260615004351_002_add_end_date_columns.sql`

```sql
-- Add end_date columns for auto-removal feature

ALTER TABLE events ADD COLUMN IF NOT EXISTS end_date DATE;
ALTER TABLE sermons ADD COLUMN IF NOT EXISTS end_date DATE;
ALTER TABLE gallery ADD COLUMN IF NOT EXISTS end_date DATE;
ALTER TABLE adverts ADD COLUMN IF NOT EXISTS end_date DATE;
ALTER TABLE donations ADD COLUMN IF NOT EXISTS end_date DATE;
```

## `./supabase/migrations/20260615011101_003_add_occupation.sql`

```sql
-- Add occupation column to members table
ALTER TABLE members ADD COLUMN IF NOT EXISTS occupation TEXT;
```

## `./supabase/migrations/20260615011236_004_add_content_fields.sql`

```sql
-- Add content columns to church_settings
ALTER TABLE church_settings ADD COLUMN IF NOT EXISTS about_text TEXT;
ALTER TABLE church_settings ADD COLUMN IF NOT EXISTS donation_text TEXT DEFAULT 'Support our ministry with your generous donations.';
ALTER TABLE church_settings ADD COLUMN IF NOT EXISTS live_stream_url TEXT;
```

## `./tailwind.config.js`

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ['./index.html', './src/**/*.{js,ts,jsx,tsx}'],
  theme: {
    extend: {},
  },
  plugins: [],
};

```

## `./tsconfig.app.json`

```json
{
  "compilerOptions": {
    "target": "ES2020",
    "useDefineForClassFields": true,
    "lib": ["ES2020", "DOM", "DOM.Iterable"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,
    "jsx": "react-jsx",

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["src"]
}

```

## `./tsconfig.json`

```json
{
  "files": [],
  "references": [
    { "path": "./tsconfig.app.json" },
    { "path": "./tsconfig.node.json" }
  ]
}

```

## `./tsconfig.node.json`

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2023"],
    "module": "ESNext",
    "skipLibCheck": true,

    /* Bundler mode */
    "moduleResolution": "bundler",
    "allowImportingTsExtensions": true,
    "isolatedModules": true,
    "moduleDetection": "force",
    "noEmit": true,

    /* Linting */
    "strict": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  },
  "include": ["vite.config.ts"]
}

```

## `./vite.config.ts`

```ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  optimizeDeps: {
    exclude: ['lucide-react'],
  },
});

```

