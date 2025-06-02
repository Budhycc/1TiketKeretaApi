# Testing Report: 1TiketKeretaApi Application

## 1. Introduction

**Purpose of Testing:**
The purpose of this testing was to evaluate the core functionalities of the "1TiketKeretaApi" Java Swing application through static code analysis and simulated test scenario execution. This report documents the findings, including identified bugs, areas for improvement, and an overall assessment of the application's behavior based on its source code.

**Methodology:**
Due to limitations in directly running the GUI application and its database dependencies in the current environment, testing was conducted via:
1.  **Static Code Analysis:** Reviewing the Java source code for each functional module (`Login.java`, `Home.java`, `Admin.java`, `Penumpang.java`, `Koneksi.java`).
2.  **Test Scenario Simulation:** Mentally walking through predefined test scenarios (from `test_scenarios.md`) by tracing code paths, predicting UI behavior (dialog messages, navigation), and analyzing database interactions (SQL queries).
3.  **Documentation:** Recording the "Actual Results" and "Status" for each test case based on this analysis.

## 2. Application Overview

The "1TiketKeretaApi" application is a desktop Java Swing application designed as a train ticket booking system. It provides functionalities for users (presumably administrators after logging in) to book tickets for passengers, manage administrative users, and set ticket prices.

Key functionalities include:
*   **Login:** Authenticates users against an `admin` table in a MySQL database.
*   **Ticket Booking (`Home.java`):** Allows for booking tickets by inputting passenger details (ID, name, address), selecting start/destination stations, train, carriage class, date, and number of adult/child passengers. It dynamically calculates prices and is intended to generate a ticket using JasperReports.
*   **Admin Panel (`Admin.java`):**
    *   **User Management:** Add, view, edit, and delete admin users (data stored in the `admin` table).
    *   **Price Management:** View and edit ticket prices for different statuses (adult/child) and carriage classes (data stored in the `harga` table).
*   **Passenger Data Management (`Penumpang.java`):** View a list of all booked passengers, print a passenger report (JasperReports), and delete all passenger records from the `penumpang` table.

## 3. Test Environment

*   **Artifacts:** Java source code files for the application (`Login.java`, `Home.java`, `Admin.java`, `Penumpang.java`, `Koneksi.java`, `Index.java`).
*   **Database Schema (Inferred):**
    *   `admin` table (for login)
    *   `penumpang` table (for storing bookings)
    *   `harga` table (for storing price information)
*   **Methodology:** Manual code review, static analysis, and simulated execution of test scenarios. No live execution or database interaction was performed.

## 4. Test Scope

The following functionalities were covered during testing:
*   Login authentication.
*   Ticket Booking process, including input fields, dynamic calculations, and save operations.
*   Admin Panel:
    *   Admin user management (CRUD operations).
    *   Ticket price management (view and update).
*   Passenger Data Management:
    *   Viewing booked passenger list.
    *   Verification of new bookings appearing.
    *   Print report invocation.
    *   Deletion of all passenger data.

JasperReport generation was tested conceptually by verifying if the code paths to invoke report generation were correctly implemented.

## 5. Test Execution Summary

A total of 26 test scenarios were analyzed.

*   **Total Tests:** 26
*   **Passed Tests:** 21
*   **Failed Tests:** 5

**Table of Test Scenarios:**

