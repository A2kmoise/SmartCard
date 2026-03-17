# SmartPay - RFID Wallet Management System

A modern React Native mobile application for managing RFID card payments with real-time backend integration.

## 🎨 Features

### Authentication
- ✅ Secure login with JWT tokens
- ✅ Real-time backend connection checking
- ✅ Auto-login with stored credentials
- ✅ Role-based access control (Admin/Cashier)

### Admin Dashboard
- ✅ Card scanning (real RFID + simulation)
- ✅ Real-time balance display
- ✅ Top-up cards with quick amount buttons
- ✅ Transaction history
- ✅ User account creation

### Cashier Dashboard (E-Commerce Style)
- ✅ Split-screen layout: Products | Cart
- ✅ Product catalog with add-to-cart
- ✅ Shopping cart with quantity controls
- ✅ Real-time total calculation
- ✅ Balance checking before checkout
- ✅ One-click checkout

### Profile & Settings
- ✅ User information display
- ✅ Server connection status
- ✅ Role-based permissions
- ✅ Quick actions menu
- ✅ Secure logout

## 🔌 Hardware Connections

### ESP8266 & MFRC522 RFID Module

#### Pin Mapping
| MFRC522 Pin | ESP8266 Pin | NodeMCU Pin | Description |
|-------------|-------------|-------------|-------------|
| SDA (SS) | GPIO2 | D4 | Slave Select |
| SCK | GPIO14 | D5 | Serial Clock |
| MOSI | GPIO13 | D7 | Master Out Slave In |
| MISO | GPIO12 | D6 | Master In Slave Out |
| IRQ | - | - | Not connected |
| GND | GND | GND | Ground |
| RST | GPIO0 | D3 | Reset |
| 3.3V | 3.3V | 3.3V | Power Supply |

#### Wiring Diagram
```
ESP8266 (NodeMCU)          MFRC522 RFID Reader
┌─────────────┐            ┌──────────────┐
│             │            │              │
│    3.3V ────┼────────────┼──── 3.3V     │
│     GND ────┼────────────┼──── GND      │
│      D3 ────┼────────────┼──── RST      │
│      D4 ────┼────────────┼──── SDA (SS) │
│      D5 ────┼────────────┼──── SCK      │
│      D6 ────┼────────────┼──── MISO     │
│      D7 ────┼────────────┼──── MOSI     │
│             │            │              │
└─────────────┘            └──────────────┘
```

#### Important Notes
- **Power**: MFRC522 operates at 3.3V only. Do NOT connect to 5V!
- **SPI Interface**: Uses hardware SPI for reliable communication
- **Reset Pin**: D3 (GPIO0) used for module reset
- **Chip Select**: D4 (GPIO2) for SPI slave select

#### Firmware Configuration
The pin definitions are set in the firmware:
```cpp
#define RST_PIN D3  // GPIO0
#define SS_PIN D4   // GPIO2
```

---

## 🚀 Quick Start

### 1. Install Dependencies
```bash
npm install
```

### 2. Configure Backend
Edit `config/app-config.ts`:
```typescript
export const APP_CONFIG = {
    SERVER_URL: 'http:/url/:9201',
    // ...
};
```

### 3. Start the App
```bash
npx expo start
```

### 4. Login
- **Admin**: `admin` / `admin123`
- **Cashier**: `cashier` / `cashier123`

## 📱 Screenshots

### Login Screen
- Modern gradient design
- Server status indicator
- Auto-connection checking
- Test credentials display

### Admin Dashboard
- Card balance display
- Quick top-up amounts ($10, $20, $50, $100)
- Real-time Socket.IO updates
- Clean, minimal interface

### Cashier Dashboard
- **Left Panel**: Product catalog
  - Tap to add products to cart
  - Price display for each item
  - Scrollable product list
  
- **Right Panel**: Shopping cart
  - Quantity controls (+/-)
  - Remove items
  - Real-time total calculation
  - Checkout button

### Profile Screen
- User avatar with role badge
- Connection information
- Quick actions
- Permission list
- Logout button

## 🏗️ Architecture

