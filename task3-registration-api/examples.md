# Примеры запросов и ответов API регистрации

## Обзор

Документ содержит примеры HTTP запросов и ответов для эндпоинта регистрации пользователя `POST /api/v1/users/register`.

---

## Успешный сценарий

### Запрос

```http
POST /api/v1/users/register HTTP/1.1
Host: api.example.com
Content-Type: application/json
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36
Accept: application/json

{
  "email": "john.doe@example.com",
  "password": "SecurePass123!",
  "firstName": "John",
  "lastName": "Doe"
}
```

### Ответ

```http
HTTP/1.1 201 Created
Content-Type: application/json
Location: /api/v1/users/550e8400-e29b-41d4-a716-446655440000
X-Request-ID: 123e4567-e89b-12d3-a456-426614174000

{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "john.doe@example.com",
  "firstName": "John",
  "lastName": "Doe",
  "createdAt": "2024-02-14T20:15:30Z",
  "status": "pending_verification"
}
```

---

##  Сценарии с ошибками

### 1. Ошибка валидации полей

#### Запрос

```http
POST /api/v1/users/register HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "invalid-email",
  "password": "123",
  "firstName": "",
  "lastName": "Doe"
}
```

#### Ответ

```http
HTTP/1.1 400 Bad Request
Content-Type: application/json
X-Request-ID: 123e4567-e89b-12d3-a456-426614174001

{
  "error": "VALIDATION_ERROR",
  "message": "Invalid input data",
  "details": [
    {
      "field": "email",
      "message": "Invalid email format"
    },
    {
      "field": "password",
      "message": "Password must be at least 8 characters long and contain at least one uppercase letter and one digit"
    },
    {
      "field": "firstName",
      "message": "First name is required and cannot be empty"
    }
  ]
}
```

### 2. Пользователь уже существует

#### Запрос

```http
POST /api/v1/users/register HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "existing.user@example.com",
  "password": "SecurePass123!",
  "firstName": "Jane",
  "lastName": "Smith"
}
```

#### Ответ

```http
HTTP/1.1 409 Conflict
Content-Type: application/json
X-Request-ID: 123e4567-e89b-12d3-a456-426614174002

{
  "error": "USER_ALREADY_EXISTS",
  "message": "User with this email already registered",
  "details": []
}
```

### 3. Превышен лимит запросов

#### Запрос

```http
POST /api/v1/users/register HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "SecurePass123!",
  "firstName": "Test",
  "lastName": "User"
}
```

#### Ответ

```http
HTTP/1.1 429 Too Many Requests
Content-Type: application/json
Retry-After: 300
X-Request-ID: 123e4567-e89b-12d3-a456-426614174003

{
  "error": "RATE_LIMIT_EXCEEDED",
  "message": "Too many registration attempts. Please try again later.",
  "details": [
    {
      "retryAfter": 300
    }
  ]
}
```

### 4. Временный email адрес

#### Запрос

```http
POST /api/v1/users/register HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "test@10minutemail.com",
  "password": "SecurePass123!",
  "firstName": "Test",
  "lastName": "User"
}
```

#### Ответ

```http
HTTP/1.1 422 Unprocessable Entity
Content-Type: application/json
X-Request-ID: 123e4567-e89b-12d3-a456-426614174004

{
  "error": "UNPROCESSABLE_ENTITY",
  "message": "Disposable email addresses are not allowed",
  "details": []
}
```

### 5. Внутренняя ошибка сервера

#### Запрос

```http
POST /api/v1/users/register HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "SecurePass123!",
  "firstName": "Test",
  "lastName": "User"
}
```

#### Ответ

```http
HTTP/1.1 500 Internal Server Error
Content-Type: application/json
X-Request-ID: 123e4567-e89b-12d3-a456-426614174005

{
  "error": "INTERNAL_SERVER_ERROR",
  "message": "An unexpected error occurred",
  "details": []
}
```

### 6. Сервис временно недоступен