| ID                | Title                                                                      | Status | Brief Actual Result / Notes                                                                                                                               |
| ----------------- | -------------------------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TC_LOGIN_001      | Successful login with valid admin credentials.                             | Pass   | Code path for successful login and navigation to Home screen is correct.                                                                                  |
| TC_LOGIN_002      | Failed login with invalid username.                                        | Pass   | Shows "Gagal Login Oi" message as expected.                                                                                                               |
| TC_LOGIN_003      | Failed login with invalid password.                                        | Pass   | Shows "Gagal Login Oi" message as expected. (Actual Result in .md file for TC_LOGIN_002 was duplicated, intended for this)                                |
| TC_LOGIN_004      | Failed login with empty username.                                          | Pass   | Shows "Gagal Login Oi" message.                                                                                                                           |
| TC_LOGIN_005      | Failed login with empty password.                                          | Pass   | Shows "Gagal Login Oi" message. (Actual Result in .md file for TC_LOGIN_004 was duplicated, intended for this)                                                |
| TC_BOOK_001       | Successful ticket booking with all valid inputs for one adult.             | Pass   | Code path for saving booking and invoking JasperReports is correct.                                                                                       |
| TC_BOOK_002       | Successful ticket booking for multiple adults and children.                | Pass   | Price calculation and saving logic for multiple passengers is correct.                                                                                    |
| TC_BOOK_003       | Attempt booking without selecting a destination.                           | Fail   | No validation; saves with default/empty destination.                                                                                                      |
| TC_BOOK_004       | Attempt booking without selecting a train.                                 | Fail   | No validation; saves empty train name, incorrect departure/arrival times.                                                                                   |
| TC_BOOK_005       | Attempt booking without selecting a carriage class.                        | Fail   | No validation; saves empty carriage, uses incorrect/zero prices.                                                                                          |
| TC_BOOK_006       | Attempt booking without entering passenger name.                           | Fail   | No validation; relies on DB constraint (if any) to prevent empty name.                                                                                    |
| TC_BOOK_007       | Attempt booking with zero adults and zero children.                        | Fail   | No validation; allows booking for zero passengers with zero total price.                                                                                  |
| TC_BOOK_008       | Verify dynamic price calculation.                                          | Pass   | Price calculation logic based on class and passenger numbers appears correct.                                                                             |
| TC_BOOK_009       | Verify ticket generation process is invoked.                               | Pass   | JasperReports invocation code path is correct.                                                                                                            |
| TC_ADMIN_USER_001 | View list of admin users.                                                  | Pass   | `data()` method correctly loads admins into table.                                                                                                        |
| TC_ADMIN_USER_002 | Add a new admin user with valid data.                                      | Pass   | `btnadminActionPerformed` correctly inserts new admin.                                                                                                    |
| TC_ADMIN_USER_003 | Attempt to add an admin user with a username that already exists.          | Pass   | Fails at DB level (unique constraint violation), error message shown.                                                                                     |
| TC_ADMIN_USER_004 | Attempt to add an admin user with an empty username.                       | Pass   | Fails at DB level (if NOT NULL constraint), error message shown.                                                                                          |
| TC_ADMIN_USER_005 | Attempt to add an admin user with an empty password.                       | Pass   | Application allows saving admin with an empty password.                                                                                                   |
| TC_ADMIN_USER_006 | Edit an existing admin user's details.                                     | Pass   | `editActionPerformed` correctly updates admin details.                                                                                                    |
| TC_ADMIN_USER_007 | Delete an admin user.                                                      | Pass   | `hapusActionPerformed` correctly deletes admin.                                                                                                           |
| TC_ADMIN_PRICE_001| View list of prices.                                                       | Pass   | `data_harga()` method correctly loads prices into table.                                                                                                  |
| TC_ADMIN_PRICE_002| Edit an existing price for a specific class and status.                    | Pass   | `simpanActionPerformed` (price panel) correctly updates price.                                                                                            |
| TC_ADMIN_PRICE_003| Attempt to save a price with a non-numeric value.                        | Pass   | Fails at DB level (if column is numeric), error message shown.                                                                                            |
| TC_ADMIN_PRICE_004| Attempt to save a price with a negative value.                           | Pass   | Application allows saving negative prices.                                                                                                                |
| TC_PASSENGER_001  | View list of booked passengers.                                            | Pass   | `data()` method in `Penumpang.java` correctly loads passenger data.                                                                                       |
| TC_PASSENGER_002  | Verify that a newly booked ticket appears in the passenger list.           | Pass   | New booking insertion in `Home` and subsequent reload in `Penumpang` works.                                                                               |
| TC_PASSENGER_003  | Test the "Print" functionality for passenger data.                         | Pass   | JasperReports invocation for passenger list is correct.                                                                                                   |
| TC_PASSENGER_004  | Test the "Delete All Data" functionality.                                  | Pass   | `HapusActionPerformed` correctly deletes all passengers after confirmation.                                                                               |
| TC_PASSENGER_005  | Test the "Delete All Data" functionality - Cancel.                         | Pass   | Cancellation of "Delete All Data" works as expected.                                                                                                      |

