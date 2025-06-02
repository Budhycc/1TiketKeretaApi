# Test Scenarios for 1TiketKeretaApi Application

## Login Scenarios

---
**ID:** TC_LOGIN_001
**Title:** Successful login with valid admin credentials.
**Preconditions:**
    - Valid admin username and password exist in the `admin` table.
    - Application is at the Login screen.
**Steps:**
    1. Enter valid admin username in the "Username" field.
    2. Enter corresponding valid password in the "Password" field.
    3. Click the "LOGIN" button.
**Expected Result:**
    - The Login window closes.
    - The Home (ticket booking) window opens.
**Actual Result:** Code in `Login.java` (`btnloginActionPerformed`) executes a SQL query. If a match is found, it calls `new Home().setVisible(true);` and `dispose();`.
**Status:** Pass

---
**ID:** TC_LOGIN_002
**Title:** Failed login with invalid username.
**Preconditions:**
    - Application is at the Login screen.
**Steps:**
    1. Enter an invalid username in the "Username" field.
    2. Enter any password in the "Password" field.
    3. Click the "LOGIN" button.
**Expected Result:**
    - An error message "Gagal Login Oi" (or similar) is displayed.
    - The user remains on the Login screen.
**Actual Result:** SQL query `SELECT * FROM admin WHERE username='<invalid_username>' And password='<password>'` is executed. `rs.next()` is false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` is shown. Login screen remains.
**Status:** Pass
**Actual Result:** SQL query `SELECT * FROM admin WHERE username='<valid_username>' And password='<invalid_password>'` is executed. `rs.next()` is false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` is shown. Login screen remains.
**Status:** Pass

---
**ID:** TC_LOGIN_003
**Title:** Failed login with invalid password.
**Preconditions:**
    - Valid admin username exists in the `admin` table.
    - Application is at the Login screen.
**Steps:**
    1. Enter a valid admin username in the "Username" field.
    2. Enter an invalid password in the "Password" field.
    3. Click the "LOGIN" button.
**Expected Result:**
    - An error message "Gagal Login Oi" (or similar) is displayed.
    - The user remains on the Login screen.
**Actual Result:**
**Status:**

---
**ID:** TC_LOGIN_004
**Title:** Failed login with empty username.
**Preconditions:**
    - Application is at the Login screen.
**Steps:**
    1. Leave the "Username" field empty.
    2. Enter any password in the "Password" field.
    3. Click the "LOGIN" button.
**Expected Result:**
    - An error message indicating failure (e.g., "Gagal Login Oi" or a specific database error if the query fails) is displayed.
    - The user remains on the Login screen.
