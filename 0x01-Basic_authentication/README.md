# Basic API Authentication

This project explores implementing basic API authentication techniques using Python and Flask. It covers error handling, authentication classes, and user validation.

## Tasks :page_with_curl:

* **1. Error Handler: Unauthorized**
  * **[api/v1/app.py](./api/v1/app.py):** Adds a custom error handler for unauthorized errors (401).
  * **[api/v1/views/index.py](./api/v1/views/index.py):** Implements an endpoint to trigger the 401 error.
  * **Usage:**
    ```bash
    curl "http://0.0.0.0:5000/api/v1/unauthorized"
    ```
    **Output:**
    ```json
    {"error": "Unauthorized"}
    ```

* **2. Error Handler: Forbidden**
  * **[api/v1/app.py](./api/v1/app.py):** Adds a custom error handler for forbidden errors (403).
  * **[api/v1/views/index.py](./api/v1/views/index.py):** Implements an endpoint to trigger the 403 error.
  * **Usage:**
    ```bash
    curl "http://0.0.0.0:5000/api/v1/forbidden"
    ```
    **Output:**
    ```json
    {"error": "Forbidden"}
    ```

* **3. Auth Class**
  * **[api/v1/auth/__init__.py](./api/v1/auth/__init__.py):** Empty file.
  * **[api/v1/auth/auth.py](./api/v1/auth/auth.py):** Defines the base `Auth` class with methods for authentication.
  * **Usage:**
    ```python
    from api.v1.auth.auth import Auth

    a = Auth()
    print(a.require_auth("/api/v1/status/", ["/api/v1/status/"]))
    print(a.authorization_header())
    print(a.current_user())
    ```
    **Output:**
    ```python
    False
    None
    None
    ```

* **4. Define Routes That Don’t Need Authentication**
  * **[api/v1/auth/auth.py](./api/v1/auth/auth.py):** Updates the `require_auth` method to exclude certain paths from authentication.
  * **Usage:**
    ```python
    from api.v1.auth.auth import Auth

    a = Auth()
    print(a.require_auth(None, None))
    print(a.require_auth("/api/v1/status/", []))
    ```
    **Output:**
    ```python
    True
    False
    ```

* **5. Request Validation**
  * **[api/v1/auth/auth.py](./api/v1/auth/auth.py):** Updates the `authorization_header` method to extract the Authorization header.
  * **[api/v1/app.py](./api/v1/app.py):** Adds request validation based on the `Auth` class.
  * **Usage:**
    ```bash
    curl "http://0.0.0.0:5000/api/v1/status"
    curl "http://0.0.0.0:5000/api/v1/users" -H "Authorization: Test"
    ```

* **6. Basic Auth**
  * **[api/v1/auth/basic_auth.py](./api/v1/auth/basic_auth.py):** Defines the `BasicAuth` class inheriting from `Auth`.
  * **[api/v1/app.py](./api/v1/app.py):** Configures the application to use `BasicAuth`.
  * **Usage:**
    ```bash
    curl "http://0.0.0.0:5000/api/v1/users" -H "Authorization: Basic Ym9iQGhidG4uaW86SDBsYmVydG9uU2Nob29sOTgh"
    ```

* **7. Basic - Base64 Part**
  * **[api/v1/auth/basic_auth.py](./api/v1/auth/basic_auth.py):** Adds `extract_base64_authorization_header` method for Base64 extraction.
  * **Usage:**
    ```python
    from api.v1.auth.basic_auth import BasicAuth

    a = BasicAuth()
    print(a.extract_base64_authorization_header("Basic SG9sYmVydG9u"))
    ```
    **Output:**
    ```python
    "SG9sYmVydG9u"
    ```

* **8. Basic - Base64 Decode**
  * **[api/v1/auth/basic_auth.py](./api/v1/auth/basic_auth.py):** Adds `decode_base64_authorization_header` method for decoding Base64 strings.
  * **Usage:**
    ```python
    from api.v1.auth.basic_auth import BasicAuth

    a = BasicAuth()
    print(a.decode_base64_authorization_header("SG9sYmVydG9u"))
    ```
    **Output:**
    ```python
    "Holberton"
    ```

* **9. Basic - User Validation**
  * **[api/v1/auth/basic_auth.py](./api/v1/auth/basic_auth.py):** Adds `user_object_from_authentication` method to validate user credentials.
  * **Usage:**
    ```python
    from api.v1.auth.basic_auth import BasicAuth

    a = BasicAuth()
    print(a.user_object_from_authentication("Basic SG9sYmVydG9uOlN0cm9uZw=="))
    ```
    **Output:**
    ```python
    {'email': 'bob@hbtn.io'}
    ```
