# Test Cases for Delivery Method Validation

## Test File: test/api/basketApiSpec.ts

### New Test Cases (7 tests)

#### 1. POST placing an order without deliveryMethodId fails
**Purpose:** Validate that checkout fails when deliveryMethodId is missing  
**Request Body:**
```json
{
  "orderDetails": {
    "paymentId": "card",
    "addressId": 1
  }
}
```
**Expected Result:** 500 error with message "Delivery method ID is required for checkout."

---

#### 2. POST placing an order with null deliveryMethodId fails
**Purpose:** Validate that checkout fails when deliveryMethodId is explicitly null  
**Request Body:**
```json
{
  "orderDetails": {
    "deliveryMethodId": null,
    "paymentId": "card",
    "addressId": 1
  }
}
```
**Expected Result:** 500 error with message "Delivery method ID is required for checkout."

---

#### 3. POST placing an order with undefined deliveryMethodId fails
**Purpose:** Validate that checkout fails when deliveryMethodId is explicitly undefined  
**Request Body:**
```json
{
  "orderDetails": {
    "deliveryMethodId": undefined,
    "paymentId": "card",
    "addressId": 1
  }
}
```
**Expected Result:** 500 error with message "Delivery method ID is required for checkout."

---

#### 4. POST placing an order with invalid deliveryMethodId fails
**Purpose:** Validate that checkout fails when deliveryMethodId doesn't exist (9999)  
**Request Body:**
```json
{
  "orderDetails": {
    "deliveryMethodId": 9999,
    "paymentId": "card",
    "addressId": 1
  }
}
```
**Expected Result:** 500 error with message "Invalid delivery method ID."

---

#### 5. POST placing an order with non-existent deliveryMethodId fails
**Purpose:** Validate that checkout fails when deliveryMethodId doesn't exist (999)  
**Request Body:**
```json
{
  "orderDetails": {
    "deliveryMethodId": 999,
    "paymentId": "card",
    "addressId": 1
  }
}
```
**Expected Result:** 500 error with message "Invalid delivery method ID."

---

#### 6. POST placing an order with valid deliveryMethodId succeeds
**Purpose:** Validate that checkout succeeds with a valid deliveryMethodId  
**Request Body:**
```json
{
  "orderDetails": {
    "deliveryMethodId": 2,
    "paymentId": "card",
    "addressId": 1
  }
}
```
**Expected Result:** 200 success with order confirmation

---

#### 7. POST placing an order without orderDetails object fails
**Purpose:** Validate that checkout fails when orderDetails is missing entirely  
**Request Body:**
```json
{}
```
**Expected Result:** 500 error with message "Delivery method ID is required for checkout."

---

### Updated Existing Test Cases (4 tests)

All existing checkout tests were updated to include valid `deliveryMethodId` in their request bodies:

1. **POST placing an order for an existing basket returns orderId**
2. **POST placing an order for a non-existing basket fails**
3. **POST placing an order for a basket with a negative total cost is possible**
4. **POST placing an order for a basket with 99% discount is possible**

---

## Test File: test/api/dataExportApiSpec.ts

### Updated Existing Test Cases (2 tests)

Both tests that use the checkout endpoint were updated to include valid `deliveryMethodId`:

1. **Export data including orders without use of CAPTCHA**
2. **Export data including orders with use of CAPTCHA**

---

## Test Execution

### Run specific test file:
```bash
npm test test/api/basketApiSpec.ts
npm test test/api/dataExportApiSpec.ts
```

### Run all tests:
```bash
npm test
```

---

## Coverage Matrix

| Scenario | Test Case | Expected Status | Error Message |
|----------|-----------|----------------|---------------|
| Missing deliveryMethodId | Test #1 | 500 | "Delivery method ID is required for checkout." |
| Null deliveryMethodId | Test #2 | 500 | "Delivery method ID is required for checkout." |
| Undefined deliveryMethodId | Test #3 | 500 | "Delivery method ID is required for checkout." |
| Invalid deliveryMethodId (9999) | Test #4 | 500 | "Invalid delivery method ID." |
| Invalid deliveryMethodId (999) | Test #5 | 500 | "Invalid delivery method ID." |
| Valid deliveryMethodId (2) | Test #6 | 200 | Success with order confirmation |
| Missing orderDetails | Test #7 | 500 | "Delivery method ID is required for checkout." |

---

## Security Validation

These tests ensure that:
- ✅ The free shipping exploit is prevented
- ✅ All delivery method IDs are validated against the database
- ✅ Missing or invalid IDs are rejected with appropriate error messages
- ✅ Valid IDs allow checkout to proceed normally
- ✅ Existing functionality remains intact
