# Payment System Setup Guide

## Overview
The payment system has been updated to use only online payments through Razorpay. Cash on Delivery (COD) has been completely removed.

## Setup Instructions

### 1. Razorpay Configuration
1. Create a Razorpay account at [https://razorpay.com](https://razorpay.com)
2. Get your API keys from the Razorpay dashboard
3. Create a `.env` file in the `backend` directory with the following:

```env
# Server Configuration
PORT=5000
NODE_ENV=development

# Database Configuration
MONGO_URI=mongodb://localhost:27017/ahalya-texile

# JWT Configuration
JWT_SECRET=your_jwt_secret_key_here

# Razorpay Configuration (REQUIRED)
RAZORPAY_KEY_ID=your_razorpay_key_id_here
RAZORPAY_KEY_SECRET=your_razorpay_key_secret_here

# Frontend URL (for CORS)
FRONTEND_URL=http://localhost:5173
```

### 2. Install Dependencies
Make sure all dependencies are installed:
```bash
# Backend
cd backend
npm install

# Frontend
cd frontend
npm install
```

### 3. Start the Servers
```bash
# Start backend (in backend directory)
npm run dev

# Start frontend (in frontend directory)
npm run dev
```

## Testing the Payment System

### 1. Test Configuration
Visit `http://localhost:5000/api/payment/test` to check if Razorpay is properly configured.

Expected response:
```json
{
  "razorpayConfigured": true,
  "keyIdExists": true,
  "keySecretExists": true,
  "message": "Payment gateway is configured"
}
```

### 2. Test Payment Flow
1. Add products to cart
2. Proceed to checkout
3. Fill in shipping details
4. Select payment method (UPI, Card, or Net Banking)
5. Click "Pay Now"
6. Complete the payment in the Razorpay modal
7. Verify order is created with "Payment Successful" status

## Payment Flow Changes

### Before
- Supported Cash on Delivery (COD)
- Mixed payment logic
- Basic error handling

### After
- **Only online payments** (UPI, Credit/Debit Card, Net Banking)
- **Streamlined payment flow** - all orders go through Razorpay
- **Enhanced error handling** with detailed messages
- **Payment status shows "Payment Successful"** instead of "Paid"
- **Better validation** and security checks

## Error Handling

### Common Issues and Solutions

1. **"Payment gateway not configured"**
   - Solution: Add Razorpay keys to `.env` file

2. **"Razorpay SDK failed to load"**
   - Solution: Check internet connection
   - Make sure frontend is running on correct port

3. **"Payment verification failed"**
   - Solution: Check server logs for detailed error
   - Verify Razorpay keys are correct

4. **"Invalid payment signature"**
   - Solution: Ensure webhook is properly configured in Razorpay dashboard
   - Check server time is synchronized

## Security Features

1. **Signature Verification**: All payments are verified using HMAC-SHA256
2. **Duplicate Payment Prevention**: Orders cannot be paid twice
3. **Configuration Validation**: Server checks for missing credentials
4. **Detailed Error Logging**: All payment errors are logged for debugging

## Support

For payment-related issues:
1. Check browser console for JavaScript errors
2. Check server logs for backend errors
3. Verify Razorpay account is active and configured
4. Test payment configuration endpoint: `/api/payment/test`

## Notes

- All payments are now mandatory - no COD option
- Orders are automatically confirmed after successful payment
- Payment status is updated to "Payment Successful"
- Enhanced error messages for better debugging
- Test mode can be used with Razorpay test keys