### File Structure
```
app/
├── (tabs)/
│   ├── index.tsx          # Admin Dashboard
│   ├── explore.tsx        # Cashier Dashboard
│   └── _layout.tsx        # Tab navigation
├── login.tsx              # Login screen
├── profile.tsx            # Profile screen
├── signup.tsx             # User creation
└── _layout.tsx            # Root layout

components/
├── card-visual.tsx        # RFID card display
├── glass-card.tsx         # Glassmorphism card
└── ui/                    # UI components

config/
└── app-config.ts          # Backend configuration

context/
└── AppContext.tsx         # Global state management

services/
├── auth.ts                # Authentication service
└── socket.ts              # Socket.IO service
```

### State Management
- **Context API** for global state
- **Socket.IO** for real-time updates
- **AsyncStorage** for persistence

### Backend Integration
- **REST API** for authentication and transactions
- **Socket.IO** for real-time card scanning
- **JWT** for secure authentication

## 🔌 API Endpoints

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/api/login` | POST | User authentication |
| `/api/register` | POST | Create new user |
| `/topup` | POST | Add balance to card |
| `/pay` | POST | Process payment |
| Socket.IO | WS | Real-time events |

## 🎯 User Flows

### Admin: Top-Up Flow
1. Login as admin
2. Scan card (or simulate)
3. View current balance
4. Select quick amount or enter custom
5. Confirm top-up
6. Balance updates in real-time

### Cashier: Payment Flow
1. Login as cashier
2. Scan customer card
3. View card balance
4. Browse products and add to cart
5. Adjust quantities as needed
6. Review total
7. Checkout (balance auto-deducted)
8. Success confirmation

### Admin: Create User Flow
1. Go to Profile
2. Click "Create Account"
3. Enter username and password
4. Select role (Admin/Cashier)
5. Submit
6. New user can login immediately

## 🎨 Design System

### Colors
- **Primary**: `#6366f1` (Indigo)
- **Success**: `#10b981` (Green)
- **Error**: `#ef4444` (Red)
- **Background**: `#0f172a` → `#1e3a5f` (Gradient)

### Typography
- **Headings**: 800 weight, tight letter-spacing
- **Body**: 500-600 weight
- **Monospace**: For UIDs and codes

### Components
- **Glass Cards**: Semi-transparent with backdrop blur
- **Gradient Buttons**: Smooth color transitions
- **Status Badges**: Color-coded indicators
- **Input Fields**: Large touch targets, clear labels

## 🔒 Security

- JWT token authentication
- Secure password storage
- Role-based access control
- Backend connection validation
- Input sanitization
- Error handling

## 🐛 Troubleshooting

### "Server Offline"
- Check VPS is running
- Verify SERVER_URL in config
- Test with: `curl http://YOUR_VPS_IP:9201/`

### "Cannot find module"
- Run: `npx expo start --clear`
- Restart TypeScript server in VS Code

### White/Black Screen
- Close app completely
- Run: `npx expo start --clear`
- Reload app

### Socket Disconnected
- App will auto-reconnect (5 attempts)
- Check backend logs
- Verify Socket.IO is running

## 📦 Dependencies

```json
{
  "expo": "~54.0.33",
  "react-native": "latest",
  "socket.io-client": "^4.8.3",
  "@react-native-async-storage/async-storage": "2.2.0",
  "expo-linear-gradient": "latest",
  "expo-router": "latest"
}
```

## 🚀 Deployment

### Build for Android
```bash
eas build --platform android
```

### Build for iOS
```bash
eas build --platform ios
```

### Update OTA
```bash
eas update
```

## 👥 Team

**iot_shield_2026**

## 📄 License

MIT License - feel free to use for your projects!

---

## 🎉 What's New in This Version

### UI/UX Improvements
- ✨ Complete redesign with modern aesthetics
- ✨ Improved color scheme and typography
- ✨ Better spacing and visual hierarchy
- ✨ Smooth animations and transitions

### Cashier Dashboard Redesign
- 🛍️ E-commerce style product selection
- 🛒 Shopping cart with quantity controls
- 💰 Real-time total calculation
- ✅ Balance validation before checkout
- 📱 Split-screen layout for better UX

### Authentication Enhancements
- 🔐 Backend connection checking
- 🟢 Real-time server status indicator
- ⚡ Faster login flow
- 🔄 Auto-retry on connection failure

### General Improvements
- 📊 Better error messages
- 🎨 Consistent design language
- 🚀 Performance optimizations
- 📱 Improved mobile responsiveness

---