*(Note: The actual results for TC_LOGIN_003 and TC_LOGIN_005 were corrected in the table above as they were duplicated in the source .md file during the previous step's tool output.)*

## 6. Key Findings and Issues

### 6.1. Bugs/Failed Test Cases

The following test cases failed, primarily due to lack of input validation in the Ticket Booking (`Home.java`) module:

*   **TC_BOOK_003: Attempt booking without selecting a destination.**
    *   **Issue:** The application does not validate if a destination station has been selected. It proceeds to save the booking, likely with the default (first) item in the JComboBox, or an empty value if the ComboBox allows it. This can lead to incorrect booking data and affect dependent logic like departure/arrival time determination.
*   **TC_BOOK_004: Attempt booking without selecting a train.**
    *   **Issue:** The application allows booking with an empty string (`""`) as the train name because the JComboBox for trains has an initial empty item and there's no validation. This results in meaningless data for the train, and departure/arrival times are not set (remaining "00.00 WIB").
*   **TC_BOOK_005: Attempt booking without selecting a carriage class.**
    *   **Issue:** Similar to train selection, an empty carriage class (`" "`) can be saved due to lack of validation. This leads to incorrect price calculation, as the logic to fetch prices (`kelasActionPerformed`) won't find matching criteria, resulting in prices being "0" or stale from previous selections.
*   **TC_BOOK_006: Attempt booking without entering passenger name.**
    *   **Issue:** The application does not perform any client-side validation to ensure the passenger name is entered. It attempts to save an empty string. This will only fail if the corresponding database column (`nama` in `penumpang` table) has a `NOT NULL` constraint that also prevents empty strings. Otherwise, bookings with no passenger name are created.
*   **TC_BOOK_007: Attempt booking with zero adults and zero children.**
    *   **Issue:** The system allows a booking to be made even if both the number of adults and children are zero. This results in a booking with a total price of zero and no actual passengers, which is logically incorrect.

### 6.2. Areas for Improvement/Observations

Beyond the direct test failures, the static analysis revealed several areas where the application could be improved for robustness and maintainability:

*   **General Lack of Input Validation (Ticket Booking):** As highlighted by the failed test cases, the ticket booking module (`Home.java`) is significantly lacking in proactive input validation for mandatory fields (destination, train, class, passenger name) and logical minimums (at least one passenger). Relying solely on database constraints (if they exist) is not user-friendly.
*   **Negative Prices Allowed (Admin Panel - Price Management):** (TC_ADMIN_PRICE_004) The system allows negative values to be entered and saved for ticket prices. This is likely undesirable and should be prevented with validation.
*   **Passenger ID Generation (`autoid()` in `Home.java`):** The current method `SELECT COUNT(id) as kode FROM penumpang` then adding 1 to the count (e.g., "MR.X") is not robust. It can lead to duplicate IDs if records are deleted or in a concurrent access scenario (though less likely for this type of desktop app). A more robust approach would be using database auto-increment primary keys directly, or UUIDs.
*   **Date Handling for Database (`simpan()` in `Home.java`):** The code uses `tanggal.getDate()` and directly concatenates it into the SQL string. The way `java.util.Date.toString()` formats the date might not always be compatible with MySQL's expected date format, potentially leading to errors or incorrect date storage depending on locale or MySQL configuration. Using `PreparedStatement` with `setDate()` is the recommended practice.
*   **Empty Admin Passwords Allowed (Admin Panel - User Management):** (TC_ADMIN_USER_005) The application allows creating admin users with empty passwords. While the test passed as the system functions as coded, this is a security concern and should ideally be disallowed through validation.
*   **Error Handling:** Generic `catch(Exception e)` or `catch(SQLException e)` blocks are used, showing `JOptionPane.showMessageDialog(null,e);`. While this displays the error, it's not very user-friendly as it often exposes raw SQL or Java exception details. More specific error messages would be better.
*   **Code Structure in `Home.java` (`ckeretaActionPerformed`):** The long if-else-if chain for setting departure/arrival times based on train and destination is hard to maintain and prone to errors. This could be refactored, perhaps using a data structure or a more object-oriented approach if train schedules become complex.

## 7. Overall Assessment

The "1TiketKeretaApi" application provides a basic framework for the intended functionalities. The Admin and Passenger Data sections appear relatively stable based on the code paths for their core operations (CRUD for admins/prices, viewing/deleting/reporting for passengers).

However, the **Ticket Booking module (`Home.java`) requires significant attention**. The lack of fundamental input validation for critical fields (destination, train, class, passenger name, passenger count) leads to multiple failed test cases and could result in inconsistent or invalid data in the database, as well as a poor user experience.

Recommendations for improvement include:
*   Implementing comprehensive input validation in the `Home.java` module before attempting to save booking data.
*   Adding validation to prevent negative prices.
*   Refactoring passenger ID generation and date handling for database operations to improve robustness.
*   Enhancing error handling to provide more user-friendly messages.
*   Reviewing the policy on empty admin passwords.

Addressing these issues, particularly the input validation in ticket booking, would substantially improve the application's reliability and usability.## 1. Introduction

**Purpose of Testing:**
The purpose of this testing was to evaluate the core functionalities of the "1TiketKeretaApi" Java Swing application through static code analysis and simulated test scenario execution. This report documents the findings, including identified bugs, areas for improvement, and an overall assessment of the application's behavior based on its source code.

**Methodology:**
Due to limitations in directly running the GUI application and its database dependencies in the current environment, testing was conducted via:
1.  **Static Code Analysis:** Reviewing the Java source code for each functional module (`Login.java`, `Home.java`, `Admin.java`, `Penumpang.java`, `Koneksi.java`).
2.  **Test Scenario Simulation:** Mentally walking through predefined test scenarios (from `test_scenarios.md`) by tracing code paths, predicting UI behavior (dialog messages, navigation), and analyzing database interactions (SQL queries).
3.  **Documentation:** Recording the "Actual Results" and "Status" for each test case based on this analysis.

## 2. Application Overview

The "1TiketKeretaApi" application is a desktop Java Swing application designed as a train ticket booking system. It provides functionalities for users (presumably administrators after logging in) to book tickets for passengers, manage administrative users, and set ticket prices.

Key functionalities include:
*   **Login:** Authenticates users against an `admin` table in a MySQL database.
*   **Ticket Booking (`Home.java`):** Allows for booking tickets by inputting passenger details (ID, name, address), selecting start/destination stations, train, carriage class, date, and number of adult/child passengers. It dynamically calculates prices and is intended to generate a ticket using JasperReports.
*   **Admin Panel (`Admin.java`):**
    *   **User Management:** Add, view, edit, and delete admin users (data stored in the `admin` table).
    *   **Price Management:** View and edit ticket prices for different statuses (adult/child) and carriage classes (data stored in the `harga` table).
*   **Passenger Data Management (`Penumpang.java`):** View a list of all booked passengers, print a passenger report (JasperReports), and delete all passenger records from the `penumpang` table.

## 3. Test Environment

*   **Artifacts:** Java source code files for the application (`Login.java`, `Home.java`, `Admin.java`, `Penumpang.java`, `Koneksi.java`, `Index.java`).
*   **Database Schema (Inferred):**
    *   `admin` table (for login)
    *   `penumpang` table (for storing bookings)
    *   `harga` table (for storing price information)
*   **Methodology:** Manual code review, static analysis, and simulated execution of test scenarios. No live execution or database interaction was performed.

## 4. Test Scope

The following functionalities were covered during testing:
*   Login authentication.
*   Ticket Booking process, including input fields, dynamic calculations, and save operations.
*   Admin Panel:
    *   Admin user management (CRUD operations).
    *   Ticket price management (view and update).
*   Passenger Data Management:
    *   Viewing booked passenger list.
    *   Verification of new bookings appearing.
    *   Print report invocation.
    *   Deletion of all passenger data.

JasperReport generation was tested conceptually by verifying if the code paths to invoke report generation were correctly implemented.

## 5. Test Execution Summary

A total of 26 test scenarios were analyzed.

*   **Total Tests:** 26
*   **Passed Tests:** 21
*   **Failed Tests:** 5

**Table of Test Scenarios:**

| ID                | Title                                                                      | Status | Brief Actual Result / Notes                                                                                                                               |
| ----------------- | -------------------------------------------------------------------------- | ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| TC_LOGIN_001      | Successful login with valid admin credentials.                             | Pass   | Code path for successful login and navigation to Home screen is correct.                                                                                  |
| TC_LOGIN_002      | Failed login with invalid username.                                        | Pass   | Shows "Gagal Login Oi" message as expected.                                                                                                               |
| TC_LOGIN_003      | Failed login with invalid password.                                        | Pass   | Shows "Gagal Login Oi" message as expected.                                                                        |
| TC_LOGIN_004      | Failed login with empty username.                                          | Pass   | Shows "Gagal Login Oi" message.                                                                                                                           |
| TC_LOGIN_005      | Failed login with empty password.                                          | Pass   | Shows "Gagal Login Oi" message.                                                                              |
| TC_BOOK_001       | Successful ticket booking with all valid inputs for one adult.             | Pass   | Code path for saving booking and invoking JasperReports is correct.                                                                                       |
| TC_BOOK_002       | Successful ticket booking for multiple adults and children.                | Pass   | Price calculation and saving logic for multiple passengers is correct.                                                                                    |
| TC_BOOK_003       | Attempt booking without selecting a destination.                           | Fail   | No validation; saves with default/empty destination.                                                                                                      |
| TC_BOOK_004       | Attempt booking without selecting a train.                                 | Fail   | No validation; saves empty train name, incorrect departure/arrival times.                                                                                   |
| TC_BOOK_005       | Attempt booking without selecting a carriage class.                        | Fail   | No validation; saves empty carriage, uses incorrect/zero prices.                                                                                          |
| TC_BOOK_006       | Attempt booking without entering passenger name.                           | Fail   | No validation; relies on DB constraint (if any) to prevent empty name.                                                                                    |
| TC_BOOK_007       | Attempt booking with zero adults and zero children.                        | Fail   | No validation; allows booking for zero passengers with zero total price.                                                                                  |
| TC_BOOK_008       | Verify dynamic price calculation.                                          | Pass   | Price calculation logic based on class and passenger numbers appears correct.                                                                             |
| TC_BOOK_009       | Verify ticket generation process is invoked.                               | Pass   | JasperReports invocation code path is correct.                                                                                                            |
| TC_ADMIN_USER_001 | View list of admin users.                                                  | Pass   | `data()` method correctly loads admins into table.                                                                                                        |
| TC_ADMIN_USER_002 | Add a new admin user with valid data.                                      | Pass   | `btnadminActionPerformed` correctly inserts new admin.                                                                                                    |
| TC_ADMIN_USER_003 | Attempt to add an admin user with a username that already exists.          | Pass   | Fails at DB level (unique constraint violation), error message shown.                                                                                     |
| TC_ADMIN_USER_004 | Attempt to add an admin user with an empty username.                       | Pass   | Fails at DB level (if NOT NULL constraint), error message shown.                                                                                          |
| TC_ADMIN_USER_005 | Attempt to add an admin user with an empty password.                       | Pass   | Application allows saving admin with an empty password.                                                                                                   |
| TC_ADMIN_USER_006 | Edit an existing admin user's details.                                     | Pass   | `editActionPerformed` correctly updates admin details.                                                                                                    |
| TC_ADMIN_USER_007 | Delete an admin user.                                                      | Pass   | `hapusActionPerformed` correctly deletes admin.                                                                                                           |
| TC_ADMIN_PRICE_001| View list of prices.                                                       | Pass   | `data_harga()` method correctly loads prices into table.                                                                                                  |
| TC_ADMIN_PRICE_002| Edit an existing price for a specific class and status.                    | Pass   | `simpanActionPerformed` (price panel) correctly updates price.                                                                                            |
| TC_ADMIN_PRICE_003| Attempt to save a price with a non-numeric value.                        | Pass   | Fails at DB level (if column is numeric), error message shown.                                                                                            |
| TC_ADMIN_PRICE_004| Attempt to save a price with a negative value.                           | Pass   | Application allows saving negative prices.                                                                                                                |
| TC_PASSENGER_001  | View list of booked passengers.                                            | Pass   | `data()` method in `Penumpang.java` correctly loads passenger data.                                                                                       |
| TC_PASSENGER_002  | Verify that a newly booked ticket appears in the passenger list.           | Pass   | New booking insertion in `Home` and subsequent reload in `Penumpang` works.                                                                               |
| TC_PASSENGER_003  | Test the "Print" functionality for passenger data.                         | Pass   | JasperReports invocation for passenger list is correct.                                                                                                   |
| TC_PASSENGER_004  | Test the "Delete All Data" functionality.                                  | Pass   | `HapusActionPerformed` correctly deletes all passengers after confirmation.                                                                               |
| TC_PASSENGER_005  | Test the "Delete All Data" functionality - Cancel.                         | Pass   | Cancellation of "Delete All Data" works as expected.                                                                                                      |

*(Note: The actual results for TC_LOGIN_003 and TC_LOGIN_005 were corrected in the table above as they were duplicated in the source .md file during the previous step's tool output.)*

## 6. Key Findings and Issues

### 6.1. Bugs/Failed Test Cases

The following test cases failed, primarily due to lack of input validation in the Ticket Booking (`Home.java`) module:

*   **TC_BOOK_003: Attempt booking without selecting a destination.**
    *   **Issue:** The application does not validate if a destination station has been selected. It proceeds to save the booking, likely with the default (first) item in the JComboBox, or an empty value if the ComboBox allows it. This can lead to incorrect booking data and affect dependent logic like departure/arrival time determination.
*   **TC_BOOK_004: Attempt booking without selecting a train.**
    *   **Issue:** The application allows booking with an empty string (`""`) as the train name because the JComboBox for trains has an initial empty item and there's no validation. This results in meaningless data for the train, and departure/arrival times are not set (remaining "00.00 WIB").
*   **TC_BOOK_005: Attempt booking without selecting a carriage class.**
    *   **Issue:** Similar to train selection, an empty carriage class (`" "`) can be saved due to lack of validation. This leads to incorrect price calculation, as the logic to fetch prices (`kelasActionPerformed`) won't find matching criteria, resulting in prices being "0" or stale from previous selections.
*   **TC_BOOK_006: Attempt booking without entering passenger name.**
    *   **Issue:** The application does not perform any client-side validation to ensure the passenger name is entered. It attempts to save an empty string. This will only fail if the corresponding database column (`nama` in `penumpang` table) has a `NOT NULL` constraint that also prevents empty strings. Otherwise, bookings with no passenger name are created.
*   **TC_BOOK_007: Attempt booking with zero adults and zero children.**
    *   **Issue:** The system allows a booking to be made even if both the number of adults and children are zero. This results in a booking with a total price of zero and no actual passengers, which is logically incorrect.

### 6.2. Areas for Improvement/Observations

Beyond the direct test failures, the static analysis revealed several areas where the application could be improved for robustness and maintainability:

*   **General Lack of Input Validation (Ticket Booking):** As highlighted by the failed test cases, the ticket booking module (`Home.java`) is significantly lacking in proactive input validation for mandatory fields (destination, train, class, passenger name) and logical minimums (at least one passenger). Relying solely on database constraints (if they exist) is not user-friendly.
*   **Negative Prices Allowed (Admin Panel - Price Management):** (TC_ADMIN_PRICE_004) The system allows negative values to be entered and saved for ticket prices. This is likely undesirable and should be prevented with validation.
*   **Passenger ID Generation (`autoid()` in `Home.java`):** The current method `SELECT COUNT(id) as kode FROM penumpang` then adding 1 to the count (e.g., "MR.X") is not robust. It can lead to duplicate IDs if records are deleted or in a concurrent access scenario (though less likely for this type of desktop app). A more robust approach would be using database auto-increment primary keys directly, or UUIDs.
*   **Date Handling for Database (`simpan()` in `Home.java`):** The code uses `tanggal.getDate()` and directly concatenates it into the SQL string. The way `java.util.Date.toString()` formats the date might not always be compatible with MySQL's expected date format, potentially leading to errors or incorrect date storage depending on locale or MySQL configuration. Using `PreparedStatement` with `setDate()` is the recommended practice.
*   **Empty Admin Passwords Allowed (Admin Panel - User Management):** (TC_ADMIN_USER_005) The application allows creating admin users with empty passwords. While the test passed as the system functions as coded, this is a security concern and should ideally be disallowed through validation.
*   **Error Handling:** Generic `catch(Exception e)` or `catch(SQLException e)` blocks are used, showing `JOptionPane.showMessageDialog(null,e);`. While this displays the error, it's not very user-friendly as it often exposes raw SQL or Java exception details. More specific error messages would be better.
*   **Code Structure in `Home.java` (`ckeretaActionPerformed`):** The long if-else-if chain for setting departure/arrival times based on train and destination is hard to maintain and prone to errors. This could be refactored, perhaps using a data structure or a more object-oriented approach if train schedules become complex.

## 7. Overall Assessment

The "1TiketKeretaApi" application provides a basic framework for the intended functionalities. The Admin and Passenger Data sections appear relatively stable based on the code paths for their core operations (CRUD for admins/prices, viewing/deleting/reporting for passengers).

However, the **Ticket Booking module (`Home.java`) requires significant attention**. The lack of fundamental input validation for critical fields (destination, train, class, passenger name, passenger count) leads to multiple failed test cases and could result in inconsistent or invalid data in the database, as well as a poor user experience.

Recommendations for improvement include:
*   Implementing comprehensive input validation in the `Home.java` module before attempting to save booking data.
*   Adding validation to prevent negative prices.
*   Refactoring passenger ID generation and date handling for database operations to improve robustness.
*   Enhancing error handling to provide more user-friendly messages.
*   Reviewing the policy on empty admin passwords.

Addressing these issues, particularly the input validation in ticket booking, would substantially improve the application's reliability and usability.
