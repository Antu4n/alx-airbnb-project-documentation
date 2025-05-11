## 1. User Authentication

### Overview:

Handles secure registration and login for users (guests and hosts), ensuring only authorized users can access protected resources.

### API Endpoints:

| Method | Endpoint        | Description                  |
| ------ | --------------- | ---------------------------- |
| POST   | `/api/register` | Registers a new user         |
| POST   | `/api/login`    | Authenticates and logs in    |
| GET    | `/api/profile`  | Retrieves authenticated user |

---

### Input Specifications:

* **POST `/api/register`**

  ```json
  {
    "name": "Jane Doe",
    "email": "jane@example.com",
    "password": "SecurePass123",
    "role": "guest" "host"
  }
  ```

* **POST `/api/login`**

  ```json
  {
    "email": "jane@example.com",
    "password": "SecurePass123"
  }
  ```

---

### Output Specifications:

* **Success Response**

  ```json
  {
    "token": "jwt-token-here",
    "user": {
      "id": 1,
      "name": "Jane Doe",
      "email": "jane@example.com",
      "role": "guest"
    }
  }
  ```

* **Error Example**

  ```json
  {
    "error": "Invalid credentials"
  }
  ```

---

### Validation Rules:

* Email must be unique and properly formatted.
* Password must be at least 8 characters, include a number and a special character.
* Role must be either "guest" or "host".

---

### Performance Criteria:

* Response time < 500ms.
* Authentication tokens (JWT) must expire within 24 hours.
* Limit login attempts (rate-limiting) to prevent brute-force attacks.

---

## 2. Property Management

### Overview:

Hosts can create, update, and delete property listings.

### API Endpoints:

| Method | Endpoint              | Description                   |
| ------ | --------------------- | ----------------------------- |
| POST   | `/api/properties`     | Create a new property listing |
| GET    | `/api/properties`     | Retrieve all listings         |
| PUT    | `/api/properties/:id` | Update a listing              |
| DELETE | `/api/properties/:id` | Delete a listing              |

---

### Input Specifications:

* **POST `/api/properties`**

  ```json
  {
    "title": "Beachfront Villa",
    "description": "Ocean views, 3 bedrooms",
    "location": "Diani, Kenya",
    "price_per_night": 150,
    "amenities": ["WiFi", "Pool", "Air Conditioning"],
    "available_dates": ["2025-06-01", "2025-06-15"]
  }
  ```

---

### Output Specifications:

```json
{
  "id": 42,
  "title": "Beachfront Villa",
  "host_id": 1,
  "location": "Diani, Kenya",
  "status": "active"
}
```

---

### Validation Rules:

* `price_per_night` must be a positive number.
* `title` and `location` must be non-empty strings.
* `available_dates` must be an array of future dates.

---

### Performance Criteria:

* Listings should load within 700ms with pagination.
* Full-text search on `title` and `location`.
* Cache frequently accessed listings.

---

## 3. Booking System

### Overview:

Enables guests to book properties for available dates, ensuring no double bookings.

### API Endpoints:

| Method | Endpoint            | Description          |
| ------ | ------------------- | -------------------- |
| POST   | `/api/bookings`     | Create a new booking |
| GET    | `/api/bookings/:id` | View booking details |
| DELETE | `/api/bookings/:id` | Cancel a booking     |

---

### Input Specifications:

* **POST `/api/bookings`**

  ```json
  {
    "property_id": 42,
    "check_in": "2025-06-01",
    "check_out": "2025-06-05",
    "guests": 2
  }
  ```

---

### Output Specifications:

```json
{
  "booking_id": 1001,
  "status": "pending",
  "total_price": 600,
  "payment_required": true
}
```

---

### Validation Rules:

* Dates must not overlap with existing bookings.
* `check_in` must be before `check_out`.
* Number of `guests` must not exceed property capacity.

---

### Performance Criteria:

* Booking validation and creation must occur within 1 second.
* Use transactional integrity to prevent double bookings.
* Trigger payment within 200ms after booking confirmation.

