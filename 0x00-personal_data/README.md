# Personal Data Handling

This project involves implementing various functions to handle and secure personal data. The goal is to create systems for redacting sensitive information, managing logs, and connecting to a secure database.

## Tasks :page_with_curl:

* **0. Regex-ing**
  * **[filter_datum.py](./filter_datum.py):** Write a function called `filter_datum` to obfuscate specific fields in a log message using regular expressions.
    * **Function Signature:**
      ```python
      def filter_datum(fields: List[str], redaction: str, message: str, separator: str) -> str:
      ```
    * **Details:**
      - **Arguments:**
        - `fields`: A list of strings representing all fields to obfuscate.
        - `redaction`: A string representing by what the field will be obfuscated.
        - `message`: A string representing the log line.
        - `separator`: A string representing the character separating all fields in the log line (`message`).
      - **Requirement:** Use `re.sub` to perform the substitution with a single regex.
      - **Implementation:** The function should be less than 5 lines long.
    * **Usage:**
      ```python
      filter_datum(["password", "date_of_birth"], 'xxx', "name=egg;email=eggmin@eggsample.com;password=eggcellent;date_of_birth=12/12/1986;", ';')
      ```
    * **Expected Output:**
      ```python
      name=egg;email=eggmin@eggsample.com;password=xxx;date_of_birth=xxx;
      ```

* **1. Log Formatter**
  * **[filtered_logger.py](./filtered_logger.py):** Update the `RedactingFormatter` class to filter log values based on specified fields.
    * **Class:** `RedactingFormatter`
    * **Attributes:**
      - `REDACTION`: The redaction string (e.g., "***").
      - `FORMAT`: Log format string.
      - `SEPARATOR`: Separator used in log entries.
    * **Constructor:** Accepts a list of strings `fields` as an argument.
    * **Method:** Implement `format` to filter values in log records using `filter_datum`.
    * **Usage:**
      ```python
      import logging
      from filtered_logger import RedactingFormatter

      message = "name=Bob;email=bob@dylan.com;ssn=000-123-0000;password=bobby2019;"
      log_record = logging.LogRecord("my_logger", logging.INFO, None, None, message, None, None)
      formatter = RedactingFormatter(fields=("email", "ssn", "password"))
      print(formatter.format(log_record))
      ```
    * **Expected Output:**
      ```python
      [HOLBERTON] my_logger INFO 2019-11-19 18:24:25,105: name=Bob; email=***; ssn=***; password=***;
      ```

* **2. Create Logger**
  * **[filtered_logger.py](./filtered_logger.py):** Implement a `get_logger` function that returns a configured logger.
    * **Function:** `get_logger`
    * **Logger Name:** "user_data"
    * **Log Level:** `logging.INFO`
    * **Handler:** `StreamHandler` with `RedactingFormatter` as formatter.
    * **Constant:** `PII_FIELDS` tuple containing fields from `user_data.csv` considered as PII.
    * **Usage:**
      ```python
      import logging
      from filtered_logger import get_logger, PII_FIELDS

      print(get_logger.__annotations__.get('return'))
      print("PII_FIELDS: {}".format(len(PII_FIELDS)))
      ```
    * **Expected Output:**
      ```python
      <class 'logging.Logger'>
      PII_FIELDS: 5
      ```

* **3. Connect to Secure Database**
  * **[filtered_logger.py](./filtered_logger.py):** Implement `get_db` function to connect to a MySQL database using environment variables.
    * **Function:** `get_db`
    * **Environment Variables:**
      - `PERSONAL_DATA_DB_USERNAME` (default: "root")
      - `PERSONAL_DATA_DB_PASSWORD` (default: "")
      - `PERSONAL_DATA_DB_HOST` (default: "localhost")
      - `PERSONAL_DATA_DB_NAME`
    * **Usage:**
      ```python
      from filtered_logger import get_db

      db = get_db()
      cursor = db.cursor()
      cursor.execute("SELECT COUNT(*) FROM users;")
      for row in cursor:
          print(row[0])
      cursor.close()
      db.close()
      ```
    * **Expected Output:**
      ```python
      2
      ```

* **4. Read and Filter Data**
  * **[filtered_logger.py](./filtered_logger.py):** Implement `main` function to retrieve and display filtered data from the database.
    * **Function:** `main`
    * **Filter Fields:** name, email, phone, ssn, password
    * **Usage:**
      ```python
      from filtered_logger import get_db

      def main():
          db = get_db()
          cursor = db.cursor()
          cursor.execute("SELECT * FROM users;")
          for row in cursor:
              # Filter and print the row data
              pass
          cursor.close()
          db.close()

      if __name__ == "__main__":
          main()
      ```
    * **Expected Output:**
      ```python
      [HOLBERTON] user_data INFO 2019-11-19 18:37:59,596: name=***; email=***; phone=***; ssn=***; password=***; ip=60ed:c396:2ff:244:bbd0:9208:26f2:93ea; last_login=2019-11-14 06:14:24; user_agent=Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/74.0.3729.157 Safari/537.36;
      ```

* **5. Encrypting Passwords**
  * **[encrypt_password.py](./encrypt_password.py):** Implement `hash_password` function to hash passwords using `bcrypt`.
    * **Function:** `hash_password`
    * **Library:** `bcrypt`
    * **Usage:**
      ```python
      import bcrypt
      from encrypt_password import hash_password

      password = "MyAmazingPassw0rd"
      print(hash_password(password))
      print(hash_password(password))
      ```
    * **Expected Output:**
      ```python
      b'$2b$12$Fnjf6ew.oPZtVksngJjh1.vYCnxRjPm2yt18kw6AuprMRpmhJVxJO'
      b'$2b$12$xSAw.bxfSTAlIBglPMXeL.SJnzme3Gm0E7eOEKOVV2OhqOakyUN5m'
      ```

* **6. Check Valid Password**
  * **[encrypt_password.py](./encrypt_password.py):** Implement `is_valid` function to validate if a provided password matches a hashed password.
    * **Function:** `is_valid`
    * **Library:** `bcrypt`
    * **Usage:**
      ```python
      from encrypt_password import hash_password, is_valid

      password = "MyAmazingPassw0rd"
      encrypted_password = hash_password(password)
      print(encrypted_password)
      print(is_valid(encrypted_password, password))
      ```
    * **Expected Output:**
      ```python
      b'$2b$12$Fnjf6ew.oPZtVksngJjh1.vYCnxRjPm2yt18kw6AuprMRpmhJVxJO'
      True
      ```