#### Запрос

```http
POST /api/v1/users/register HTTP/1.1
Host: api.example.com
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "SecurePass123!",
  "firstName": "Test",
  "lastName": "User"
}
```

#### Ответ

```http
HTTP/1.1 503 Service Unavailable
Content-Type: application/json
Retry-After: 60
X-Request-ID: 123e4567-e89b-12d3-a456-426614174006

{
  "error": "SERVICE_UNAVAILABLE",
  "message": "Service temporarily unavailable. Please try again later.",
  "details": []
}
```

---

## Примеры использования с curl

### Успешная регистрация

```bash
curl -X POST \
  https://api.example.com/v1/users/register \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
    "email": "john.doe@example.com",
    "password": "SecurePass123!",
    "firstName": "John",
    "lastName": "Doe"
  }'
```

### Регистрация с ошибкой валидации

```bash
curl -X POST \
  https://api.example.com/v1/users/register \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json' \
  -d '{
    "email": "invalid-email",
    "password": "123",
    "firstName": "",
    "lastName": "Doe"
  }' \
  -v
```

---

## Примеры для разных языков программирования

### JavaScript (fetch)

```javascript
// Успешная регистрация
const registerUser = async () => {
  try {
    const response = await fetch('https://api.example.com/v1/users/register', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
      },
      body: JSON.stringify({
        email: 'john.doe@example.com',
        password: 'SecurePass123!',
        firstName: 'John',
        lastName: 'Doe'
      })
    });

    if (response.ok) {
      const user = await response.json();
      console.log('User registered:', user);
    } else {
      const error = await response.json();
      console.error('Registration failed:', error);
    }
  } catch (error) {
    console.error('Network error:', error);
  }
};
```

### Python (requests)

```python
import requests

def register_user():
    url = 'https://api.example.com/v1/users/register'
    data = {
        'email': 'john.doe@example.com',
        'password': 'SecurePass123!',
        'firstName': 'John',
        'lastName': 'Doe'
    }
    headers = {
        'Content-Type': 'application/json',
        'Accept': 'application/json'
    }

    try:
        response = requests.post(url, json=data, headers=headers)
        
        if response.status_code == 201:
            user = response.json()
            print(f"User registered: {user}")
        else:
            error = response.json()
            print(f"Registration failed: {error}")
            
    except requests.exceptions.RequestException as e:
        print(f"Network error: {e}")

register_user()
```

### Java (OkHttp)

```java
import okhttp3.*;

public class UserRegistration {
    private static final String API_URL = "https://api.example.com/v1/users/register";
    private static final MediaType JSON = MediaType.get("application/json; charset=utf-8");

    public void registerUser() throws IOException {
        OkHttpClient client = new OkHttpClient();
        
        String json = "{\"email\":\"john.doe@example.com\",\"password\":\"SecurePass123!\",\"firstName\":\"John\",\"lastName\":\"Doe\"}";
        
        RequestBody body = RequestBody.create(json, JSON);
        Request request = new Request.Builder()
                .url(API_URL)
                .post(body)
                .addHeader("Content-Type", "application/json")
                .addHeader("Accept", "application/json")
                .build();

        try (Response response = client.newCall(request).execute()) {
            if (response.isSuccessful()) {
                String responseBody = response.body().string();
                System.out.println("User registered: " + responseBody);
            } else {
                System.out.println("Registration failed: " + response.code() + " " + response.body().string());
            }
        }
    }
}
```

---

## Тестовые данные

### Валидные тестовые данные

```json
{
  "email": "test.user+1@example.com",
  "password": "TestPass123!",
  "firstName": "Test",
  "lastName": "User"
}
```

### Невалидные тестовые данные

```json
{
  "email": "invalid-email-format",
  "password": "short",
  "firstName": "",
  "lastName": "User123!"
}
```

---

*Примеры подготовлены для демонстрации правильного использования API и обработки различных сценариев ответов.*
