# Unit Tests for Delivery Method Validation

## Overview
Added comprehensive unit tests to validate the delivery method ID validation fix in the checkout endpoint (`/rest/basket/:id/checkout`).

## Test Coverage

### New Tests Added (7 tests)

1. **POST placing an order without deliveryMethodId fails**
   - Tests that checkout fails when `deliveryMethodId` is missing from `orderDetails`
   - Expected: 500 error with message "Delivery method ID is required for checkout."

2. **POST placing an order with null deliveryMethodId fails**
   - Tests that checkout fails when `deliveryMethodId` is explicitly set to `null`
   - Expected: 500 error with message "Delivery method ID is required for checkout."

3. **POST placing an order with undefined deliveryMethodId fails**
   - Tests that checkout fails when `deliveryMethodId` is explicitly set to `undefined`
   - Expected: 500 error with message "Delivery method ID is required for checkout."

4. **POST placing an order with invalid deliveryMethodId fails**
   - Tests that checkout fails when `deliveryMethodId` is set to a non-existent ID (9999)
   - Expected: 500 error with message "Invalid delivery method ID."

5. **POST placing an order with non-existent deliveryMethodId fails**
   - Tests that checkout fails when `deliveryMethodId` is set to another non-existent ID (999)
   - Expected: 500 error with message "Invalid delivery method ID."

6. **POST placing an order with valid deliveryMethodId succeeds**
   - Tests that checkout succeeds when a valid `deliveryMethodId` (2) is provided
   - Expected: 200 success with order confirmation

7. **POST placing an order without orderDetails object fails**
   - Tests that checkout fails when the entire `orderDetails` object is missing
   - Expected: 500 error with message "Delivery method ID is required for checkout."

### Updated Existing Tests (4 tests)

Updated the following existing tests to include valid `deliveryMethodId` in their request bodies to ensure they continue to work with the new validation:

**In test/api/basketApiSpec.ts:**
1. **POST placing an order for an existing basket returns orderId**
2. **POST placing an order for a non-existing basket fails**
3. **POST placing an order for a basket with a negative total cost is possible**
4. **POST placing an order for a basket with 99% discount is possible**

**In test/api/dataExportApiSpec.ts:**
5. **Export data including orders without use of CAPTCHA**
6. **Export data including orders with use of CAPTCHA**

## Test File Locations
- `test/api/basketApiSpec.ts` - Main checkout validation tests
- `test/api/dataExportApiSpec.ts` - Data export tests that use checkout

## Validation Coverage

These tests ensure that:
- ✅ Missing delivery method IDs are rejected
- ✅ Null/undefined delivery method IDs are rejected
- ✅ Non-existent delivery method IDs are rejected
- ✅ Valid delivery method IDs are accepted
- ✅ The fix prevents the free shipping exploit
- ✅ Existing functionality continues to work correctly

## Running the Tests

```bash
npm test test/api/basketApiSpec.ts
```

Or run all API tests:

```bash
npm test
```
