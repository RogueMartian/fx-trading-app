# FX Trading Platform

A scalable, secure, and modular NestJS-based backend system for currency exchange and trading.

## Postman Documentation

https://documenter.getpostman.com/view/19965916/2sB2cVgNZ5

## App Flow

![App Screenshot](https://github.com/ItisCyph3r/fx-trading-app/blob/main/src/assets/trading-flow-diagram.svg)

## Features

- **User Authentication**: Secure registration, login, and email verification
- **Wallet Management**: Create and manage currency wallets
- **Currency Conversion**: Convert between different currencies with real-time rates
- **Trading Functionality**: Buy and sell currencies with customizable spread
- **Transaction History**: Track all financial operations with detailed records
- **Exchange Rate Management**: Integration with external FX API with caching

## System Architecture

The application follows a modular architecture based on NestJS best practices:

- **Modules**: Separate domains into modules (Auth, User, Wallet, Transaction, FX)
- **Controllers**: Handle HTTP requests and route them to appropriate services
- **Services**: Implement business logic and domain-specific operations
- **Entities**: Define database models and relationships
- **DTOs**: Validate incoming data and define request/response contracts
- **Guards**: Implement authorization and role-based access control

### Key Components

- **Auth Module**: Handles user registration, login, and token management
- **User Module**: Manages user profiles and permissions
- **Wallet Module**: Manages currency wallets and balance operations
- **Transaction Module**: Records and retrieves financial operations
- **FX Module**: Manages currency exchange rates with caching

## Technical Stack

- **Framework**: NestJS
- **Database**: PostgreSQL
- **ORM**: TypeORM
- **Authentication**: JWT
- **Validation**: class-validator
- **API Documentation**: Swagger
- **Email Service**: Nodemailer
- **External API**: ExchangeRate API

## Setup Instructions

### Prerequisites

- Node.js (v14 or later)
- PostgreSQL
- npm or yarn

### Environment Setup
1. Copy `.env.example` to `.env`
2. Update the values in `.env` with your credentials
3. Never commit `.env` to version control

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/fx-trading-platform.git
   cd fx-trading-platform
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the PostgreSQL database

4. Run the application:
   ```bash
   npm run start:dev
   ```

5. Access the Swagger documentation at:
   ```
   http://localhost:5000/api-docs
   ```

## API Documentation

### Authentication

#### Register a new user
```
POST /auth/register
Body: { "email": "user@example.com", "password": "password123" }
```

#### Verify email with OTP
```
POST /auth/verify
Body: { "email": "user@example.com", "otp": "1234" }
```

#### Resend OTP
```
POST /auth/resend-otp
Body: { "email": "user@example.com" }
```

#### Login
```
POST /auth/login
Body: { "email": "user@example.com", "password": "password123" }
```

### Wallet Management

#### Get user wallets
```
GET /wallet
Headers: { "Authorization": "Bearer {token}" }
```

#### Fund wallet
```
POST /wallet/fund
Headers: { "Authorization": "Bearer {token}" }
Body: { "currency": "NGN", "amount": 10000 }
```

#### Convert currency
```
POST /wallet/convert
Headers: { "Authorization": "Bearer {token}" }
Body: { "fromCurrency": "NGN", "toCurrency": "USD", "amount": 1000 }
```

#### Trade currency
```
POST /wallet/trade
Headers: { "Authorization": "Bearer {token}" }
Body: { "baseCurrency": "NGN", "quoteCurrency": "USD", "amount": 1000, "side": "BUY" }
```

### FX Rates

#### Get specific rate
```
GET /fx/rate?base=USD&target=NGN
Headers: { "Authorization": "Bearer {token}" }
```

#### Get all rates
```
GET /fx/rates?base=USD
Headers: { "Authorization": "Bearer {token}" }
```

### Transactions

#### Get transactions
```
GET /transactions?type=FUNDING&currency=NGN&from=2023-01-01&to=2023-12-31&page=1&limit=10
Headers: { "Authorization": "Bearer {token}" }
```

## Architectural Decisions

### Database Schema

The system uses a relational database with the following key entities:
- **User**: Stores user accounts and authentication information
- **Wallet**: Manages currency balances for each user
- **Transaction**: Records all financial operations
- **FxRate**: Caches exchange rates to reduce API calls

### Security Measures

1. **Request Validation**: All incoming requests are validated using DTOs and class-validator
2. **Transaction Atomicity**: Database transactions ensure ACID properties for financial operations
3. **Role-Based Access**: Guards ensure users can only access their own data
4. **Password Hashing**: Passwords are securely hashed with bcrypt
5. **JWT Authentication**: Secure token-based authentication

### Scalability Considerations

1. **Rate Caching**: FX rates are cached to reduce external API calls
2. **Pagination**: List endpoints support pagination for performance
3. **Modular Architecture**: Easy to extend with new modules or features
4. **Database Indexes**: Proper indexes for frequently queried fields

## Key Assumptions

1. **Currency Support**: The system initially supports major currencies, but can be extended
2. **Transaction Types**: Four types of transactions are supported: FUNDING, WITHDRAWAL, TRADE, CONVERSION
3. **User Roles**: Two roles are supported: USER and ADMIN
4. **Initial Balance**: New users receive 1000 NGN upon registration
5. **Trading Spread**: A 2% spread is applied to trading operations

## Testing

Run the test suite with:

```bash
npm test
```

Key areas covered by tests:
- Wallet balance management
- Currency conversion logic
- Trading operations
- Transaction recording
- Authentication flow

## Future Enhancements

1. **Additional Payment Methods**: Integration with payment gateways
2. **Advanced Trading Features**: Limit orders, stop losses
3. **Push Notifications**: Real-time alerts for transactions
4. **Analytics Dashboard**: Visualize trading performance
5. **Two-Factor Authentication**: Enhance security for sensitive operations

## Testing

### Overview
The application uses Jest as the testing framework with comprehensive test coverage for core functionalities.

### Test Categories

1. **Unit Tests**
   - Controller tests
   - Service tests 
   - Entity validation tests
   - DTO validation tests

2. **Integration Tests**
   - Database operations
   - External API interactions
   - Transaction flows

### Key Test Coverage Areas

1. **Authentication**
   - User registration
   - Login flows
   - OTP verification
   - JWT token validation

2. **Wallet Operations**
   - Wallet creation
   - Balance updates
   - Currency conversion
   - Trading operations

3. **Transaction Management**
   - Transaction creation
   - Transaction history retrieval
   - Pagination and filtering

4. **FX Operations**
   - Rate fetching
   - Rate caching
   - Rate conversion calculations

### Test Mocks

The test suite includes comprehensive mocks for:
- Database entities
- External API responses
- User data
- Wallet data
- Transaction data
- FX rates

Example mock data:
```typescript
export const mockWallet = {
    id: '123e4567-e89b-12d3-a456-426614174001',
    user: mockUser,
    currency: 'NGN',
    balance: 1000
};

export const mockTradeWallets = {
    USD: {
        balance: 1000000,
        currency: 'USD'
    },
    NGN: {
        balance: 200000,
        currency: 'NGN'
    }
};