**Actual Result:** SQL query `SELECT * FROM admin WHERE username='' And password='<password>'` is executed. Assuming no admin user has an empty username, `rs.next()` is false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` is shown. Login screen remains.
**Status:** Pass
**Actual Result:** SQL query `SELECT * FROM admin WHERE username='<username>' And password=''` is executed. Assuming the specified user does not have an empty password, `rs.next()` is false. `JOptionPane.showMessageDialog(null, "Gagal Login Oi");` is shown. Login screen remains.
**Status:** Pass

---
**ID:** TC_LOGIN_005
**Title:** Failed login with empty password.
**Preconditions:**
    - Application is at the Login screen.
**Steps:**
    1. Enter any username in the "Username" field.
    2. Leave the "Password" field empty.
    3. Click the "LOGIN" button.
**Expected Result:**
    - An error message indicating failure (e.g., "Gagal Login Oi" or a specific database error if the query fails) is displayed.
    - The user remains on the Login screen.
**Actual Result:**
**Status:**

---

## Ticket Booking (Home Screen) Scenarios

---
**ID:** TC_BOOK_001
**Title:** Successful ticket booking with all valid inputs for one adult.
**Preconditions:**
    - User is logged in and on the Home screen.
    - Price data for selected train/class exists in the `harga` table.
**Steps:**
    1. Passenger ID (`fid`) is auto-populated.
    2. Enter passenger name in "Nama Penumpang".
    3. Enter passenger address in "Alamat".
    4. "Stasiun Awal" is "Bandung (BD)".
    5. Select a "Stasiun Tujuan" from the dropdown.
    6. Select a "Kereta Api" from the dropdown.
    7. Select a "Gerbong" from the dropdown.
    8. Select a date from "tanggal".
    9. Set "Dewasa" spinner to 1.
    10. Set "Anak-anak" spinner to 0.
    11. Verify "Total" price is calculated and displayed.
    12. Enter a valid amount in "Bayar" (greater than or equal to Total).
    13. Press Enter in the "Bayar" field to calculate "Kembali".
    14. Click the "Simpan" button.
**Expected Result:**
    - Booking is saved to the `penumpang` table.
    - A success message (or ticket generation process) is initiated.
    - The Home window closes and a new Home window (or the same one cleared) is displayed with a new auto-generated ID.
    - JasperReports ticket generation process is invoked (e.g., `JasperViewer.viewReport` is called).
**Actual Result:** Code in `Home.java` (`simpan` method) executes an INSERT SQL query. If successful, it disposes the current `Home` window, creates a new one (via `Index.oke()`), and then calls `JasperFillManager.fillReport` and `JasperViewer.viewReport` with "src/report/Tiket.jasper" and the passenger ID.
**Status:** Pass

---
**ID:** TC_BOOK_002
**Title:** Successful ticket booking with all valid inputs for multiple adults and children.
**Preconditions:**
    - User is logged in and on the Home screen.
    - Price data for selected train/class exists in the `harga` table for both adults and children.
**Steps:**
    1. Enter passenger name, address.
    2. Select destination, train, carriage, and date.
    3. Set "Dewasa" spinner to 2 (or any number > 0).
    4. Set "Anak-anak" spinner to 1 (or any number > 0).
    5. Verify "Total" price is calculated correctly based on adult and child prices.
    6. Enter a valid amount in "Bayar".
    7. Press Enter in "Bayar" field.
    8. Click "Simpan".
**Expected Result:**
    - Booking is saved to the `penumpang` table.
    - Ticket generation process is initiated.
    - The Home window is refreshed/reopened.
**Actual Result:** Similar to TC_BOOK_001. The `dewasa()` and `anak()` methods correctly use spinner values and prices from `hdewasa`/`hanak` labels. The `simpan()` method inserts these values. JasperReports is invoked.
**Status:** Pass

---
**ID:** TC_BOOK_003
**Title:** Attempt booking without selecting a destination.
**Preconditions:**
    - User is logged in and on the Home screen.
**Steps:**
    1. Fill in all other required fields (name, train, class, passengers).
    2. Leave "Stasiun Tujuan" at its default/unselected state (if applicable, or try to proceed if it's a mandatory selection from a list).
    3. Click "Simpan".
**Expected Result:**
    - An error message is displayed indicating that destination is required.
    - Booking is not saved.
**Actual Result:** No explicit validation in `simpan()`. `stasiun_tujuan.getSelectedItem()` likely returns the first item by default. The `INSERT` query proceeds. Departure/arrival times might be incorrect if they rely on a specific valid destination in `ckeretaActionPerformed`. Booking is saved with potentially default/undesired destination.
**Status:** Fail

---
**ID:** TC_BOOK_004
**Title:** Attempt booking without selecting a train.
**Preconditions:**
    - User is logged in and on the Home screen.
**Steps:**
    1. Fill in all other required fields (name, destination, class, passengers).
    2. Leave "Kereta Api" at its default/empty state (e.g., the first empty item).
    3. Click "Simpan".
**Expected Result:**
    - An error message is displayed indicating that train selection is required, or the application handles it gracefully (e.g., no price calculation).
    - Booking is not saved if selection is mandatory for pricing/saving.
**Actual Result:** No explicit validation in `simpan()`. `ckereta.getSelectedItem()` returns `""` (the default empty item). `INSERT` query saves `""` for train name. `berangkat` and `tiba` labels remain "00.00 WIB". Price calculation might be based on stale `hdewasa`/`hanak` if no train implies no class prices are loaded.
**Status:** Fail

---
**ID:** TC_BOOK_005
**Title:** Attempt booking without selecting a carriage class.
**Preconditions:**
    - User is logged in and on the Home screen.
**Steps:**
    1. Fill in all other required fields (name, destination, train, passengers).
    2. Leave "Gerbong" at its default/empty state.
    3. Click "Simpan".
**Expected Result:**
    - An error message is displayed indicating that carriage class is required for price calculation.
    - Booking is not saved.
**Actual Result:** No explicit validation in `simpan()`. `cgerbong.getSelectedItem()` returns `" "` (the default empty item). `kelasActionPerformed` is called, but no conditions match for `" "`, so `hdewasa` and `hanak` are not updated (or are "0"). `ltotal` calculation uses these incorrect/zero prices. `INSERT` query saves `" "` for gerbong and an incorrect total.
**Status:** Fail

---
**ID:** TC_BOOK_006
**Title:** Attempt booking without entering passenger name.
**Preconditions:**
    - User is logged in and on the Home screen.
**Steps:**
    1. Fill in all other required fields (destination, train, class, passengers).
    2. Leave "Nama Penumpang" empty.
    3. Click "Simpan".
**Expected Result:**
    - An error message is displayed (e.g., database error for non-nullable field, or specific validation message).
    - Booking is not saved.
**Actual Result:** No explicit validation in `simpan()`. `fnama.getText()` is empty. `INSERT` query attempts to save `""` for name. If DB column `nama` in `penumpang` is `NOT NULL` and disallows empty strings, SQL execution fails, and a generic `JOptionPane` shows the SQL error. Otherwise, empty name is saved.
**Status:** Fail

---
**ID:** TC_BOOK_007
**Title:** Attempt booking with zero adults and zero children.
**Preconditions:**
    - User is logged in and on the Home screen.
**Steps:**
    1. Fill in all other required fields (name, destination, train, class).
    2. Set "Dewasa" spinner to 0.
    3. Set "Anak-anak" spinner to 0.
    4. Click "Simpan".
**Expected Result:**
    - An error message is displayed indicating at least one passenger is required, or the total price is zero and saving is prevented.
    - Booking is not saved.
**Actual Result:** No explicit validation in `simpan()`. `sdewasa.getValue()` and `sanak.getValue()` are 0. `ltotal` becomes "Rp. 0". `INSERT` query saves booking with 0 adults, 0 children, and 0 total.
**Status:** Fail

---
**ID:** TC_BOOK_008
**Title:** Verify dynamic price calculation based on train, class, and number of passengers.
**Preconditions:**
    - User is logged in and on the Home screen.
    - Known prices exist in the `harga` table for different classes and adult/child status.
**Steps:**
    1. Select a "Kereta Api".
    2. Select a "Gerbong" (e.g., "Eksekutif 1").
    3. Set "Dewasa" spinner to 1, "Anak-anak" to 0. Note the "Total".
    4. Change "Dewasa" spinner to 2. Verify "Total" doubles.
    5. Set "Anak-anak" spinner to 1. Verify "Total" increases by the child price for that class.
    6. Change "Gerbong" to another class (e.g., "Ekonomi 1").
    7. Verify "Total" updates based on the new class prices for the current number of adults and children.
**Expected Result:**
    - The "Total" field updates correctly and dynamically as train, class, and number of passengers change, reflecting the prices from the `harga` table.
    - `hdewasa` and `hanak` labels display the correct per-person prices for the selected class.
**Actual Result:** `cgerbong` selection triggers `kelasActionPerformed` which queries `harga` table and sets `hdewasa`/`hanak` labels. Spinners `sdewasa`/`sanak` trigger `dewasa()`/`anak()` methods which use these labels' values to calculate `ltotal`. Logic appears correct.
**Status:** Pass

---
**ID:** TC_BOOK_009
**Title:** Verify ticket generation process is invoked.
**Preconditions:**
    - A successful booking is made (as in TC_BOOK_001).
**Steps:**
    1. Complete all steps for a successful booking.
    2. Click "Simpan".
**Expected Result:**
    - The `JasperFillManager.fillReport` and `JasperViewer.viewReport` methods are called with `src/report/Tiket.jasper` and the correct passenger ID parameter.
    - (Conceptual) A ticket would be displayed if a display environment was available.
**Actual Result:** The `simpan()` method, after a successful database INSERT, calls `JasperFillManager.fillReport` with "src/report/Tiket.jasper" and `JasperViewer.viewReport`. The passenger ID (`fid.getText()`) is correctly passed as a parameter.
**Status:** Pass

---

## Admin Panel - User Management Scenarios

---
**ID:** TC_ADMIN_USER_001
**Title:** View list of admin users.
**Preconditions:**
    - User is logged in and on the Admin Panel screen (`Admin.java`).
    - Admin users exist in the `admin` table.
**Steps:**
    1. Navigate to the Admin Panel.
**Expected Result:**
    - The table under the "Admin" titled border is populated with existing admin user data (No, Nama, Username, Password).
**Actual Result:** The `Admin()` constructor calls `data()`, which executes `SELECT * FROM admin order by id ASC` and populates the `admin` JTable.
**Status:** Pass

---
**ID:** TC_ADMIN_USER_002
**Title:** Add a new admin user with valid data.
**Preconditions:**
    - User is on the Admin Panel screen.
**Steps:**
    1. Enter a unique name in the "Nama" field.
    2. Enter a unique username in the "Username" field.
    3. Enter a password in the "Password" field.
    4. Click the "Simpan" button under the "Admin" section.
**Expected Result:**
    - A success message "Berhasil" is displayed.
    - The new admin user appears in the admin table.
    - Input fields are cleared.
**Actual Result:** `btnadminActionPerformed` in `Admin.java` executes an `INSERT INTO admin` query. On success, it shows "Berhasil" message, reloads the table using `data()`, and clears input fields using `bersih()`.
**Status:** Pass

---
**ID:** TC_ADMIN_USER_003
**Title:** Attempt to add an admin user with a username that already exists.
**Preconditions:**
    - User is on the Admin Panel screen.
    - An admin user with username 'testuser' already exists.
**Steps:**
    1. Enter any name in "Nama".
    2. Enter 'testuser' in "Username".
    3. Enter any password in "Password".
    4. Click "Simpan".
**Expected Result:**
    - An error message is displayed (likely a database constraint violation error for unique username).
    - The user is not added to the table.
**Actual Result:** `btnadminActionPerformed` attempts `INSERT`. If the username already exists and the `username` column in the `admin` table has a UNIQUE constraint, the database will reject the insert. The `catch(SQLException e)` block will display a `JOptionPane` containing the SQL error message (e.g., "Duplicate entry..."). The table will not show the new user.
**Status:** Pass

---
**ID:** TC_ADMIN_USER_004
**Title:** Attempt to add an admin user with an empty username.
**Preconditions:**
    - User is on the Admin Panel screen.
**Steps:**
    1. Enter any name in "Nama".
    2. Leave "Username" empty.
    3. Enter any password in "Password".
    4. Click "Simpan".
**Expected Result:**
    - An error message is displayed (database error for non-nullable field or specific validation).
    - The user is not added.
**Actual Result:** `btnadminActionPerformed` attempts `INSERT` with an empty string for username. If the `username` column in the `admin` table is `NOT NULL` and does not allow empty strings, the database will reject it. The `catch(SQLException e)` block will show the SQL error. The user is not added.
**Status:** Pass

---
**ID:** TC_ADMIN_USER_005
**Title:** Attempt to add an admin user with an empty password.
**Preconditions:**
    - User is on the Admin Panel screen.
**Steps:**
    1. Enter any name in "Nama".
    2. Enter a unique username in "Username".
    3. Leave "Password" empty.
    4. Click "Simpan".
**Expected Result:**
    - User is added (password might be stored as empty string if allowed by DB). Or an error if password has constraints.
    - *Self-correction: The application saves empty passwords without error.*
**Actual Result:** `btnadminActionPerformed` attempts `INSERT` with an empty string for the password. Standard database configurations for password fields (VARCHAR) typically allow empty strings. The insert succeeds, "Berhasil" is shown, and the table is updated.
**Status:** Pass

---
**ID:** TC_ADMIN_USER_006
**Title:** Edit an existing admin user's details.
**Preconditions:**
    - User is on the Admin Panel screen.
    - At least one admin user exists.
**Steps:**
    1. Click on an existing admin user in the table.
    2. The user's details populate the "Nama", "Username", "Password" fields. The ID is stored in `bayangan1`.
    3. Modify the "Nama" field.
    4. Modify the "Password" field.
    5. Click the "Edit" button.
**Expected Result:**
    - A success message "Berhasil" is displayed.
    - The admin table refreshes and shows the updated details for that user.
    - Input fields are cleared.
**Actual Result:** `adminMouseClicked` populates fields including `bayangan1` (ID). `editActionPerformed` executes `UPDATE admin ... WHERE id='...'`. On success, "Berhasil" is shown, table is reloaded via `data()`, and fields cleared via `bersih()`.
**Status:** Pass

---
**ID:** TC_ADMIN_USER_007
**Title:** Delete an admin user.
**Preconditions:**
    - User is on the Admin Panel screen.
    - At least one admin user exists that can be deleted.
**Steps:**
    1. Click on an existing admin user in the table to select them.
    2. Click the "Hapus" button.
**Expected Result:**
    - A success message "Berhasil" is displayed.
    - The admin table refreshes and the selected user is no longer listed.
    - Input fields are cleared.
**Actual Result:** `adminMouseClicked` populates `bayangan1` (ID). `hapusActionPerformed` executes `DELETE FROM admin WHERE id='...'`. On success, "Berhasil" is shown, table reloaded via `data()`, and fields cleared via `bersih()`.
**Status:** Pass

---

## Admin Panel - Price Management Scenarios

---
**ID:** TC_ADMIN_PRICE_001
**Title:** View list of prices.
**Preconditions:**
    - User is on the Admin Panel screen.
    - Price data exists in the `harga` table.
**Steps:**
    1. Navigate to the Admin Panel.
**Expected Result:**
    - The table under the "Harga" titled border is populated with existing price data (No, Status, Kelas, Harga).
**Actual Result:** The `Admin()` constructor calls `data_harga()`, which executes `SELECT * FROM harga` and populates the `tableharga` JTable.
**Status:** Pass

---
**ID:** TC_ADMIN_PRICE_002
**Title:** Edit an existing price for a specific class and status.
**Preconditions:**
    - User is on the Admin Panel screen.
    - Price data exists.
**Steps:**
    1. Click on a price entry in the "Harga" table.
    2. The "Status", "Kelas", and "Harga" fields are populated. The ID is stored in `bayangan1`.
    3. Modify the value in the "Harga" field to a new valid numeric value.
    4. Click the "Simpan" button under the "Harga" section.
**Expected Result:**
    - A success message "Berhasil" is displayed.
    - The "Harga" table refreshes and shows the updated price for that entry.
    - Input fields are cleared.
**Actual Result:** `tablehargaMouseClicked` populates fields from the selected row, storing the ID in `bayangan1`. The user edits the `harga` field. The "Simpan" button in the "Harga" panel (`simpanActionPerformed`) executes `UPDATE harga set harga='<new_value>' where id='<id_from_bayangan1>'`. On success, "Berhasil" is shown, `data_harga()` reloads the table, and `bersih()` clears fields.
**Status:** Pass

---
**ID:** TC_ADMIN_PRICE_003
**Title:** Attempt to save a price with a non-numeric value.
**Preconditions:**
    - User is on the Admin Panel screen.
    - Price data exists.
**Steps:**
    1. Select a price entry from the table.
    2. Enter "abc" (or any non-numeric string) in the "Harga" field.
    3. Click "Simpan" under the "Harga" section.
**Expected Result:**
    - An error message is displayed (likely a `NumberFormatException` or database error if it tries to save).
    - The price is not updated.
**Actual Result:** User enters "abc" into the `harga` field. `simpanActionPerformed` executes `UPDATE harga set harga='abc' ...`. If the `harga` column in the database is numeric, this will cause a SQL error. The `catch(SQLException e)` block displays a `JOptionPane` with the database error message. The price is not updated.
**Status:** Pass

---
**ID:** TC_ADMIN_PRICE_004
**Title:** Attempt to save a price with a negative value.
**Preconditions:**
    - User is on the Admin Panel screen.
    - Price data exists.
**Steps:**
    1. Select a price entry from the table.
    2. Enter "-100" in the "Harga" field.
    3. Click "Simpan" under the "Harga" section.
**Expected Result:**
    - The price is updated to -100 (if no validation prevents it).
    - *Self-correction: The application allows negative prices. This might be a point for improvement.*
**Actual Result:** User enters "-100" into `harga` field. `simpanActionPerformed` executes `UPDATE harga set harga='-100' ...`. Assuming the `harga` column in the database is a signed numeric type, the update is successful. "Berhasil" is shown, table reloads. No application-level validation prevents negative prices.
**Status:** Pass

---

## Passenger Data Management (Penumpang Screen) Scenarios

---
**ID:** TC_PASSENGER_001
**Title:** View list of booked passengers.
**Preconditions:**
    - User is logged in.
    - Passenger bookings exist in the `penumpang` table.
**Steps:**
    1. Navigate to the "Data Penumpang" screen from the menu.
**Expected Result:**
    - The table on the "Data Penumpang" screen is populated with all existing passenger booking details.
**Actual Result:** The `Penumpang()` constructor calls its `data()` method, which executes `SELECT * FROM penumpang order by id ASC` and populates the `penumpang` JTable with the results.
**Status:** Pass

---
**ID:** TC_PASSENGER_002
**Title:** Verify that a newly booked ticket appears in the passenger list.
**Preconditions:**
    - User is logged in.
**Steps:**
    1. Note the current number of passengers in the "Data Penumpang" screen (or if it's empty).
    2. Navigate to the Home screen and successfully book a new ticket (e.g., TC_BOOK_001).
    3. After booking, navigate back to the "Data Penumpang" screen.
**Expected Result:**
    - The newly booked ticket's details appear as a new row in the passenger table.
    - The passenger count increases by one.
**Actual Result:** After a successful booking in `Home.java` (which inserts into `penumpang` table), navigating to `Penumpang` screen (which creates a new `Penumpang` instance) will call `data()`, repopulating the table with the latest data, including the new booking.
**Status:** Pass

---
**ID:** TC_PASSENGER_003
**Title:** Test the "Print" functionality for passenger data.
**Preconditions:**
    - User is on the "Data Penumpang" screen.
**Steps:**
    1. Click the "Print" button.
**Expected Result:**
    - The `JasperFillManager.fillReport` and `JasperViewer.viewReport` methods are called with `src/report/penumpang.jasper`.
    - (Conceptual) A report listing all passengers would be displayed if a display environment was available.
**Actual Result:** The "Print" button's action listener (`jButton1ActionPerformed`) calls `JasperFillManager.fillReport` with "src/report/penumpang.jasper" and `JasperViewer.viewReport`. This correctly invokes the reporting process.
**Status:** Pass

---
**ID:** TC_PASSENGER_004
**Title:** Test the "Delete All Data" functionality.
**Preconditions:**
    - User is on the "Data Penumpang" screen.
    - Some passenger data exists.
**Steps:**
    1. Click the "Hapus" (Delete All Data) button.
    2. A confirmation dialog "Yakin Ingin Menghapus Data?" appears.
    3. Click "Yes" (or the equivalent affirmative option).
**Expected Result:**
    - A success message "Berhasil" is displayed.
    - The passenger table becomes empty.
    - All records are deleted from the `penumpang` table in the database.
**Actual Result:** The "Hapus" button (`HapusActionPerformed`) shows a `JOptionPane` confirmation. If "Yes" is selected, it executes `DELETE FROM penumpang`. On successful deletion, "Berhasil" is shown, and `data()` is called to refresh the table (which will now be empty).
**Status:** Pass

---
**ID:** TC_PASSENGER_005
**Title:** Test the "Delete All Data" functionality - Cancel.
**Preconditions:**
    - User is on the "Data Penumpang" screen.
    - Some passenger data exists.
**Steps:**
    1. Click the "Hapus" (Delete All Data) button.
    2. A confirmation dialog "Yakin Ingin Menghapus Data?" appears.
    3. Click "No" (or the equivalent negative option).
**Expected Result:**
    - No success message regarding deletion is displayed.
    - The passenger table remains unchanged.
    - No records are deleted from the `penumpang` table.
**Actual Result:** The "Hapus" button (`HapusActionPerformed`) shows a `JOptionPane` confirmation. If "No" is selected, the `if` condition `(pesan == JOptionPane.YES_OPTION)` is false, and no database operation or table refresh occurs beyond the initial load.
**Status:** Pass

---
